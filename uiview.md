# UIView

UIView表示屏幕上的一块矩形区域，它在App中占有绝对重要的地位，因为IOS中几乎所有可视化控件都是UIView的子类。负责渲染区域的内容，并且响应该区域内发生的触摸事件。



## 初始化

`UIView` 的指定初始化方法为：

    - (instancetype)initWithFrame:(CGRect)frame

尽管可以使用`init`来初始化一个`UIView` ,但最终实际会调用`initWithFrame:CGRectZero`返回一个宽高为0的`UIView`。

## 子元素

`UIView` 有着`HTML`一样的树形层级结构，`A`可以被添加到`B`上面而变成`B`的子元素：


    - (void)addSubview:(UIView *)view;
    
同样的，`UIView`也有类似的纵向显示层级`index`，越晚被添加的元素，它的层级也就越高，通过下面的方法可以改变这些子元素的层级位置：

    - (void)insertSubview:(UIView *)view belowSubview:(UIView *)siblingSubview;
    - (void)insertSubview:(UIView *)view aboveSubview:(UIView *)siblingSubview;
    
    - (void)insertSubview:(UIView *)view belowSubview:(UIView *)siblingSubview;
    - (void)insertSubview:(UIView *)view aboveSubview:(UIView *)siblingSubview;
    
    - (void)bringSubviewToFront:(UIView *)view;
    - (void)sendSubviewToBack:(UIView *)view;
    
    


## 布局
