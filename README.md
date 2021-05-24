# NotePad

## 一、软件定位

This is an AndroidStudio rebuild of google SDK sample NotePad

NotePad是一个基于android操作系统的记事本软件

## 二、使用环境

Android 3.5.2

SDK Version 30

Gradle Version 5.4.1

**注意：若要使用该软件需要在项目导入后，更改project structure 中的相应版本。**

## 三、代码结构说明

![structure](https://github.com/floweral/Notepad/blob/master/screenshot/structure.png)



一共包含了6个类，其中4个Activity，一个ContentProvider，还有一个数据契约类。

NotesList 应用程序的入口，笔记本的首页面会显示笔记的列表
NoteEditor 编辑笔记内容的Activity
TitleEditor 编辑笔记标题的Activity
NotesLiveFolder ContentProvider的LiveFolder（实时文件夹），这个功能在Android API 14后被废弃，不再支持。因此代码中所有涉及LiveFolder的内容将不再阐述。
NotePadProvider 这是笔记本应用的ContentProvider，也是整个应用的关键所在



原文链接：https://blog.csdn.net/llfjfz/article/details/67638499



## 四、添加时间戳

1、在notelist_item.xml布局文件中多添加一个

```xml
<TextView  
       android:id="@android:id/text2"  
       android:layout_width="match_parent"  
       android:layout_height="wrap_content"  
       android:textAppearance="?android:attr/textAppearanceLarge"  
       android:gravity="center_vertical"  
       android:paddingLeft="5dp"  
       android:singleLine="true" />  
```

2、在NoteList类的PROJECTION中添加

源代码：

```java
private static final String[] PROJECTION = new String[] {
        NotePad.Notes._ID, // 0
        NotePad.Notes.COLUMN_NAME_TITLE, // 1
};
```

添加：

```java
NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,
```

  

3、将NoteList类中的dataColumns修改为

```
String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE };
```



4、在NoteEditor类的updateNote方法中获取当前系统的时间，并对时间进行格式化

   

```java
	  // Sets up a map to contain values to be updated in the provider.   
      ContentValues values = new ContentValues();  
      // 转化时间格式
      Long now = Long.valueOf(System.currentTimeMillis());  
      SimpleDateFormat sf = new SimpleDateFormat("yy/MM/dd HH:mm");  
      Date d = new Date(now);  
      String format = sf.format(d);  
      values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, format);
```



## 五、运行结果

![Home_page](https://github.com/floweral/Notepad/blob/master/screenshot/Home_page.png)

运行后的主界面如图所示



![1](https://github.com/floweral/Notepad/blob/master/screenshot/1.png)

主界面功能

![2](https://github.com/floweral/Notepad/blob/master/screenshot/2.png)

编写记事本内容的界面



![3](https://github.com/floweral/Notepad/blob/master/screenshot/3.png)

时间戳

![4](https://github.com/floweral/Notepad/blob/master/screenshot/4.png)

还实现了搜索框功能，可以进行关键字匹对	

