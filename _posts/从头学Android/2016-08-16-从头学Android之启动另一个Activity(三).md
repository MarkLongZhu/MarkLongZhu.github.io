---
layout: post
title:  "启动另一个Activity(三)"
date:   2016-08-30 21:39:00
categories:  从头学Android
tags: 从头学Android
---
* content
{:toc}

> 在之前我们介绍了如何传递数据到下一个Activity中去，那么怎么回传数据到上一个Activity呢

## 代码编写

##### activity_main.xml

``` xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="10dp"
    tools:context="com.marklogzhu.learn.myapplication.MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="返回的数据："
            android:textSize="20sp" />

        <TextView
            android:id="@+id/tv_sendData"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="20sp" />
    </LinearLayout>


    <Button
        android:id="@+id/btn_start"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="启动Activity" />

</LinearLayout>

```

##### MainActivity.java

``` java

package com.marklogzhu.learn.myapplication;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    private TextView sendDataTextView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
        ((Button) super.findViewById(R.id.btn_start))
                .setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        Intent mIntent = new Intent(MainActivity.this,Main2Activity.class);
                        MainActivity.this.startActivityForResult(mIntent,1);
                    }
                });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        String dataS = data.getStringExtra("data");
        this.sendDataTextView.setText(dataS);
    }

    private void  initViews(){
        this.sendDataTextView = (TextView) super.findViewById(R.id.tv_sendData);
    }
}




```



##### activity_main2.xml

``` xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="发送的数据："
            android:textSize="20sp" />

        <EditText
            android:id="@+id/et_sendData"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="20sp" />
    </LinearLayout>

    <Button
        android:id="@+id/btn_return"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="返回上一个Activity" />

</LinearLayout>


```

##### MainActivity.java

``` java

package com.marklogzhu.learn.myapplication;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class Main2Activity extends AppCompatActivity {
    private EditText sendDataEditText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        initViews();
        ((Button)super.findViewById(R.id.btn_return)).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent mIntent = new Intent();
                mIntent.putExtra("data",sendDataEditText.getText().toString().trim());
                Main2Activity.this.setResult(RESULT_OK, mIntent);
                Main2Activity.this.finish();
            }
        });
    }

    private void initViews() {
        this.sendDataEditText = (EditText) super.findViewById(R.id.et_sendData);
    }
}


```



## 运行效果

![](http://oajxivjud.bkt.clouddn.com/returnDataActivity.gif)
