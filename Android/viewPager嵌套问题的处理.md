[**android viewpager2 嵌套viewpage2*****Android\* \*ViewPager嵌套\*ViewPager布局**](https://blog.csdn.net/weixin_31414515/article/details/117314095)

2021-05-27 04:34:11

# 一、事件分发与拦截问题

> **dispatchTouchEvent**方法用于事件的分发，Android中所有的事件都必须经过这个方法的分发，
>
> 然后决定是自身消费当前事件还是继续往下分发给子控件处理。返回true表示不继续分发，事件没有被消费。
>
>返回false则继续往下分发，如果是ViewGroup则分发给onInterceptTouchEvent进行判断是否拦截>该事件。
>
>onTouchEvent方法用于事件的处理，返回true表示消费处理当前事件，返回false则不处理，交给>子控件进行继续分发。
>
>**onInterceptTouchEvent是ViewGroup中才有的方法，View中没有**，它的作用是负责事件的拦截，>返回true的时候表示拦截当前事件，
>
>不继续往下分发，交给自身的onTouchEvent进行处理。返回false则不拦截，继续往下传。这是>ViewGroup特有的方法，
>
>因为ViewGroup中可能还有子View，而在Android中View中是不能再包含子View的(iOS可以)。

# 二、viewPager嵌套问题的处理

```java
 package com.lwj.app.customview;

import android.content.Context;

import android.graphics.PointF;

import android.support.v4.view.ViewPager;

import android.util.AttributeSet;

import android.view.MotionEvent;

    public class ChildViewPager extends ViewPager {

        /**
         * 触摸时按下的点
         **/

        PointF downP = new PointF();

        /**
         * 触摸时当前的点
         **/

        PointF curP = new PointF();

        OnSingleTouchListener onSingleTouchListener;

        public ChildViewPager(Context context, AttributeSet attrs) {

            super(context, attrs);

// TODO Auto-generated constructor stub

        }

        public ChildViewPager(Context context) {

            super(context);

// TODO Auto-generated constructor stub

        }

        @Override

        public boolean onTouchEvent(MotionEvent arg0) {

// TODO Auto-generated method stub

//每次进行onTouch事件都记录当前的按下的坐标

            if (getChildCount() <= 1) {

                return super.onTouchEvent(arg0);

            }

            curP.x = arg0.getX();

            curP.y = arg0.getY();

            if (arg0.getAction() == MotionEvent.ACTION_DOWN) {

//记录按下时候的坐标

//切记不可用 downP = curP ，这样在改变curP的时候，downP也会改变

                downP.x = arg0.getX();

                downP.y = arg0.getY();

//此句代码是为了通知他的父ViewPager现在进行的是本控件的操作，不要对我的操作进行干扰

                getParent().requestDisallowInterceptTouchEvent(true);

            }

            if (arg0.getAction() == MotionEvent.ACTION_MOVE) {

//此句代码是为了通知他的父ViewPager现在进行的是本控件的操作，不要对我的操作进行干扰

                getParent().requestDisallowInterceptTouchEvent(true);

            }

            if (arg0.getAction() == MotionEvent.ACTION_UP || arg0.getAction() == MotionEvent.ACTION_CANCEL) {

//在up时判断是否按下和松手的坐标为一个点

//如果是一个点，将执行点击事件，这是我自己写的点击事件，而不是onclick

                getParent().requestDisallowInterceptTouchEvent(false);

                if (downP.x == curP.x && downP.y == curP.y) {

                    return true;

                }

            }

            super.onTouchEvent(arg0); //注意这句不能 return super.onTouchEvent(arg0); 否则触发parent滑动

            return true;

        }

    }
```