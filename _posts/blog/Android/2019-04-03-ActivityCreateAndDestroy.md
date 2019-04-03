---

layout: post
title: Activity销毁与重建
categories: Android
description: Activity销毁与重建过程简介
keywords: Android, Activity

---
# Activity销毁与重建

## Activity的生命周期
![](https://raw.githubusercontent.com/gplcn/gplcn.github.io/master/images/posts/android/activity_lifecycle.png)
## Activity的销毁的两种情况：

* 当用户按返回按钮或你的Activity通过调用finish()销毁时，这属于正常销毁；
	* 正常销毁是不需要恢复状态的，下次进入重新创建新的实例；
* 如果Activity当前被停止或长期未使用，或者前台Activity需要更多资源以致系统必须关闭后台进程恢复内存，系统也可能会销毁Activity，这属于非正常销毁；
	* 非正常销毁时，系统会保存销毁前的状态数据，用户再次回到销毁的Activity,系统会使用保存的数据来创建新的实例，为用户主动状态。
	* 旋转屏幕、键盘可用性改变、更改语言都可以归结为非正常销毁；
	* 如果需要模拟非正常销毁，可以打开开发者选项，选择不保留活动。

## 保存状态
当系统要保存当前页面的的状态数据时会调用``onSaveInstanceState``；
原则：即当系统“未经你许可”时销毁了你的activity，则onSaveInstanceState会被系统调用，这是系统的责任，因为它必须要提供一个机会让你保存你的数据；

例如如下操作将调用``onSaveInstanceState ``：

* 当用户按下HOME键时；
* 长按HOME键，选择运行其他的程序时；
* 按下电源按键（关闭屏幕显示）时；
* 从activity中启动一个新的activity时；
* 屏幕方向切换时，例如从竖屏切换到横屏时。

系统只会保存页面中指定过ID的view状态，系统恢复状态时将根据ID找到对应的view恢复对应的状态，系统不会主动保存我们自己的成员变量，如果想恢复这些变量状态需要自己在``onSaveInstanceState``中通过key-value形式保存。

```
android.app.Activity#onSaveInstanceState(android.os.Bundle)
protected void onSaveInstanceState(Bundle outState) {
        outState.putBundle(WINDOW_HIERARCHY_TAG, mWindow.saveHierarchyState());

        outState.putInt(LAST_AUTOFILL_ID, mLastAutofillId);
        Parcelable p = mFragments.saveAllState();
        if (p != null) {
            outState.putParcelable(FRAGMENTS_TAG, p);
        }
        if (mAutoFillResetNeeded) {
            outState.putBoolean(AUTOFILL_RESET_NEEDED, true);
            getAutofillManager().onSaveInstanceState(outState);
        }
        getApplication().dispatchActivitySaveInstanceState(this, outState);
    }
 
 android.view.View#dispatchSaveInstanceState
 protected void dispatchSaveInstanceState(SparseArray<Parcelable> container) {
 //这是为何没有设置过id的view状态不会被保留的原因
        if (mID != NO_ID && (mViewFlags & SAVE_DISABLED_MASK) == 0) {
            mPrivateFlags &= ~PFLAG_SAVE_STATE_CALLED;
            Parcelable state = onSaveInstanceState();
            if ((mPrivateFlags & PFLAG_SAVE_STATE_CALLED) == 0) {
                throw new IllegalStateException(
                        "Derived class did not call super.onSaveInstanceState()");
            }
            if (state != null) {
                // Log.i("View", "Freezing #" + Integer.toHexString(mID)
                // + ": " + state);
                container.put(mID, state);
            }
        }
    }
    
```

## 恢复状态
* 每次创建新的activity实例的或重建一个activity实例的时候都会调用``onCreate``方法；
	* 应该先检查是否Bundle是null，如果是null，则表明是要创建一个全新的对象，而不是重建一个上次被销毁的对象；
* 除了在``onCreate``中恢复状态外，也可以选择在``onRestoreInstanceState``中实现，这个函数在``onStart``之后调用；
	* ``onRestoreInstanceState``只有在有数据要恢复的时候系统会调用，所以不必检查Bundle是否为null。

```
android.app.Activity#onRestoreInstanceState(android.os.Bundle)
protected void onRestoreInstanceState(Bundle savedInstanceState) {
        if (mWindow != null) {
            Bundle windowState = savedInstanceState.getBundle(WINDOW_HIERARCHY_TAG);
            if (windowState != null) {
                mWindow.restoreHierarchyState(windowState);
            }
        }
    }
    
android.view.View#dispatchRestoreInstanceState    
protected void dispatchRestoreInstanceState(SparseArray<Parcelable> container) {
        if (mID != NO_ID) {
        //view会根据id找到保存的状态数据
            Parcelable state = container.get(mID);
            if (state != null) {
                // Log.i("View", "Restoreing #" + Integer.toHexString(mID)
                // + ": " + state);
                mPrivateFlags &= ~PFLAG_SAVE_STATE_CALLED;
                //进行实质性的恢复操作
                onRestoreInstanceState(state);
                if ((mPrivateFlags & PFLAG_SAVE_STATE_CALLED) == 0) {
                    throw new IllegalStateException(
                            "Derived class did not call super.onRestoreInstanceState()");
                }
            }
        }
    }
```

* 注意：``onSaveInstanceState``和``onRestoreInstanceState``不一定成对调用；有些情况下调用``onSaveInstanceState``，但不会销毁activity因此不会调用``onRestoreInstanceState``。

## 状态保存与恢复与生命周期关联图例
![](https://raw.githubusercontent.com/gplcn/gplcn.github.io/master/images/posts/android/basic-lifecycle-savestate.png)

