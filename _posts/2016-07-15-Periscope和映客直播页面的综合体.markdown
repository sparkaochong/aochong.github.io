---
layout: post
title: Periscope和映客直播页面的综合体
date: 2016-07-15 22:41:11.000000000 +09:00
categories: iOS
tags: Periscope&emsp;iOS&emsp;映客&emsp;

---
原版Periscope效果图  
（颜色比较多所以图片比较多，不过效果还不是很好，建议下个[Periscope](https://itunes.apple.com/cn/app/periscope/id972909677?mt=8)，开个VPN看看）

![](http://upload-images.jianshu.io/upload_images/1812927-7c2d8fbb26bf3d12.gif?imageMogr2/auto-orient/strip)

本demo效果图：

![](http://upload-images.jianshu.io/upload_images/1812927-7147f233e22ada1d.gif?imageMogr2/auto-orient/strip)

[代码地址](https://github.com/louis-ly/LYPeriscope)
###实现
#####手势下拉
  模态弹出时需要设置下弹出的模式和风格`modalPresentationStyle`。是为了回头手势下拉漏出上一个控制器的view时是有内容的，而不会是漆黑一片。因为一个控制器切换到另一个控制器时，系统为了节约资源，会将上一个页面暂时从屏幕上移除，需要的时候再加回来。

```
LYLiveViewController *liveVC = [LYLiveViewController new];
liveVC.modalPresentationStyle = UIModalPresentationCustom;
[self presentViewController:liveVC animated:YES completion:nil];
```

  那么手势是添加在控制器的view上面吗？如果你观察上面gif，你会发现，在视图下拉的过程中，后面有一层黑色的遮住，松手控制器消失的过程黑色是会有渐变成透明的过程的。所有手势应该加在自定义的view上，回头所以的视图都加在这个创建的view上面。

```
- (void)viewDidLoad {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor colorWithWhite:0 alpha:0.7];
    
    _containView = [UIView new];
    _containView.backgroundColor = [UIColor colorWithWhite:0.95 alpha:1];
    [_containView addSubview:self.looseCloseTipView];
    [self.view addSubview:_containView];
    
    _panGesture = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(handlePanGesture:)];
    _panGesture.delegate = self;
    [_containView addGestureRecognizer:_panGesture];
}

- (void)viewWillLayoutSubviews {
    [super viewWillLayoutSubviews];
    _containView.frame = self.view.bounds;
}

- (void)handlePanGesture:(UIPanGestureRecognizer *)panGesture {
    CGPoint movePoint = [panGesture translationInView:panGesture.view];
    [panGesture setTranslation:CGPointZero inView:panGesture.view];
    
    if (panGesture.state == UIGestureRecognizerStateBegan) {
        
    } else if (panGesture.state == UIGestureRecognizerStateChanged) {
        _containView.y += movePoint.y / 2.5;
        self.view.backgroundColor = [UIColor colorWithWhite:0 alpha:0.7];
        
        if (_containView.y > 64 && _looseCloseTipView.alpha == 0) {
            [UIView animateWithDuration:0.3 animations:^{
                _looseCloseTipView.alpha = 1;
            }];
        } else if (_containView.y < 64 && _looseCloseTipView.alpha == 1) {
            [UIView animateWithDuration:0.3 animations:^{
                _looseCloseTipView.alpha = 0;
            }];
        } else if (_containView.y < 0) {
            self.view.backgroundColor = [UIColor blackColor];
        }
    } else if (panGesture.state == UIGestureRecognizerStateEnded) {
        if (_containView.y < 0) {
            [UIView animateWithDuration:0.3 delay:0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
                _containView.y = 0;
            } completion:^(BOOL finished) {
                
            }];
        } else if (_containView.y > 64) {
            [UIView animateWithDuration:0.5 delay:0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
                _containView.y = _containView.height;
                self.view.backgroundColor = [UIColor colorWithWhite:0 alpha:0];
            } completion:^(BOOL finished) {
                [self dismissViewControllerAnimated:NO completion:nil];
            }];
        } else if (_containView.y < 64) {
            [UIView animateWithDuration:0.3 delay:0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
                _containView.y = 0;
                self.view.backgroundColor = [UIColor colorWithWhite:0 0.7];
            } completion:^(BOOL finished) {
                
            }];
        }
    }
}

- (UIView *)looseCloseTipView {
    if (!_looseCloseTipView) {
        CGFloat screenWidth =[UIScreen mainScreen].bounds.size.width;
        _looseCloseTipView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, screenWidth, 40)];
        _looseCloseTipView.alpha = 0;
        
        CAGradientLayer *gradient = [CAGradientLayer layer];
        gradient.frame = CGRectMake(0, 0, screenWidth, 40);
        gradient.colors = @[(__bridge id)[UIColor colorWithWhite:0 alpha:0.5].CGColor,
                            (__bridge id)[UIColor colorWithWhite:0 alpha:0.2].CGColor,
                            (__bridge id)[UIColor colorWithWhite:0 alpha:0].CGColor];
        [_looseCloseTipView.layer addSublayer:gradient];
        
        UILabel *label = [[UILabel alloc] initWithFrame:_looseCloseTipView.bounds];
        NSMutableAttributedString *attrString = [[NSMutableAttributedString alloc] initWithString:@"松开以关闭"];
        [attrString addAttribute:NSFontAttributeName value:[UIFont systemFontOfSize:12.0] range:NSMakeRange(0, 3)];
        [attrString addAttribute:NSFontAttributeName value:[UIFont boldSystemFontOfSize:12] range:NSMakeRange(3, 2)];
        [attrString addAttribute:NSForegroundColorAttributeName value:[UIColor whiteColor] range:NSMakeRange(0, attrString.length)];
        label.attributedText = attrString;
        label.textAlignment = 1;
        [_looseCloseTipView addSubview:label];
    }
    return _looseCloseTipView;
}
```

![](http://upload-images.jianshu.io/upload_images/1812927-563d80e09e55aae0.gif?imageMogr2/auto-orient/strip)

  手势移动多少，视图并没有跟着移动多少，所以我这边将移动的距离除以2.5，`_containView.y += movePoint.y / 2.5;`
  如果手势是上拉，则漏出来的是全黑色的，所以在`_containView.y < 0`控制器的view就设置为黑色。

#####加载动画
![](http://upload-images.jianshu.io/upload_images/1812927-5fd0ad3f336205b2.gif?imageMogr2/auto-orient/strip)

  这个一条一条的图，其实只是由一个小图片填充而成的。

![](http://upload-images.jianshu.io/upload_images/1812927-5b5cb7013fff8d6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```
CGFloat screenWidth =[UIScreen mainScreen].bounds.size.width;
_loadingView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, screenWidth * 2, 221)];
_loadingView.backgroundColor = [UIColor colorWithPatternImage:[UIImage imageNamed:@"loading_pattern_video"]];
_loadingView.layer.anchorPoint = CGPointMake(0, 0);
[_containView addSubview:_loadingView];
```

  动画的过程，其实只是图片向做移动的过程而已。不断的重复动画即可。

```
CAKeyframeAnimation *animation = [CAKeyframeAnimation animationWithKeyPath:@"position"];
animation.repeatCount = MAXFLOAT;
animation.duration = 5;
animation.delegate = self;
animation.removedOnCompletion = YES;
animation.fillMode = kCAFillModeForwards;
animation.values = @[[NSValue valueWithCGPoint:CGPointMake(0, 0)],
                     [NSValue valueWithCGPoint:CGPointMake(-screenWidth, 0)]];
[_loadingView.layer addAnimation:animation forKey:@"animation"];
```

#####添加scrollView

```
- (UIScrollView *)scrollView {
    if (!_scrollView) {
        _scrollView = [[UIScrollView alloc] initWithFrame:CGRectMake(0, 0, kScreenWidth, kScreenHeight)];
        _scrollView.contentSize = CGSizeMake(kScreenWidth * 2, 0);
        _scrollView.pagingEnabled = YES;
        _scrollView.showsHorizontalScrollIndicator = NO;
        _scrollView.contentOffset = CGPointMake(kScreenWidth, 0);
        
        LYLiveDetailViewController *detailVC = [LYLiveDetailViewController new];
        detailVC.view.frame = CGRectMake(0, 0, kScreenWidth, kScreenHeight);
        [self addChildViewController:detailVC];
        [_scrollView addSubview:detailVC.view];
        
        LYLiveChatViewController *_chatVC = [LYLiveChatViewController new];
        _chatVC.view.frame = CGRectMake(kScreenWidth, 0, kScreenWidth, kScreenHeight);
        [self addChildViewController:_chatVC];
        [_scrollView addSubview:_chatVC.view];
    }
    return _scrollView;
}
```

  scrollView上面添加两个控制器`LYLiveDetailViewController`和`LYLiveChatViewController`，将代码分开管理。`LYLiveDetailViewController`是管理直播间的详情和直播观众的列表，`LYLiveChatViewController`是聊天室的一些东西，比如送礼物、点赞动画、发送聊天上面的。

#####实现LYLiveDetailViewController内容
![](http://upload-images.jianshu.io/upload_images/1812927-fe7633f4b5ade0e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  当tableView在滚动的时候，上面的`‘English Chap On Hong Kong Radio！’`视图会跟着移动，本文是通过tableView的`scrollViewDidScroll:`代理，实时的改变它的坐标，来实现粘合的感觉。

```
#define LYLiveDetailTitleViewHeight 60
#define LYLiveDetailTableY 128
- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    CGFloat contentOffsetY = scrollView.contentOffset.y;
    _titleView.y = (contentOffsetY < 0 ? -contentOffsetY : 0) + LYLiveDetailTableY - LYLiveDetailTitleViewHeight;
}
```

  此时效果已经基本实现了，如下图：
  
![](http://upload-images.jianshu.io/upload_images/1812927-2db20e49c9c8f180.gif?imageMogr2/auto-orient/strip)

  但是会遇到一个问题，当tableView已经滚到下面的时候，想滑动contentInset.Top区域想实现让整个控制器都下移，来实现退出直播的效果，却被tableView给接收了，之前的pan手势没有被激活。那么怎么办？那么只要当tableView滚至底部的时候，点击contentInset.Top的区域tableView不要接收touch就好了，所以咱们要自定义tableView，实现hitTest:withEvent:方法来进行判断。代码量很少，但是却实现了需要的效果，代码如下：

```
@interface LYLiveDetailTableView : UITableView
@end
@implementation LYLiveDetailTableView
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event {
    return point.y < 0 ? nil : [super hitTest:point withEvent:event];
}
@end
```

![](http://upload-images.jianshu.io/upload_images/1812927-7cb09bde557178a6.gif?imageMogr2/auto-orient/strip)

  早一些假数据后得到下面效果：

![](http://upload-images.jianshu.io/upload_images/1812927-98d0b22e7e01b656.gif?imageMogr2/auto-orient/strip)

  上面`直播观众`的视图有悬浮的效果，并不是通过`headerSection`来实现的，而是重新创建的视图，在`scrollViewDidScroll:`方法中控制`hidden`属性。

#####实现LYLiveChatViewController内容


![](http://upload-images.jianshu.io/upload_images/1812927-a1d915465744c3dd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  这个页面的难点不是很多，都是一些交互的细节，也没啥子好说的了。不过可以说下控制送礼物在屏幕上的数量，观察发现映客上面不管多少个人送礼物，始终只有两个礼物在上面显示。
  本demo给到的方案是，当收到礼物消息的时候，不是马上做显示到屏幕上的操作，而是先存到一个数组里面。
  再创建一个定时器，定时去判断两个礼物位置是否正在做动画，如果没有，则从刚刚存礼物消息的数组中取出最先存进去的`firstObject`礼物消息，做动画操作。
  具体代码看工程里`LYLiveGiftBarrage`和`LYLiveGiftBarrageView`文件。

####END
  想讲的说的差不多了，如果有不理解或者更好的建议欢迎评论里沟通。



