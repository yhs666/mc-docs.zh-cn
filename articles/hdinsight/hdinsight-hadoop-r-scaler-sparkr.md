---
title: "将 ScaleR 和 SparkR 与 Azure HDInsight 配合使用 | Azure"
description: "将 ScaleR 和 SparkR 与 R Server 和 HDInsight 配合使用"
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/19/2017
ms.date: 12/25/2017
ms.author: v-yiso
ms.openlocfilehash: afc9db570100fe7866366f0cb7cc476719db3812
ms.sourcegitcommit: 25dbb1efd7ad6a3fb8b5be4c4928780e4fbe14c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2017
---
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a>在 HDInsight 中将 ScaleR 和 SparkR 配合使用

本文展示了如何使用 **ScaleR** 逻辑回归模型基于通过 **SparkR** 联接的航班延迟和天气数据来预测航班抵达延迟。 本方案演示了如何将用于在 Spark 中处理数据的 ScaleR 功能与 Microsoft R Server 配合使用来进行分析。 使用这些技术组合，可以在分布式处理中应用最新功能。

虽然这两个程序包都在 Hadoop 的 Spark 执行引擎上运行，但是会阻止它们共享内存中数据，因为它们各自需要使用其自己的 Spark 会话。 在此问题在将来的 R Server 版本中得到解决之前，解决方法是保留非重叠的 Spark 会话，并通过中间文件交换数据。 此处的说明表明这些要求很容易实现。

此处，我们使用最初由 Mario Inchiosa 和 Roni Burd 在 Strata 2016 研讨会中分享的一个示例，网络研讨会 [Building a Scalable Data Science Platform with R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio)（使用 R 构建可缩放的数据科研平台）中也提供了此示例。该示例使用 SparkR 来将知名航班的抵达延迟数据集与起飞机场和降落机场的天气数据联接起来。 然后，它将联接后的数据用作 ScaleR 逻辑回归模型的输入来预测航班抵达延迟。

我们演练的代码最初是针对在 Azure 上的 HDInsight 群集中的 Spark 上运行的 R Server 编写的。 但是，在本地环境的上下文中，在同一脚本中混合使用 SparkR 和 ScaleR 的概念仍然有效。 下面，我们假设读者对 R 以及 R Server 的 [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) 库有一个中等水平的了解。 在演练本方案时，我们还使用了 [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html)。

## <a name="the-airline-and-weather-datasets"></a>航班和天气数据集

**AirOnTime08to12CSV** 航空公司公用数据集包含美国境内从 1987 年 10 月到 2012 年 12 月所有商务航班的抵达和出发详细信息。 这是一个大型数据集：总共有大约 1.5 亿条记录， 解压缩后略小于 4 GB。 可从[美国政府存档](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236)获取该数据集。 为了方便使用，它以 zip 文件的形式 (AirOnTimeCSV.zip) 提供，其中包含 [Revolution Analytics 数据集存储库](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)中的 303 个单独的每月 CSV 文件

