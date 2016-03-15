---
layout: post
title: 安卓开发：图片全屏显示 Demo
---
### 本文转载自 [fishorbird 博客](http://blog.csdn.net/fishorbird/article/details/49472633)。

这个 Demo 可以从手机内存中读取一张图片，在一个全屏的 activity 中显示。

## Java 代码

```java

import java.io.File;
import android.app.Activity;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Matrix;
import android.os.Bundle;
import android.os.Environment;
import android.util.DisplayMetrics;
import android.view.Window;
import android.view.WindowManager;
import android.widget.ImageView;

public class MainActivity extends Activity {
	ImageView img;
	String path;
	String filepath;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
				WindowManager.LayoutParams.FLAG_FULLSCREEN);//不显示标题栏
		setContentView(R.layout.activity_main);
		img = (ImageView) findViewById(R.id.imageView1);
		File sd = Environment.getExternalStorageDirectory();
		path = sd.getPath();//获得手机内存storage的位置
		filepath = path + "/picturereceive/beautiful2.jpg";//storage下需要全屏显示的图片路径（要根据自己手机中需要显示图片路径位置进行修改）
		File file = new File(filepath);
		System.out.println("<<<<<<<<<<<<<<<<<<<<<<<<"+filepath);
		if (file.exists()) {
			System.out.println("<<<<<<<<<<<<<<<<<<<<<<<4");
			Bitmap bm = BitmapFactory.decodeFile(filepath);//获得设置路径下图片并编码为Bitmap格式
			bm = big(bm);//放大图片至全屏
			System.out.println("<<<<<<<<<<<<<<<<<5");
			img.setImageBitmap(bm);//设置图片为背景图
		}
		else {
			System.err.println("<<<<<<<<<<<<<Not Found");//控制台输出没找到图片
		}
	}

	public Bitmap big(Bitmap bitmap) {  				//修改bitmap大小
		DisplayMetrics dm = new DisplayMetrics();
		getWindowManager().getDefaultDisplay().getMetrics(dm);  
		int screenWidth = dm.widthPixels;				//获取当前屏幕宽度  
		int screenHeight = dm.heightPixels;				//获取当前屏幕高度
		float w = (float) screenWidth / bitmap.getWidth();	//计算当前图片要全屏幕，宽度需要放大尺寸
		float h = (float) screenHeight / bitmap.getHeight();//计算当前图片要全屏，高度需要放大尺寸
		if (w >= h)//选取较小尺寸进行放大
			w = h;
		Matrix matrix = new Matrix();
		matrix.postScale(w, w);//设置宽高放大比例（这里为等比例放大）
		Bitmap resizeBmp = Bitmap.createBitmap(bitmap, 0, 0, bitmap.getWidth(),
				bitmap.getHeight(), matrix, true);//对现有bitmap进行放大
		return resizeBmp;
	}
}

```

## XML布局文件代码

```xml

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#000000"//设置背景为黑色
    tools:context=".MainActivity" >

    <ImageView
        android:id="@+id/imageView1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"
        android:src="@drawable/ic_launcher" />

</RelativeLayout>

```

## 注意

注意一定要加上手机读内存权限，在manifest文件中写入 <br>
`<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>`

## Demo 下载

[下载地址](http://download.csdn.net/detail/fishorbird/9221317)
