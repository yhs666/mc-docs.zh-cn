---
title: Java for Android 人脸 API 教程 | Microsoft Docs
description: 创建一个使用认知服务人脸 API 来检测和定格图像中的人脸的简单 Android 应用。
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: face
ms.topic: article
origin.date: 02/24/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 7e9b7d47c69d8d1bde02d0d64fff5f026bbc87fe
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
ms.locfileid: "23407612"
---
# <a name="getting-started-with-face-api-in-java-for-android-tutorial"></a>Java for Android 中的人脸 API 入门教程

本教程介绍如何创建和开发一个调用人脸 API 来检测图像中的人脸的简单 Android 应用程序。 该应用程序通过定格所检测到的人脸来显示结果。     

![GettingStartedAndroid](../Images/android_getstarted2.1.PNG)

## <a name="preparation"></a>准备工作

若要使用本教程，需要满足以下先决条件：

- 已安装 Android Studio 和 SDK
- Android 设备（可选，用于测试） 

## <a name="step1"></a>步骤 1：订阅人脸 API 并获取订阅密钥

在使用任何人脸 API 之前，必须在 Azure 门户中注册以订阅人脸 API。 在本教程中，可以使用主密钥和辅助密钥。

## <a name="step2"></a>步骤 2：创建应用程序框架

此步骤创建一个 Android 应用程序项目，用于实现基本 UI 来拾取和显示图像。 只需遵照以下说明操作： 

1. 打开 Android Studio。
2. 在“文件”菜单中，单击“新建项目...”
3. 将应用程序命名为 MyFirstApp，单击“下一步”。 

    ![GettingStartAndroidNewProject](../Images/AndroidNewProject.png)

4. 根据需要选择目标平台，单击“下一步”。 

    ![GettingStartAndroidNewProject2](../Images/AndroidNewProject2.png)

5. 选择“基本活动”，单击“下一步”。
6. 如下所示为活动命名，并单击“完成”。 

    ![GettingStartAndroidNewProject4](../Images/AndroidNewProject4.png)

