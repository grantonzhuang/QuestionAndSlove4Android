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
