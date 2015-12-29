# 基础



## CGRect


    /* Rectangles. */

    struct CGRect {
        CGPoint origin;
        CGSize size;
    };
    typedef struct CGRect CGRect;
    

一个`CGRect`包含了一个坐标`CGPoint`和一个尺寸`CGSize`。

`CGPoint` 描述当前元素的左上角距离父容器中的左上角的`X,Y`坐标。

`CGSize` 描述了当前元素的宽度和高度。

## 基本属性

#### frame

`frame` 该view在父view坐标系统中的位置和大小。（参照点是父亲的坐标系统）

#### bounds
`bounds` 该view在本身坐标系统中 的位置和大小。（参照点是本身坐标系统）

#### center
`center`指的就是整个视图的中心点，改变视图的`center`也会改变`frame`



## 常用方法

通过`x,y,width,heigh`四个属性可以直接创建一个`CGRect`:

    CGRect CGRectMake(CGFloat x, CGFloat y, CGFloat width, CGFloat height);
    
`CGRect`预置了灵活的方法用来获取各个布局属性的详细数值：

    CG_EXTERN CGFloat CGRectGetMinX(CGRect rect)
    CG_EXTERN CGFloat CGRectGetMidX(CGRect rect)
    CG_EXTERN CGFloat CGRectGetMaxX(CGRect rect)
    CG_EXTERN CGFloat CGRectGetMinY(CGRect rect)
    CG_EXTERN CGFloat CGRectGetMidY(CGRect rect)
    CG_EXTERN CGFloat CGRectGetMaxY(CGRect rect)
    CG_EXTERN CGFloat CGRectGetWidth(CGRect rect)
    CG_EXTERN CGFloat CGRectGetHeight(CGRect rect)


通常在计算中会有浮点数出现在`CGRect`当中，浮点数值的出现会引起屏幕渲染时文本或者图片边缘的模糊，可以是使用

    CGRect CGRectIntegral(CGRect rect)
    
得到一个包含当前`CGRect`最小的，并且坐标和宽高都是整数的`CGRect`。
