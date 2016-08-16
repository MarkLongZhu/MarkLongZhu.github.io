---
layout: post
title:  "启动另一个Activity"
date:   2016-07-22 22:14:54
categories:  从头学Android
tags: 从头学Android
---
* content
{:toc}


> 
在 [之前](https://marklongzhu.github.io/2016/07/22/%E4%BB%8E%E5%A4%B4%E5%AD%A6Andorid%E4%B9%8B%E5%90%AF%E5%8A%A8%E7%AC%AC%E4%B8%80%E4%B8%AAActivity/) 的文章里我们已经学会了怎么启动一个activity了，那么今天我们就来学习一下如何启动另一个activity。

首先我们需要新建一个项目，还不太熟悉怎么新建项目的同学可以看下 [启动一个Activity](https://marklongzhu.github.io/2016/07/22/%E4%BB%8E%E5%A4%B4%E5%AD%A6Andorid%E4%B9%8B%E5%90%AF%E5%8A%A8%E7%AC%AC%E4%B8%80%E4%B8%AAActivity/)

 新建项目操作之后，会出现如下所示的项目结构

![](http://oajxivjud.bkt.clouddn.com/1.projectStructure.png)

 然后我们在默认包名上，右键菜单新建一个activity

![](http://oajxivjud.bkt.clouddn.com/2.newActivity.png)

![](http://oajxivjud.bkt.clouddn.com/3.newActivity.png)


 从现在的项目结构中，可以很明显地发现在 ”com.marklogzhu.learn.myapplication“ 包和 "res/layout" 目录下分别新增加了一个 **Main2Activity.java** 和 **activity_main2.xml** 文件

![](http://oajxivjud.bkt.clouddn.com/4.newActivity.png)


如果你这个时候到 **”manifests/AndroidManifest.xml“** 文件中看下，会发现多出了一行
代码
``` xml
		<activity android:name=".Main2Activity"></activity>
```
这行代码的主要作用就是告诉应用，一个新的activity的诞生。

新的Activity也建好了，接下来我们就要进入今天的主题了。

首先在 **activity_main.xml**文件中拉进来一个按钮，它的作用就为了单击时启动第二个activity
``` xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context="com.marklogzhu.learn.myapplication.MainActivity">

        <Button
            android:id="@+id/btn_startActivity"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="启动第二个Activity" />
    </LinearLayout>
```
同时为了区分两个Activity,我们在 **activity_main2.xml** 文件中增加一个文本，修改之后内容如下：

``` xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:paddingTop="30dp"
        android:text="当前是Main2Activity"
        android:textSize="20sp" />

</LinearLayout>
```


在 **MainActivity.java** 中实例化按钮并添加单击事件
``` java
public class MainActivity extends AppCompatActivity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            ((Button) super.findViewById(R.id.btn_startActivity))
                    .setOnClickListener(new View.OnClickListener() {
                        @Override
                        public void onClick(View v) {
                            Intent mIntent = new Intent(MainActivity.this,Main2Activity.class);
                            MainActivity.this.startActivity(mIntent);
                        }
                    });
        }
    }
```

##### 运行结果：

<iframe height=680 width=400 src="http://oajxivjud.bkt.clouddn.com/startActivity.gif"></iframe >