为了查看天气对航班延迟的影响，我们还需要获取每个机场的天气数据。 可以从[美国海洋与大气管理存储库](http://www.ncdc.noaa.gov/orders/qclcd/)下载原始格式的该数据的 zip 文件。 为了演示此示例，我们提取了 2007 年 5 月到 2012 年 12 月的天气数据，并使用了 68 个月的 zip 天气数据文件中，每个文件内的每小时数据文件。 每月 zip 文件还包含气象台 ID (WBAN)、关联的机场 (CallSign) 与机场时区与 UTC 的偏差 (TimeZone) 之间的映射 (YYYYMMstation.txt)。 联接航班延迟和天气数据时需要用到所有这些信息。

## <a name="setting-up-the-spark-environment"></a>设置 Spark 环境

第一步是设置 Spark 环境。 首先，我们指向包含输入数据目录的目录，创建 Spark 计算上下文，并创建一个日志记录函数用于在控制台上记录参考信息：

```
workDir        <- '~'  
myNameNode     <- 'default' 
myPort         <- 0
inputDataDir   <- 'wasb://hdfs@myAzureAcccount.blob.core.chinacloudapi.cn'
hdfsFS         <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# create a persistent Spark session to reduce startup times 
#   (remember to stop it later!)

sparkCC        <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort, persistentRun=TRUE)

# create working directories 

rxHadoopMakeDir('/user')
rxHadoopMakeDir('user/RevoShare')
rxHadoopMakeDir('user/RevoShare/remoteuser')

(dataDir <- '/share')
rxHadoopMakeDir(dataDir)
rxHadoopListFiles(dataDir) 

setwd(workDir)
getwd()

# version of rxRoc that runs in a local CC 
rxRoc <- function(...){
  rxSetComputeContext(RxLocalSeq())
  roc <- RevoScaleR::rxRoc(...)
  rxSetComputeContext(sparkCC)
  return(roc)
}

logmsg <- function(msg) { cat(format(Sys.time(), "%Y-%m-%d %H:%M:%S"),':',msg,'\n') } 
t0 <- proc.time() 

#..start 

logmsg('Start') 
(trackers <- system("mapred job -list-active-trackers", intern = TRUE))
logmsg(paste('Number of task nodes=',length(trackers)))
```

接下来，将“Spark_Home”添加到 R 包的搜索路径以便可以使用 SparkR，并初始化 SparkR 会话：

```
#..setup for use of SparkR  

logmsg('Initialize SparkR') 

.libPaths(c(file.path(Sys.getenv("SPARK_HOME"), "R", "lib"), .libPaths()))
library(SparkR)

sparkEnvir <- list(spark.executor.instances = '10',
                   spark.yarn.executor.memoryOverhead = '8000')

sc <- sparkR.init(
  sparkEnvir = sparkEnvir,
  sparkPackages = "com.databricks:spark-csv_2.10:1.3.0"
)

sqlContext <- sparkRSQL.init(sc)
```

## <a name="preparing-the-weather-data"></a>准备天气数据

为了准备天气数据，我们将其集合到建模所需的各个列中： 

- “Visibility”
- “DryBulbCelsius”
- “DewPointCelsius”
- “RelativeHumidity”
- “WindSpeed”
- “Altimeter”

然后，我们添加一个与气象站关联的机场代码，并将测量值从本地时间转换为 UTC。

先创建一个文件，用于将气象站 (WBAN) 信息映射到机场代码。 我们可以从随天气数据包括的映射文件中获取此关联。 通过将天气数据文件中的 *CallSign*（例如 LAX）字段映射到航班数据中的 *Origin*。 不过，我们手头恰好有另一个映射，它将 *WBAN* 映射到 *AirportID*（例如，代表 LAX 的 12892）并包括了已保存到我们可以使用的名为“wban-to-airport-id-tz.CSV” CSV 文件中的 *TimeZone*。 例如：

| AirportID | WBAN | TimeZone
|-----------|------|---------
| 10685 | 54831 | -6
| 14871 | 24232 | -8
| .. | .. | ..

以下代码将读取每小时原始天气数据文件、将文件放入我们需要的列、合并气象站映射文件、将测量值的日期时间调整为 UTC，并写出文件的新版本：

```
# Look up AirportID and Timezone for WBAN (weather station ID) and adjust time

adjustTime <- function(dataList)
{
  dataset0 <- as.data.frame(dataList)

  dataset1 <- base::merge(dataset0, wbanToAirIDAndTZDF1, by = "WBAN")

  if(nrow(dataset1) == 0) {
    dataset1 <- data.frame(
      Visibility = numeric(0),
      DryBulbCelsius = numeric(0),
      DewPointCelsius = numeric(0),
      RelativeHumidity = numeric(0),
      WindSpeed = numeric(0),
      Altimeter = numeric(0),
      AdjustedYear = numeric(0),
      AdjustedMonth = numeric(0),
      AdjustedDay = integer(0),
      AdjustedHour = integer(0),
      AirportID = integer(0)
    )

    return(dataset1)
  }

  Year <- as.integer(substr(dataset1$Date, 1, 4))
  Month <- as.integer(substr(dataset1$Date, 5, 6))
  Day <- as.integer(substr(dataset1$Date, 7, 8))

  Time <- dataset1$Time
  Hour <- ceiling(Time/100)

  Timezone <- as.integer(dataset1$TimeZone)

  adjustdate = as.POSIXlt(sprintf("%4d-%02d-%02d %02d:00:00", Year, Month, Day, Hour), tz = "UTC") + Timezone * 3600

  AdjustedYear = as.POSIXlt(adjustdate)$year + 1900
  AdjustedMonth = as.POSIXlt(adjustdate)$mon + 1
  AdjustedDay   = as.POSIXlt(adjustdate)$mday
  AdjustedHour  = as.POSIXlt(adjustdate)$hour

  AirportID = dataset1$AirportID
  Weather = dataset1[,c("Visibility", "DryBulbCelsius", "DewPointCelsius", "RelativeHumidity", "WindSpeed", "Altimeter")]

  data.set = data.frame(cbind(AdjustedYear, AdjustedMonth, AdjustedDay, AdjustedHour, AirportID, Weather))

  return(data.set)
}

wbanToAirIDAndTZDF <- read.csv("wban-to-airport-id-tz.csv")

colInfo <- list(
  WBAN = list(type="integer"),
  Date = list(type="character"),
  Time = list(type="integer"),
  Visibility = list(type="numeric"),
  DryBulbCelsius = list(type="numeric"),
  DewPointCelsius = list(type="numeric"),
  RelativeHumidity = list(type="numeric"),
  WindSpeed = list(type="numeric"),
  Altimeter = list(type="numeric")
)

weatherDF <- RxTextData(file.path(inputDataDir, "WeatherRaw"), colInfo = colInfo)

weatherDF1 <- RxTextData(file.path(inputDataDir, "Weather"), colInfo = colInfo,
                filesystem=hdfsFS)

rxSetComputeContext("localpar")
rxDataStep(weatherDF, outFile = weatherDF1, rowsPerRead = 50000, overwrite = T,
           transformFunc = adjustTime,
           transformObjects = list(wbanToAirIDAndTZDF1 = wbanToAirIDAndTZDF))
```

## <a name="importing-the-airline-and-weather-data-to-spark-dataframes"></a>将航班和天气数据导入 Spark DataFrames

现在，使用 SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) 函数将天气和航班数据导入 Spark DataFrame。 与其他许多 Spark 方法一样，此函数是惰式执行的，也就是说，它会排入执行队列，但只在需要时才会执行。

