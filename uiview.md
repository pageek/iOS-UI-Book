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
    
讲一个元素从它的父容器中移除也非常的简单

    - (void)removeFromSuperview;

如果要获取一个元素的子元素，需要借助`UIView`的`tag`属性：

    - (nullable __kindof UIView *)viewWithTag:(NSInteger)tag; 

## 布局


#### layoutsubviews

所有关于子元素的布局都应该通过重写`layoutSubviews`来实现。

> (This method) Lays out subviews.

> Subclasses can override this method as needed to perform more precise layout of their subviews. You should override this method only if the autoresizing and constraint-based behaviors of the subviews DO NOT offer the behavior you want. You can use your implementation to set the frame rectangles of your subviews directly.

> 以上节选自 UIView Class Reference
    
    
`layoutSubviews` 的主要功能就是让程序员自己实现子 `UIViews` 的布局算法，从而在需要重新布局的时候，父 `UIView` 会按照这个流程重新布局自己的子 `UIViews`。

而且，`layoutSubviews` 方法**只能被系统触发调用，程序员不能手动直接调用**该方法。要引起该方法的调用，可以调用 `UIView` 的 `setNeedsLayout` 方法来标记一个 `UIView`。这样一来，在 UI 线程的下次绘制循环中，系统便会调用该 `UIView` 的 `layoutSubviews` 方法。

在这些情况下，会调用`layoutSubviews`:
* `addSubview` 或类似的添加子元素的操作
* 当前View的`frame`发生改变
* 滚动`UIScrollView`
* 当前View的子元素的`frame`发生改变


#### 调用


标记一个`UIView`需要重新布局的方法有：

* `setNeedsLayout`

    标记为需要重新布局，不立即刷新，但layoutSubviews一定会被调用
配合layoutIfNeeded立即更新

* `layoutIfNeeded`

    如果，有需要刷新的标记，立即调用layoutSubviews进行布局

当一个元素的`frame`依赖于内容的时候，比如一个用来显示用户姓名的`nameLabel`被重新赋值后，就需要标记要重新计算布局。例如：

    -(void)setName:(NSString *)name{
        nameLabel.text = name;
        [self setNeedsLayout];
    }

#### 技巧

因为改变元素的`frame`会引起`layoutSubviews`，在`layoutSubviews`里面，我们可以认为当前元素的`frame`已经被设置到最合适的大小了，所以我们可以在这里计算一些子元素相对父元素位置的`frame`。

比如，我们希望`nameLabel`相对于父容器垂直居中，并且宽度跟父容器大小一致:

    -(void)layoutSubviews{
        
        [nameLabel sizeToFits];
        nameLabel.frame = CGRectMake(0,
                                (CGRectGetHeight(self.frame) - CGRectGetHeight(viewA.frame))/2,
                                CGRectGetWidth(self.frame),
                                CGRectGetHeight(viewA.frame)
                                );
    }