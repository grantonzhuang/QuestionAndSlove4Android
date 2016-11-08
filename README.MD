# 5 at 2016/11/08

- desc

> 使用 intent 调用 Android 的相机拍照的时候，返回的 uri 为 null。

```java
Intent cameraIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
startActivityForResult(cameraIntent, 1000);

@Override
public void onActivityResult(int requestCode, int resultCode, Intent data) {
    Uri uri = data.getData();
    // uri is null sometimes
}
```

- reslove

> 换成 getExtras() 方法可以获得正确的数据，如果要获取文件的 uri，可以先在 intent 中指定 uri。参考[官方文档](https://developer.android.com/training/camera/photobasics.html#TaskCaptureIntent)

```java
@Override
public void onActivityResult(int requestCode, int resultCode, Intent data) {
    Bundle bundle = data.getExtras();
    Bitmap bitmap = bundle.get("data");
}
```

# 4 at 2016/11/07

- desc

> 在使用 CollapsingToolbarLayout 包含一个 ToolBar，实现一个 “收缩设置标题，展开隐藏标题” 的需求的时候，收缩和展开的时候，出现了如下警告

```
requestLayout() improperly called by android.support.design.widget.CollapsingToolbarLayout
```

- why

代码如下，这个监听会一直调用，如果不加上判断，会一直要求重新绘制 CollapsingToolbarLayout 或者 ToolBar，所以出现警告

```java
@Override
public void onOffsetChanged(AppBarLayout appBarLayout, int verticalOffset) {
    int maxScroll = appBarLayout.getTotalScrollRange();
    float percentage = (float) Math.abs(verticalOffset) / (float) maxScroll;
    if (percentage == 1) {
        if (getSupportActionBar() == null) {
            return;
        }
        getSupportActionBar().setTitle(R.string.title);
    } else {
        collapsingToolbarLayout.setTitle("");
    }
}
```

- reslove 加上对标题是否非空的判断即可

```java
if (percentage == 1) {
    if (getSupportActionBar() == null) {
        return;
    }
    if (TextUtils.isEmpty(collapsingToolbarLayout.getTitle())) {
        getSupportActionBar().setTitle(R.string.title);
    }
} else {
    if (!TextUtils.isEmpty(collapsingToolbarLayout.getTitle())) {
        collapsingToolbarLayout.setTitle("");
    }
}
```

# 3 at 2016/07/30

- desc

> 在 recyclerView 中使用 swtich 控件的时候，遇到 `Recycleview notifyDataSetChanged() IllegalStateException`

- why

> You should move method 'setOnCheckedChangeListener()' to ViewHolder which is inner class on your adapter.

> onBindViewHolder() is not a method that initialize ViewHolder. This method is step of refresh each recycler item. When you call notifyDataSetChanged(), onBindViewHolder() will be called as the number of each item times.

> So If you notifyDataSetChanged() put into onCheckChanged() and initialize checkBox in onBindViewHolder(), you will get IllegalStateException because of circular method call.

> click checkbox -> onCheckedChanged() -> notifyDataSetChanged() -> onBindViewHolder() -> set checkbox -> onChecked...

- resolve

```java
private boolean onBind;

@Override
public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
    if (onbind) {
        return;
    }
    // other opts
}

@Override
public void onBindViewHolder(YourAdapter.ViewHolder viewHolder, int position) {
    onBind = true;
    bind(holder, view);
    onBind = false;
}
```

- reference
    - [Recycleview notifyDataSetChanged() IllegalStateException](http://stackoverflow.com/questions/27070220/recycleview-notifydatasetchanged-illegalstateexception)

# 2 at 2016/07/02

- desc
> `ClassA` 中持有对 `ClassB` 实例的引用, 同时 `ClassB` 中持有对 `ClassA` 的引用, 对 `ClassA` 或者 `ClassB` 的实例使用 `Gson.toJson()` 方法, 会引起 `StackOverflowError`

- slove
> 使用 `transient` 关键字将相应的成员变量排除

```java
public class ClassA {
    transient ClassB instance;
}
```

```java
public class ClassB {
    transient ClassA instance;
}
```

- reference
    - [gson.toJson() throws StackOverflowError](http://stackoverflow.com/questions/10209959/gson-tojson-throws-stackoverflowerror)

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