```
airPath     <- file.path(inputDataDir, "AirOnTime08to12CSV")
weatherPath <- file.path(inputDataDir, "Weather") # pre-processed weather data
rxHadoopListFiles(airPath) 
rxHadoopListFiles(weatherPath) 

# create a SparkR DataFrame for the airline data

logmsg('create a SparkR DataFrame for the airline data') 
# use inferSchema = "false" for more robust parsing
airDF <- read.df(sqlContext, airPath, source = "com.databricks.spark.csv", 
                 header = "true", inferSchema = "false")

# Create a SparkR DataFrame for the weather data

logmsg('create a SparkR DataFrame for the weather data') 
weatherDF <- read.df(sqlContext, weatherPath, source = "com.databricks.spark.csv", 
                     header = "true", inferSchema = "true")
```

## <a name="data-cleansing-and-transformation"></a>数据清理和和转换

接下来，我们对航班数据执行一些清理，然后重命名列。 我们仅保留所需的变量，并将计划的起飞时间向下舍入到最近的小时，以便与起飞时的最新天气数据合并：

```
logmsg('clean the airline data') 
airDF <- rename(airDF,
                ArrDel15 = airDF$ARR_DEL15,
                Year = airDF$YEAR,
                Month = airDF$MONTH,
                DayofMonth = airDF$DAY_OF_MONTH,
                DayOfWeek = airDF$DAY_OF_WEEK,
                Carrier = airDF$UNIQUE_CARRIER,
                OriginAirportID = airDF$ORIGIN_AIRPORT_ID,
                DestAirportID = airDF$DEST_AIRPORT_ID,
                CRSDepTime = airDF$CRS_DEP_TIME,
                CRSArrTime =  airDF$CRS_ARR_TIME
)

# Select desired columns from the flight data. 
varsToKeep <- c("ArrDel15", "Year", "Month", "DayofMonth", "DayOfWeek", "Carrier", "OriginAirportID", "DestAirportID", "CRSDepTime", "CRSArrTime")
airDF <- select(airDF, varsToKeep)

# Apply schema
coltypes(airDF) <- c("character", "integer", "integer", "integer", "integer", "character", "integer", "integer", "integer", "integer")

# Round down scheduled departure time to full hour.
airDF$CRSDepTime <- floor(airDF$CRSDepTime / 100)
```

现在，我们对天气数据执行类似的操作：