7. 打开 activity_main.xml，应会看到此活动的布局编辑器。
8. 查看源文本文件，按如下所示编辑活动布局：           

    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
     
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:id="@+id/imageView1"
            android:layout_above="@+id/button1" />
    
        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Browse"
            android:id="@+id/button1"
            android:layout_alignParentBottom="true" />
    </RelativeLayout>
    ```  

9. 打开 MainActivity.java，在文件的开头插入以下 import 指令：           

        import java.io.*; 
        import android.app.*; 
        import android.content.*; 
        import android.net.*; 
        import android.os.*; 
        import android.view.*; 
        import android.graphics.*; 
        import android.widget.*; 
        import android.provider.*;
      
    接下来，修改“浏览”按钮逻辑的 MainActivity 类的 onCreate 方法：  

        private final int PICK_IMAGE = 1;
        private ProgressDialog detectionProgressDialog;
         
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            Button button1 = (Button)findViewById(R.id.button1);
            button1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent gallIntent = new Intent(Intent.ACTION_GET_CONTENT);
                    gallIntent.setType("image/*");
                    startActivityForResult(Intent.createChooser(gallIntent, "Select Picture"), PICK_IMAGE);
                }
        });
         
        detectionProgressDialog = new ProgressDialog(this);
        }
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
                Uri uri = data.getData();
                try {
                    Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                    ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                    imageView.setImageBitmap(bitmap);
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }  

现在，该应用可以浏览并在窗口中显示库中的照片，如下图所示：

![GettingStartAndroidUI](../Images/android_getstarted1.1.PNG)

## <a name="step3"></a>步骤 3：配置人脸 API 客户端库

人脸 API 是可以使用 HTTPS 请求调用的云 API。 为了方便在 .NET 平台应用程序中使用人脸 API，我们还提供了一个客户端库用于封装 Web 请求。 此示例使用客户端库来简化操作。 

遵照以下说明配置客户端库： 

1. 如示例中所示，从“项目”面板中找到项目的顶层 build.gradle 文件。 请注意，项目树中还有其他几个 build.gradle 文件，首先需要打开顶层的 build.gradle 文件。         
2. 将 mavenCentral() 添加到项目的存储库。 也可以使用 Android Studio 的默认存储库 jcenter()，因为 jcenter() 是 mavenCentral() 的超集。  

        allprojects {
            repositories {
                ...
                mavenCentral()
            }
        }

3. 打开“app”项目中的 build.gradle 文件。
4. 为 Maven 中心存储库中存储的客户端库添加一个依赖项：

        dependencies {  
            ...  
            compile 'com.microsoft.projectoxford:face:1.0.0'  
        }  

5. 打开“app”项目中的 MainActivity.java，并插入以下 import 指令： 
    
        import com.microsoft.projectoxford.face.\*;  
        import com.microsoft.projectoxford.face.contract.\*;  
    
   然后，在 MainActivity 类中插入以下代码：

        private FaceServiceClient faceServiceClient =  
                    new FaceServiceRestClient("your subscription key");  

   将上面的字符串替换为在步骤 1 中获取的订阅密钥。  
6. 打开“app”项目中名为 AndroidManifest.xml 的文件（在 app/src/main 目录中）。 在清单元素中插入以下元素：  

        <uses-permission android:name="android.permission.INTERNET" />  

7. 现在，可以从应用程序调用人脸 API。 

## <a name="step4"></a>步骤 4：上传图像以检测人脸

检测人脸的最简便方法是上传图像文件后直接调用[人脸 - 检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API。 使用客户端库时，可以使用 FaceServiceClient 的异步方法 DetectAsync 实现此目的。 每个返回的人脸包含一个指示人脸位置的矩形，以及一系列可选人脸属性。 在本示例中，我们只需检索人脸位置。 此处，需在 MainActivity 类中插入一个方法才能检测人脸： 

    // Detect faces by uploading face images
    // Frame faces after detection
    
    private void detectAndFrame(final Bitmap imageBitmap)
    {
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
        ByteArrayInputStream inputStream = 
            new ByteArrayInputStream(outputStream.toByteArray());
        AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0], 
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null           // returnFaceAttributes: a string like "age, gender"
                        );
                        if (result == null)
                        {
                            publishProgress("Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(
                                String.format("Detection Finished. %d face(s) detected",
                                        result.length));
                        return result;
                    } catch (Exception e) {
                        publishProgress("Detection failed");
                        return null;
                    }
                }
                @Override
                protected void onPreExecute() {
                    //TODO: show progress dialog
                }
                @Override
                protected void onProgressUpdate(String... progress) {
                    //TODO: update progress
                }
                @Override
                protected void onPostExecute(Face[] result) {
                    //TODO: update face frames
                }
            };
        detectTask.execute(inputStream);
    }

## <a name="step5"></a>步骤 5：标记图像中的人脸

最后一个步骤是将上述所有步骤组合在一起，并在图像中使用框架来标记检测到的人脸。 首先，打开 MainActivity.java，在 MainActivity.java 中插入一个帮助器方法用于绘制矩形： 

    private static Bitmap drawFaceRectanglesOnBitmap(Bitmap originalBitmap, Face[] faces) {
        Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
        Canvas canvas = new Canvas(bitmap);
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(Color.RED);
        int stokeWidth = 2;
        paint.setStrokeWidth(stokeWidth);
        if (faces != null) {
            for (Face face : faces) {
                FaceRectangle faceRectangle = face.faceRectangle;
                canvas.drawRect(
                        faceRectangle.left,
                        faceRectangle.top,
                        faceRectangle.left + faceRectangle.width,
                        faceRectangle.top + faceRectangle.height,
                        paint);
            }
        }
        return bitmap;
    }

接下来，完成 detectAndFrame 方法中的 TODO 部分，以定格人脸并报告状态。   

    @Override
    protected void onPreExecute() {
        
        detectionProgressDialog.show();
    }
    @Override
    protected void onProgressUpdate(String... progress) {
        
        detectionProgressDialog.setMessage(progress[0]);
    }
    @Override
    protected void onPostExecute(Face[] result) {
        
        detectionProgressDialog.dismiss();
        if (result == null) return;
        ImageView imageView = (ImageView)findViewById(R.id.imageView1);
        imageView.setImageBitmap(drawFaceRectanglesOnBitmap(imageBitmap, result));
        imageBitmap.recycle();
    }
 
最后，如下所示，从 onActivityResult 方法添加对 detectAndFrame 方法的调用。 （请注意，星号只是为了突出显示新添加的内容。 在尝试生成代码之前，必须删除星号。）  

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
     
                **detectAndFrame(bitmap);**
     
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

运行此应用程序并浏览包含人脸的图像。 请等待几秒钟，让云 API 做出响应。 之后，会收到下图所示的结果： 

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a>摘要

本教程已介绍人脸 API 的基本使用过程，并创建了一个应用程序来显示图像中的人脸标记。 有关人脸 API 的详细信息，请参阅操作说明和 [API 参考](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)。 

## <a name="related"></a>相关教程

- [CSharp 中的人脸 API 入门教程](FaceAPIinCSharpTutorial.md)
- [Python 中的人脸 API 入门教程](FaceAPIinPythonTutorial.md)

