# 情节提要动画与关键帧动画

## 简单动画示例

因为下面这些 Rectangle 都是在 ItemsControl 中的，因为在容器控件中应用主题样式时，其所有的子对象也都会继承下来。

```
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <ItemsControl Grid.Row="1" x:Name="itemsControlRectangle">             
        <ItemsControl.ItemContainerTransitions>
            <TransitionCollection>
                <EntranceThemeTransition/>
            </TransitionCollection>
        </ItemsControl.ItemContainerTransitions>
        <ItemsControl.ItemsPanel>
            <ItemsPanelTemplate>
                <WrapGrid Height="400"/>
            </ItemsPanelTemplate>
        </ItemsControl.ItemsPanel> 
        <ItemsControl.Items>
            <Rectangle Fill="Red" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Wheat" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Yellow" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Blue" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Green" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Gray" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="White" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Gainsboro" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Magenta" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="CadetBlue" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="NavajoWhite" Width="100" Height="100" Margin="12"/>
            <Rectangle Fill="Khaki" Width="100" Height="100" Margin="12"/>
        </ItemsControl.Items>
    </ItemsControl>
</Grid>
```

## 情节提要动画

就像我们前面介绍的定义样式资源一样，我们也可以将动画设为资源。

```
<Page.Resources>
    <Storyboard x:Name="storyboardRectangle" >
        <DoubleAnimation
            Storyboard.TargetName="rectangle"  
            Storyboard.TargetProperty="Opacity"                
            From="1.0" To="0" Duration="0:0:1"
			AutoReverse="True"
            RepeatBehavior="Forever"/>			
    </Storyboard>
</Page.Resources>
<Grid>
    <Rectangle x:Name="rectangle"       
        Width="200" Height="130" Fill="Blue"/>
</Grid>
```

在理解这些代码意思之前，还是先让动画跑起来，你可以加上一个 Button 并设置其 Click 事件，也可以在 MainPage 方法下直接写如下代码：

```
storyboardRectangle.Begin();
```

运行应用后，Rectangle 的透明度就会渐渐的消失而后出现。

在上面这个示例中，我们为 Rectangle 的 Opacity（透明度）属性设置了动画，Storyboard 通常存放在`<Page.Resources>` 中或 App.xaml 下的 `<Application.Resources>` 中。要给一个对象设置动画，就得应用 x:Name 属性，因为需要在动画中设置 Storyboard.TargetName 属性来创建动画的对象名称。其后的 Storyboard.TargetProperty 属性显然是设置目标属性的。如果你尝试将属性设为 Fill，而将 From 设为 Blue 就会发现这样是不行的，原因是这里设置的动画类型本身是 Double 的。

除此之外，还有 Point 和 Color 两种动画属性类型，也即是 PointAnimation 和 ColorAnimation。如果要将此处的 Rectangle 改变颜色，比如说从 Blue 到 Lime，那么它的属性应该是什么呢？Fill？不是的，虽然在 Rectangle 中是 Fill 属性，但我们最终应该将其定位到 Color。

```
Storyboard.TargetProperty="(rectangle.Fill).(SolidColorBrush.Color)"
```

如果你已经定义了 TargetName 属性为 rectangle，那么 Fill 前的 rectangle 和点都可以去掉。

左右两个括号都是必要的，它表示一个属性的名称。中间的点意味着要先获取第一个括号的属性，也就是设置动画的对应对象，然后进入到其对象模型中，此处是 Color。官网上还给出了其它示例：

`(UIElement.RenderTransform).(TranslateTransform.X)`  应用到 RenderTransform 上，并创建 TranslateTransform 的 X 值的动画

`(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)` 应用到 Fill 上，并在LinearGradientBrush 的 GradientStop 内创建 Color 的动画，这里方括号内的数字表示索引，表示集合中的一项，索引从 0 开始

`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`  应用到RenderTransform上，并创建TranslateTransform

我们还注意到，动画中还有 From 和 To 属性，顾名思义，From 表示动画的开始值，To 表示结束值。

如果没有定义 From 值，那么动画起始值为该对象属性的当前值。

如果想要设置一个和起始值相对的结束值，建议使用 By 属性。

动画在这 3 个属性中至少应该设置一个，否则动画便不会更改值，且这 3 个属性也无法同时存在。

我们还可以用设置 AutoReverse 属性为真以使动画才结束后自动进行反向播放，但反向播放完后不会再继续播放。

设置 RepeatBehavior 属性为“1x”表示动画的播放次数，或者也可以直接设为“Forever”，让其永远播放。

如果动画较多的情况下，我们哈可以设置 BeginTime 来使不同的动画错开播放。

## 关键帧动画

什么是关键帧动画？

关键帧动画建立在上文的情节提要动画概念智商，它令动画沿着一条时间线来逐步达到多个目标值，也就是说如果要让上文的 Fill 属性从 Blue 变化到 Lime 之间还可以令其先变化到 Red 或 Orange 等。

更为巧妙的是，你可以同时指定不同的属性来制作复杂的动画。

如果稍微会一点 Flash，对于关键帧的概念肯定没有问题。

1.线性关键帧

我们为动画设置一个 KeyTime 来表示间隔的时间戳，例如我们可以设置 4 个时间戳为：`KeyTime="0:0:0"、"0:0:2"、"0:0:8"、"0:0:9"`，可以看到动画在中间部分时跳跃性非常之大。但其动画都是缓慢变化的，因为这是线性的，还有一种另外一种关键帧它会让动画在时间戳上产生突变而不是渐变，这就是离散式关键帧（就像概率论中的离散型和连续型一样）。

2.样条关键帧

其主要通过 KeySpline 属性来建立过渡，例如 `KeySpline="0.1,0.1  0.7.0.8"`，这里有两个点，分别对应贝塞尔曲线的第一个控制点和第二个控制点，描述了动画的加速情况。关于贝塞尔曲线，建议大家看看维基百科，在图形化编程中非常常用。

3.缓动关键帧

这种模式就更加高级了，它由多个预定义好的数学公式来控制。以下是的缓动函数列表来源于网络：

- BackEase：动画开始在指定路径上运动前稍微收缩动画的运行。
- BounceEase：创建回弹效果。
- CircleEase：使用圆函数创建加速或减速的动画。
- CubicEase：使用函数 f(t) = t3 创建加速或减速的动画。
- ElasticEase：创建一个动画，模拟弹簧的来回振荡运动，直到它达到停止状态。
- ExponentialEase：使用指数公式创建加速或减速的动画。
- PowerEase：使用公式 f(t) = tp 创建加速或减速的动画，其中 p 等于 Power 属性。
- QuadraticEase：使用函数 f(t) = t2 创建加速或减速的动画。
- QuarticEase：使用函数 f(t) = t4 创建加速或减速的动画。
- QuinticEase：使用函数 f(t) = t5 创建加速或减速的动画。
- SineEase：使用正弦公式创建加速或减速的动画。