```
# Average weather readings by hour
logmsg('clean the weather data') 
weatherDF <- agg(groupBy(weatherDF, "AdjustedYear", "AdjustedMonth", "AdjustedDay", "AdjustedHour", "AirportID"), Visibility="avg",
                  DryBulbCelsius="avg", DewPointCelsius="avg", RelativeHumidity="avg", WindSpeed="avg", Altimeter="avg"
                  )

weatherDF <- rename(weatherDF,
                    Visibility = weatherDF$'avg(Visibility)',
                    DryBulbCelsius = weatherDF$'avg(DryBulbCelsius)',
                    DewPointCelsius = weatherDF$'avg(DewPointCelsius)',
                    RelativeHumidity = weatherDF$'avg(RelativeHumidity)',
                    WindSpeed = weatherDF$'avg(WindSpeed)',
                    Altimeter = weatherDF$'avg(Altimeter)'
)
```

## <a name="joining-the-weather-and-airline-data"></a>联接天气和航班数据

现在，我们使用 SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) 函数根据出发地 AirportID 和日期时间，针对航班和天气数据执行左外部联接。 使用外部联接可以保留所有航班数据记录，即使没有匹配的天气数据。 通过此联接，我们删除了一些多余的列，并将保留的列重命名以删除联接时传入的 DataFrame 前缀。

```
logmsg('Join airline data with weather at Origin Airport')
joinedDF <- SparkR::join(
  airDF,
  weatherDF,
  airDF$OriginAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF1 <- select(joinedDF, varsToKeep)

joinedDF2 <- rename(joinedDF1,
                    VisibilityOrigin = joinedDF1$Visibility,
                    DryBulbCelsiusOrigin = joinedDF1$DryBulbCelsius,
                    DewPointCelsiusOrigin = joinedDF1$DewPointCelsius,
                    RelativeHumidityOrigin = joinedDF1$RelativeHumidity,
                    WindSpeedOrigin = joinedDF1$WindSpeed,
                    AltimeterOrigin = joinedDF1$Altimeter
)
```

以类似的方式，我们基于目的地 AirportID 和日期时间联接天气和航班数据。

```
logmsg('Join airline data with weather at Destination Airport')
joinedDF3 <- SparkR::join(
  joinedDF2,
  weatherDF,
  airDF$DestAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF3)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF4 <- select(joinedDF3, varsToKeep)

joinedDF5 <- rename(joinedDF4,
                    VisibilityDest = joinedDF4$Visibility,
                    DryBulbCelsiusDest = joinedDF4$DryBulbCelsius,
                    DewPointCelsiusDest = joinedDF4$DewPointCelsius,
                    RelativeHumidityDest = joinedDF4$RelativeHumidity,
                    WindSpeedDest = joinedDF4$WindSpeed,
                    AltimeterDest = joinedDF4$Altimeter
                    )
```

## <a name="save-results-to-csv-for-exchange-with-scaler"></a>将结果保存到 CSV 以便与 ScaleR 交换

至此，我们已完成了需要通过 SparkR 执行的联接。 将数据从最终的 Spark DataFrame“joinedDF5”保存到 CSV 以便输入到 ScaleR，并关闭 SparkR 会话。 需要明确告知 SparkR 要在 80 个不同的分区中保存生成的 CSV，以便在 ScaleR 处理过程中实现足够的并行度：

```
logmsg('output the joined data from Spark to CSV') 
joinedDF5 <- repartition(joinedDF5, 80) # write.df below will produce this many CSVs

# write result to directory of CSVs
write.df(joinedDF5, file.path(dataDir, "joined5Csv"), "com.databricks.spark.csv", "overwrite", header = "true")

# We can shut down the SparkR Spark context now
sparkR.stop()

# remove non-data files
rxHadoopRemove(file.path(dataDir, "joined5Csv/_SUCCESS"))
```

## <a name="import-to-xdf-for-use-by-scaler"></a>导入到 XDF 供 ScaleR 使用

我们本可以按现样将联接后的航班和天气数据的 CSV 文件用于通过 ScaleR 文本数据源的进行建模。 但是，我们首先将其导入到 XDF 中，因为对数据集运行多个操作时，它更为高效：

