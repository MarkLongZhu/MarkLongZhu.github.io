---
layout: post
title:  "启动另一个Activity(二)"
date:   2016-08-26 23:30:00
categories:  从头学Android
tags: 从头学Android
---
* content
{:toc}

> 在实际项目中，启动另一个activity有时候通常需要携带一些数据过去，那么数据是怎么传递过去的呢？

## 前提准备

##### 首先来看下项目结构

![](http://oajxivjud.bkt.clouddn.com/startActivityProjectStructure.png)

##### 编辑xml文件

在 **activity_main.xml** 中添加两个 EditText 控件用以接收用户输入的内容

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
            android:text="姓名："
            android:textSize="20sp" />

        <EditText
            android:id="@+id/et_userName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="请输入用户姓名"
            android:inputType="textPersonName" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="年龄："
            android:textSize="20sp" />

        <EditText
            android:id="@+id/et_userAge"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="请输入用户年龄"
            android:inputType="number" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:paddingTop="10dp">

        <Button
            android:id="@+id/btn_startActivity"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="登陆" />
    </LinearLayout>

</LinearLayout>
```

在 **activity_main2.xml** 中添加两个TextView控件，来接收传递过来的数据

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
            android:text="当前登录用户姓名："
            android:textSize="20sp" />

        <TextView
            android:id="@+id/tv_userName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="20sp" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="当前登录用户年龄："
            android:textSize="20sp" />

        <TextView
            android:id="@+id/tv_userAge"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textSize="20sp" />
    </LinearLayout>
</LinearLayout>

```

## 传递基础数据

##### 启动端 **MainActivity.java** 编辑

``` java

public class MainActivity extends AppCompatActivity {
    private EditText userNameEditText,userAgeEditText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
        ((Button) super.findViewById(R.id.btn_startActivity))
                .setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        Intent mIntent = new Intent(MainActivity.this,Main2Activity.class);
                        mIntent.putExtra("userName",userNameEditText.getText().toString().trim());
                        mIntent.putExtra("userAge",Integer.parseInt(userAgeEditText.getText().toString().trim()));
                        MainActivity.this.startActivity(mIntent);
                    }
                });
    }
    private void  initViews(){
        this.userNameEditText = (EditText) super.findViewById(R.id.et_userName);
        this.userAgeEditText = (EditText) super.findViewById(R.id.et_userAge);
    }
}
```

##### 接收端 **MainActivity2.java** 编辑

``` java

public class Main2Activity extends AppCompatActivity {
    private TextView userNameTextView, userAgeTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        initViews();
        setText();
    }

    private void initViews() {
        this.userNameTextView = (TextView) super.findViewById(R.id.tv_userName);
        this.userAgeTextView = (TextView) super.findViewById(R.id.tv_userAge);
    }
    private void setText(){
        Intent mIntent = getIntent();
        this.userNameTextView.setText(mIntent.getStringExtra("userName"));
        this.userAgeTextView.setText(""+mIntent.getIntExtra("userAge",0));
    }
}

```

#### 运行程序

![](http://oajxivjud.bkt.clouddn.com/startDataActivity.gif)


## 传递对象数据

传递对象数据有三种方法

* Serializable的接口
* Parcelable接口
* JSON字符串传递

#### Serializable的接口


###### 增加一个用户实体类并实现Serializable的接口

``` java

/**
 * @ClassName: UserBean
 * @Description:用户实体类
 * @author: ZHU
 * @date: 2016/8/28 17:05
 */
public class UserBean implements Serializable{
    private String userName;
    private int age;

    public void setUserName(String userName) {
        this.userName = userName;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getUserName() {
        return userName;
    }
    public int getAge() {
        return age;
    }
}
```

###### 启动端 MainActivity.java 代码

``` java

public class MainActivity extends AppCompatActivity {
    private EditText userNameEditText,userAgeEditText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
        ((Button) super.findViewById(R.id.btn_startActivity))
                .setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        Intent mIntent = new Intent(MainActivity.this,Main2Activity.class);
                        mIntent.putExtra("Data",getData());
                        MainActivity.this.startActivity(mIntent);
                    }
                });
    }
    private UserBean getData(){
        UserBean mData = new UserBean();
        mData.setUserName(userNameEditText.getText().toString().trim());
        mData.setAge(Integer.parseInt(userAgeEditText.getText().toString());
        return mData;
    }

    private void  initViews(){
        this.userNameEditText = (EditText) super.findViewById(R.id.et_userName);
        this.userAgeEditText = (EditText) super.findViewById(R.id.et_userAge);
    }
}

```

###### 接收端 Main2Activity.java 代码

``` java

public class Main2Activity extends AppCompatActivity {
    private TextView userNameTextView, userAgeTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        initViews();
        setText();
    }

    private void initViews() {
        this.userNameTextView = (TextView) super.findViewById(R.id.tv_userName);
        this.userAgeTextView = (TextView) super.findViewById(R.id.tv_userAge);
    }
    private void setText(){
        Intent mIntent = getIntent();
        UserBean mData = (UserBean)mIntent.getSerializableExtra("Data");
        this.userNameTextView.setText(mData.getUserName());
        this.userAgeTextView.setText(""+mData.getAge());
    }
}

```

#### Parcelable 接口

###### 增加一个用户实体类并实现Serializable的接口

``` java

import android.os.Parcel;
import android.os.Parcelable;
/**
 * @ClassName: UserBean
 * @Description:用户实体类
 * @author: ZHU
 * @date: 2016/8/28 17:05
 */
public class UserBean implements Parcelable {
    private String userName;
    private int age;

    public UserBean(){}

    protected UserBean(Parcel in) {
        userName = in.readString();
        age = in.readInt();
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(userName);
        dest.writeInt(age);
    }

    @Override
    public int describeContents() {
        return 0;
    }

    public static final Creator<UserBean> CREATOR = new Creator<UserBean>() {
        @Override
        public UserBean createFromParcel(Parcel in) {
            return new UserBean(in);
        }

        @Override
        public UserBean[] newArray(int size) {
            return new UserBean[size];
        }
    };

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getUserName() {
        return userName;
    }

    public int getAge() {
        return age;
    }
}

```

###### 启动端 MainActivity.java 代码

``` java

public class MainActivity extends AppCompatActivity {
    private EditText userNameEditText,userAgeEditText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
        ((Button) super.findViewById(R.id.btn_startActivity))
                .setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        Intent mIntent = new Intent(MainActivity.this,Main2Activity.class);
                        mIntent.putExtra("Data",getData());
                        MainActivity.this.startActivity(mIntent);
                    }
                });
    }
    private UserBean getData(){
        UserBean mData = new UserBean();
        mData.setUserName(userNameEditText.getText().toString().trim());
        mData.setAge(Integer.parseInt(userAgeEditText.getText().toString());
        return mData;
    }

    private void  initViews(){
        this.userNameEditText = (EditText) super.findViewById(R.id.et_userName);
        this.userAgeEditText = (EditText) super.findViewById(R.id.et_userAge);
    }
}

```

###### 接收端 Main2Activity.java 代码

``` java

public class Main2Activity extends AppCompatActivity {
    private TextView userNameTextView, userAgeTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        initViews();
        setText();
    }

    private void initViews() {
        this.userNameTextView = (TextView) super.findViewById(R.id.tv_userName);
        this.userAgeTextView = (TextView) super.findViewById(R.id.tv_userAge);
    }
    private void setText(){
        Intent mIntent = getIntent();
        UserBean mData = (UserBean)mIntent.getParcelableExtra("Data");
        this.userNameTextView.setText(mData.getUserName());
        this.userAgeTextView.setText(""+mData.getAge());
    }
}

```

#### JSON字符串传递

> 此处使用第三方开源框架GSON来实现JSON与对象的转换

在 build.gradle中引用GSON

``` xml
dependencies {
   ......
    compile 'com.google.code.gson:gson:2.2.4'
   ......
}

```


###### 增加一个用户实体类

``` java

 /**
 * @ClassName: UserBean
 * @Description:用户实体类
 * @author: ZHU
 * @date: 2016/8/28 17:05
 */
public class UserBean {
    private String userName;
    private int age;

    public void setUserName(String userName) {
        this.userName = userName;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getUserName() {
        return userName;
    }
    public int getAge() {
        return age;
    }
}

```

###### 启动端 MainActivity.java 代码

``` java

public class MainActivity extends AppCompatActivity {
    private EditText userNameEditText,userAgeEditText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViews();
        ((Button) super.findViewById(R.id.btn_startActivity))
                .setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        Intent mIntent = new Intent(MainActivity.this,Main2Activity.class);
                        String jsonData = new Gson().toJson(getData());
                        mIntent.putExtra("jsonData",jsonData);
                        MainActivity.this.startActivity(mIntent);
                    }
                });
    }
    private UserBean getData(){
        UserBean mData = new UserBean();
        mData.setUserName(userNameEditText.getText().toString().trim());
        mData.setAge(Integer.parseInt(userAgeEditText.getText().toString()));
        return mData;
    }

    private void  initViews(){
        this.userNameEditText = (EditText) super.findViewById(R.id.et_userName);
        this.userAgeEditText = (EditText) super.findViewById(R.id.et_userAge);
    }
}
```

###### 接收端 Main2Activity.java 代码

``` java

public class Main2Activity extends AppCompatActivity {
    private TextView userNameTextView, userAgeTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        initViews();
        setText();
    }

    private void initViews() {
        this.userNameTextView = (TextView) super.findViewById(R.id.tv_userName);
        this.userAgeTextView = (TextView) super.findViewById(R.id.tv_userAge);
    }
    private void setText(){
        Intent mIntent = getIntent();
        UserBean mData = new Gson().fromJson(mIntent.getStringExtra("jsonData"),UserBean.class);
        this.userNameTextView.setText(mData.getUserName());
        this.userAgeTextView.setText(""+mData.getAge());
    }
}

```

#### 总结

在这几种方法中 **转换为字符串** 的速度是最慢的。**Seralizable** 次之，**Parcelable** 比 **Seralizable** 快10倍。所以从性能上考虑，建议优先选择Parcelable。但是Parcelable相比其他，需要编写大量重复的模板代码。但是可以通过第三方类库的形式减化操作。





