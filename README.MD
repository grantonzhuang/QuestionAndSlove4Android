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