```
logmsg('Import the CSV to compressed, binary XDF format') 

# set the Spark compute context for R Server 
rxSetComputeContext(sparkCC)
rxGetComputeContext()

colInfo <- list(
  ArrDel15 = list(type="numeric"),
  Year = list(type="factor"),
  Month = list(type="factor"),
  DayofMonth = list(type="factor"),
  DayOfWeek = list(type="factor"),
  Carrier = list(type="factor"),
  OriginAirportID = list(type="factor"),
  DestAirportID = list(type="factor"),
  RelativeHumidityOrigin = list(type="numeric"),
  AltimeterOrigin = list(type="numeric"),
  DryBulbCelsiusOrigin = list(type="numeric"),
  WindSpeedOrigin = list(type="numeric"),
  VisibilityOrigin = list(type="numeric"),
  DewPointCelsiusOrigin = list(type="numeric"),
  RelativeHumidityDest = list(type="numeric"),
  AltimeterDest = list(type="numeric"),
  DryBulbCelsiusDest = list(type="numeric"),
  WindSpeedDest = list(type="numeric"),
  VisibilityDest = list(type="numeric"),
  DewPointCelsiusDest = list(type="numeric")
)

joinedDF5Txt <- RxTextData(file.path(dataDir, "joined5Csv"),
                           colInfo = colInfo, fileSystem = hdfsFS)
rxGetInfo(joinedDF5Txt) 

destData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

rxImport(inData = joinedDF5Txt, destData, overwrite = TRUE)

rxGetInfo(destData, getVarInfo = T)

# File name: /user/RevoShare/dev/delayDataLarge/joined5XDF 
# Number of composite data files: 80 
# Number of observations: 148619655 
# Number of variables: 22 
# Number of blocks: 320 
# Compression type: zlib 
# Variable information: 
#   Var 1: ArrDel15, Type: numeric, Low/High: (0.0000, 1.0000)
# Var 2: Year
# 26 factor levels: 1987 1988 1989 1990 1991 ... 2008 2009 2010 2011 2012
# Var 3: Month
# 12 factor levels: 10 11 12 1 2 ... 5 6 7 8 9
# Var 4: DayofMonth
# 31 factor levels: 1 3 4 5 7 ... 29 30 2 18 31
# Var 5: DayOfWeek
# 7 factor levels: 4 6 7 1 3 2 5
# Var 6: Carrier
# 30 factor levels: PI UA US AA DL ... HA F9 YV 9E VX
# Var 7: OriginAirportID
# 374 factor levels: 15249 12264 11042 15412 13930 ... 13341 10559 14314 11711 10558
# Var 8: DestAirportID
# 378 factor levels: 13303 14492 10721 11057 13198 ... 14802 11711 11931 12899 10559
# Var 9: CRSDepTime, Type: integer, Low/High: (0, 24)
# Var 10: CRSArrTime, Type: integer, Low/High: (0, 2400)
# Var 11: RelativeHumidityOrigin, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 12: AltimeterOrigin, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 13: DryBulbCelsiusOrigin, Type: numeric, Low/High: (-46.1000, 47.8000)
# Var 14: WindSpeedOrigin, Type: numeric, Low/High: (0.0000, 81.0000)
# Var 15: VisibilityOrigin, Type: numeric, Low/High: (0.0000, 90.0000)
# Var 16: DewPointCelsiusOrigin, Type: numeric, Low/High: (-41.7000, 29.0000)
# Var 17: RelativeHumidityDest, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 18: AltimeterDest, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 19: DryBulbCelsiusDest, Type: numeric, Low/High: (-46.1000, 53.9000)
# Var 20: WindSpeedDest, Type: numeric, Low/High: (0.0000, 136.0000)
# Var 21: VisibilityDest, Type: numeric, Low/High: (0.0000, 88.0000)
# Var 22: DewPointCelsiusDest, Type: numeric, Low/High: (-43.0000, 29.0000)

finalData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

```

## <a name="splitting-data-for-training-and-test"></a>拆分数据以供训练和测试

使用 rxDataStep 拆分 2012 年的数据以供测试，保留剩余的数据以供训练：

```
# split out the training data

logmsg('split out training data as all data except year 2012')
trainDS <- RxXdfData( file.path(dataDir, "finalDataTrain" ),fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = trainDS,
            rowSelection = ( Year != 2012 ), overwrite = T )

# split out the testing data

logmsg('split out the test data for year 2012') 
testDS <- RxXdfData( file.path(dataDir, "finalDataTest" ), fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = testDS,
            rowSelection = ( Year == 2012 ), overwrite = T )

rxGetInfo(trainDS)
rxGetInfo(testDS)
```

## <a name="train-and-test-a-logistic-regression-model"></a>训练并测试逻辑回归模型

