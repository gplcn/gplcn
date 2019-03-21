---

layout: post
title: Android RelativeLayout#onMeasure 源码解析
categories: Android
description: 解析RelativeLayout#onMeasure过程
keywords: RelativeLayout, Android,View

---

# android.widget.RelativeLayout#onMeasure 源码解析

##  View的测量过程：

先看一下View#measure源码

```
public final void measure(int widthMeasureSpec, int heightMeasureSpec) {
        ...

        final boolean forceLayout = (mPrivateFlags & PFLAG_FORCE_LAYOUT) == PFLAG_FORCE_LAYOUT;

        // Optimize layout by avoiding an extra EXACTLY pass when the view is
        // already measured as the correct size. In API 23 and below, this
        // extra pass is required to make LinearLayout re-distribute weight.
        final boolean specChanged = widthMeasureSpec != mOldWidthMeasureSpec
                || heightMeasureSpec != mOldHeightMeasureSpec;
        final boolean isSpecExactly = MeasureSpec.getMode(widthMeasureSpec) == MeasureSpec.EXACTLY
                && MeasureSpec.getMode(heightMeasureSpec) == MeasureSpec.EXACTLY;
        final boolean matchesSpecSize = getMeasuredWidth() == MeasureSpec.getSize(widthMeasureSpec)
                && getMeasuredHeight() == MeasureSpec.getSize(heightMeasureSpec);
        final boolean needsLayout = specChanged
                && (sAlwaysRemeasureExactly || !isSpecExactly || !matchesSpecSize);

        if (forceLayout || needsLayout) {
            // first clears the measured dimension flag
            mPrivateFlags &= ~PFLAG_MEASURED_DIMENSION_SET;

            resolveRtlPropertiesIfNeeded();

				//在这种情况下DecorView forceLayout==true
            int cacheIndex = forceLayout ? -1 : mMeasureCache.indexOfKey(key);
            if (cacheIndex < 0 || sIgnoreMeasureCache) {
                // measure ourselves, this should set the measured dimension flag back
                onMeasure(widthMeasureSpec, heightMeasureSpec);
                mPrivateFlags3 &= ~PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT;
            } else {
                long value = mMeasureCache.valueAt(cacheIndex);
                // Casting a long to int drops the high 32 bits, no mask needed
                setMeasuredDimensionRaw((int) (value >> 32), (int) value);
                mPrivateFlags3 |= PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT;
            }

           ...

            mPrivateFlags |= PFLAG_LAYOUT_REQUIRED;
        }

        mOldWidthMeasureSpec = widthMeasureSpec;
        mOldHeightMeasureSpec = heightMeasureSpec;

        mMeasureCache.put(key, ((long) mMeasuredWidth) << 32 |
                (long) mMeasuredHeight & 0xffffffffL); // suppress sign extension
    }

```

* 上面方法被``final``修饰,各``ViewGroup``都继承自``View``所以所有布局都走这个实现逻辑；
* 通过上面源码还发现View对测量如下优化：
 * 读取``mPrivateFlags ``标记是否强制测量及重新布局；
 * 通过比较新旧``measureSpec ``来计算是否需要重新测量等操作；
 * 如果``forceLayout、needsLayout``为``false``将不会进行实际的测量操作；
 * 如果``forceLayout``为``true``将立即执行测量操作；
 * 如果``forceLayout``为``false``,``needsLayout ``为``true ``则对``mPrivateFlags3``存储一个标记，在``layout(l,t,r,b)``时再调用``onMeasure()``；

```
//layout部分源码
 public void layout(int l, int t, int r, int b) {
        if ((mPrivateFlags3 & PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT) != 0) {
            onMeasure(mOldWidthMeasureSpec, mOldHeightMeasureSpec);
            mPrivateFlags3 &= ~PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT;
        }
	...
}

```
## RelativeLayout的measure过程


```
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec){
    …
    View[] views = mSortedHorizontalChildren;
    int count = views.length;

    for (int i = 0; i < count; i++) {
        View child = views[i];
        if (child.getVisibility() != GONE) {
            LayoutParams params = (LayoutParams) child.getLayoutParams();
            int[] rules = params.getRules(layoutDirection);

            applyHorizontalSizeRules(params, myWidth, rules);
	//
            measureChildHorizontal(child, params, myWidth, myHeight);

            if (positionChildHorizontal(child, params, myWidth, isWrapContentWidth)) {
                offsetHorizontalAxis = true;
            }
        }
    }

    views = mSortedVerticalChildren;
    count = views.length;
    final int targetSdkVersion = getContext().getApplicationInfo().targetSdkVersion;

    for (int i = 0; i < count; i++) {
        final View child = views[i];
        if (child.getVisibility() != GONE) {
            final LayoutParams params = (LayoutParams) child.getLayoutParams();

            applyVerticalSizeRules(params, myHeight, child.getBaseline());
            measureChild(child, params, myWidth, myHeight);
            if (positionChildVertical(child, params, myHeight, isWrapContentHeight)) {
                offsetVerticalAxis = true;
            }

            if (isWrapContentWidth) {
                if (isLayoutRtl()) {
                    if (targetSdkVersion < Build.VERSION_CODES.KITKAT) {
                        width = Math.max(width, myWidth - params.mLeft);
                    } else {
                        width = Math.max(width, myWidth - params.mLeft + params.leftMargin);
                    }
                } else {
                    if (targetSdkVersion < Build.VERSION_CODES.KITKAT) {
                        width = Math.max(width, params.mRight);
                    } else {
                        width = Math.max(width, params.mRight + params.rightMargin);
                    }
                }
            }

	…
            setMeasuredDimension(width, height);
}

```

* RelativeLayout会对子view进行水平方向和垂直方向两侧测量，因此每个子view都将进行两此measure；
* 如果子view高度和RelativeLayout高度不同，再进行垂直测量时因为与刚刚进行的水平测量时指定的高度不同导致View测量的优化失效


