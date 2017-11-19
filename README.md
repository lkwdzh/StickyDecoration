# StickyDecoration
利用`RecyclerView.ItemDecoration`实现顶部悬浮效果

![效果](http://upload-images.jianshu.io/upload_images/1638147-89986d7141741cdf.gif?imageMogr2/auto-orient/strip)

## 添加依赖
项目要求： `minSdkVersion` >= 14.
在你的`build.gradle`中 :
```gradle
repositories {
    jcenter()// If not already there
}
dependencies {
    compile 'com.gavin.com.library:stickyDecoration:1.1.2'
}
```

## 支持
- **LinearLayoutManager**
- **GridLayoutManager**
- **点击事件**
- **分割线**

## 使用

- `StickyDecoration`——文字悬浮
```java
//回调
GroupListener groupListener = new GroupListener() {
    @Override
    public String getGroupName(int position) {
        //根据position获取对应的组名称
        return dataList.get(position).getProvince();
    }
};
//创建StickyDecoration，实现悬浮栏
StickyDecoration decoration = StickyDecoration.Builder
        .init(groupListener)
        .setGroupBackground(Color.parseColor("#48BDFF"))  //背景色（默认 透明）
        .setGroupHeight(DensityUtil.dip2px(this, 35))     //高度
        .setDivideColor(Color.parseColor("#CCCCCC"))      //分割线颜色（默认 #CCCCCC）
        .setDivideHeight(DensityUtil.dip2px(this, 1))     //分割线高宽度 (默认 0)
        .setGroupTextColor(Color.BLACK)                   //字体颜色 （默认 Color.WHITE）
        .setGroupTextSize(DensityUtil.sp2px(this, 15))    //字体大小 （默认 40）
        .setTextSideMargin(DensityUtil.dip2px(this, 10))  //边距   靠左时为左边距  靠右时为右边距（默认 10）
        .isAlignLeft(false)                               //靠右显示  （默认 靠左）
        .setOnClickListener(new OnGroupClickListener() {  //点击事件，返回当前分组下第一个item的position
            @Override
            public void onClick(int position) {
                //处理点击事件
            }
        })
        .build();
...
mRecyclerView.addItemDecoration(decoration);
```
效果：

![效果](http://upload-images.jianshu.io/upload_images/1638147-f3c2cbe712aa65fb.gif?imageMogr2/auto-orient/strip)



- `PowerfulStickyDecoration`——自定义`View`悬浮

先创建布局`item_group`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:background="@color/colorAccent"
              android:gravity="center_vertical"
              android:id="@+id/ll"
              android:orientation="horizontal">

    <ImageView
        android:id="@+id/iv"
        android:layout_width="30dp"
        android:layout_height="30dp"
        android:layout_marginLeft="10dp"
        android:src="@mipmap/ic_launcher"/>

    <TextView
        android:id="@+id/tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="15sp"
        android:textColor="@android:color/white"/>
</LinearLayout>
```
创建`PowerfulStickyDecoration`，实现自定`View`悬浮
```java
PowerGroupListener listener = new PowerGroupListener() {
    @Override
    public String getGroupName(int position) {
        return dataList.get(position).getProvince();
    }

    @Override
    public View getGroupView(int position) {
        //获取自定定义的组View
        View view = getLayoutInflater().inflate(R.layout.item_group, null, false);
        ((TextView) view.findViewById(R.id.tv)).setText(dataList.get(position).getProvince());
        return view;
    }
};
PowerfulStickyDecoration decoration = PowerfulStickyDecoration.Builder
        .init(listener)
        .setGroupHeight(DensityUtil.dip2px(this, 40))       //设置高度
        .setGroupBackground(Color.parseColor("#48BDFF"))    //设置背景  （默认 透明）
        .setDivideColor(Color.parseColor("#CCCCCC"))        //分割线颜色（默认 #CCCCCC）
        .setDivideHeight(DensityUtil.dip2px(this, 1))       //分割线高宽度 (默认 0)
        .isAlignLeft(false)                                 //靠右显示  （默认 靠左）
        .setOnClickListener(new OnGroupClickListener() {    //点击事件，返回当前分组下第一个item的position
            @Override
            public void onClick(int position) {
                //处理点击事件
            }
        })
        .build();

  ...
mRecyclerView.addItemDecoration(decoration);
```
效果：

![效果](http://upload-images.jianshu.io/upload_images/1638147-3fed255296a6c3db.gif?imageMogr2/auto-orient/strip)