现在可以构建模型了。 为了查看天气数据对抵达时间延迟的影响，我们使用了 ScaleR 的逻辑回归例程。 我们使用它来针对大于 15 分钟的抵达延迟是否受起飞和降落机场的天气影响进行建模：

```
logmsg('train a logistic regression model for Arrival Delay > 15 minutes') 
formula <- as.formula(ArrDel15 ~ Year + Month + DayofMonth + DayOfWeek + Carrier +
                     OriginAirportID + DestAirportID + CRSDepTime + CRSArrTime + 
                     RelativeHumidityOrigin + AltimeterOrigin + DryBulbCelsiusOrigin +
                     WindSpeedOrigin + VisibilityOrigin + DewPointCelsiusOrigin + 
                     RelativeHumidityDest + AltimeterDest + DryBulbCelsiusDest +
                     WindSpeedDest + VisibilityDest + DewPointCelsiusDest
                   )

# Use the scalable rxLogit() function but set max iterations to 3 for the purposes of 
# this exercise 

logitModel <- rxLogit(formula, data = trainDS, maxIterations = 3)

base::summary(logitModel)
```

现在，让我们做出一些预测并查看 ROC 和 AUC，了解模型如何处理测试数据。

```
# Predict over test data (Logistic Regression).

logmsg('predict over the test data') 
logitPredict <- RxXdfData(file.path(dataDir, "logitPredict"), fileSystem = hdfsFS)

# Use the scalable rxPredict() function

rxPredict(logitModel, data = testDS, outData = logitPredict,
          extraVarsToWrite = c("ArrDel15"), 
          type = 'response', overwrite = TRUE)

# Calculate ROC and Area Under the Curve (AUC).

logmsg('calculate the roc and auc') 
logitRoc <- rxRoc("ArrDel15", "ArrDel15_Pred", logitPredict)
logitAuc <- rxAuc(logitRoc)
head(logitAuc)
logitAuc

plot(logitRoc)
```

## <a name="scoring-elsewhere"></a>在其他位置评分

我们还可以使用该模型在其他平台上为数据评分。 可以将数据保存到 RDS 文件，并将该 RDS 传输并导入到 SQL Server R Services 等目标评分环境。 请务必确保要评分的数据的系数级别与构建的模型上的级别匹配。 为实现此匹配，可以通过 ScaleR 的 `rxCreateColInfo()` 函数来提取并保存与建模数据关联的列信息，并将该列信息应用到输入数据源用于预测。 下面，我们将保存测试数据集的一些行，并在预测脚本中使用此示例中的列信息：

```
# save the model and a sample of the test dataset 

logmsg('save serialized version of the model and a sample of the test data')
rxSetComputeContext('localpar') 
saveRDS(logitModel, file = "logitModel.rds")
testDF <- head(testDS, 1000)  
saveRDS(testDF    , file = "testDF.rds"    )
list.files()

rxHadoopListFiles(file.path(inputDataDir,''))
rxHadoopListFiles(dataDir)

# stop the spark engine 
rxStopEngine(sparkCC) 

logmsg('Done.')
elapsed <- (proc.time() - t0)[3]
logmsg(paste('Elapsed time=',sprintf('%6.2f',elapsed),'(sec)\n\n'))
```

## <a name="summary"></a>摘要

在本文中，我们展示了如何在 Hadoop Spark 中将用于数据操作的 SparkR 和用于模型开发的 ScaleR 配合使用。 本方案要求你维护单独的 Spark 会话（一次仅运行一个会话），并通过 CSV 文件交换数据。 尽管此过程现已相当简单直接，但在将来的 R Server 版本中将会更加简单，因为到时 SparkR 和 ScaleR 可以共享 Spark 会话，因而也能共享 Spark DataFrames。

## <a name="next-steps-and-more-information"></a>后续步骤和详细信息

- 有关使用 Spark 上的 R Server 的详细信息，请参阅 [MSDN 上的入门指南](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)

- 有关 R Server 的一般信息，请参阅 [Get started with R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node)（R 入门）一文。

有关 SparkR 的用法的详细信息，请参阅：

- [Apache SparkR 文档](https://spark.apache.org/docs/2.1.0/sparkr.html)

- [SparkR Overview](https://docs.databricks.com/spark/latest/sparkr/overview.html) （SparkR 概述）

<!--Update_Description: wording update-->