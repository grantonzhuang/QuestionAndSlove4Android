# 1 at 2016/06/13

- desc
> 在 SDK 23 中，Android 官方去掉了 `HttpClient` 等包，项目中引用的 `org.apache.http` 等类库无法导入

- slove
> 在 module 层级的 `build.gradle` 文件中添加如下代码

```build.gradle
android {
    useLibrary 'org.apache.http.legacy'
}
```

- reference
    - [HttpClient won't import in Android Studio](http://stackoverflow.com/questions/32153318/httpclient-wont-import-in-android-studio)

# 0 at 2016/03/08

- desc
> 在`NestedScrollView`中，使用一个`ScrollAbleView`比如`RecyclerView`的时候，`RecyclerView`不显示。

- slove:
> 需要自定义`RecyclerView`的`LayoutManager`，以重新计算该控件的高度。

- desc:
> 但是`RecyclerView`依旧不能滑动

- slove：
> 代码如下

```java
recyclerView.setLayoutManager(new CustomLayoutManager(this));
recyclerView.setNestedScrollingEnabled(false);
recyclerView.setHasFixedSize(false);
recyclerView.setAdapter(mAdapter);
```

- reference
    - [RecyclerView和ScrollView嵌套使用](http://www.cnblogs.com/tianzhijiexian/p/4469516.html#commentform)
    - [RecyclerView inside ScrollView is not working](http://stackoverflow.com/questions/27083091/recyclerview-inside-scrollview-is-not-working)