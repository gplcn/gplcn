---

layout: post
title: Fragment的状态保存与恢复
categories: Android
description: Fragment的状态保存与恢复过程介绍
keywords: Android, Fragment

---
# Fragment的状态保存与恢复

![](https://raw.githubusercontent.com/gplcn/gplcn.github.io/master/images/posts/android/fragment_lifecycle.png)

## 保存
Fragment都是嵌套在FragmentActivity中，FragmentActivity重要的职责就是对它内部的Fragment进行管理维护；

```
V27.1.1 android.support.v4.app.FragmentActivity#onSaveInstanceState
 @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        markFragmentsCreated();
        //该方法的会调用方法android.support.v4.app.FragmentManagerImpl#saveAllState
        //mFragments 是一个FragmentController对象
        Parcelable p = mFragments.saveAllState();
        if (p != null) {
            outState.putParcelable(FRAGMENTS_TAG, p);
        }
        if (mPendingFragmentActivityResults.size() > 0) {
            outState.putInt(NEXT_CANDIDATE_REQUEST_INDEX_TAG, mNextCandidateRequestIndex);

            int[] requestCodes = new int[mPendingFragmentActivityResults.size()];
            String[] fragmentWhos = new String[mPendingFragmentActivityResults.size()];
            for (int i = 0; i < mPendingFragmentActivityResults.size(); i++) {
                requestCodes[i] = mPendingFragmentActivityResults.keyAt(i);
                fragmentWhos[i] = mPendingFragmentActivityResults.valueAt(i);
            }
            outState.putIntArray(ALLOCATED_REQUEST_INDICIES_TAG, requestCodes);
            outState.putStringArray(REQUEST_FRAGMENT_WHO_TAG, fragmentWhos);
        }
    }
 
 //获取FragmentManager也是通过FragmentController来获取的
 public FragmentManager getSupportFragmentManager() {
        return mFragments.getSupportFragmentManager();
    }
```

* 对于Fragment的控制，FragmentActivity其实是通过FragmentController实现的；
* 对于Fragment的状态系统有一个重要的类``final class FragmentState ``，该类内部将记录Fragment有关的eg:tag、id、hidden、arguments等等内部的和外部的各种信息。

* 阅读``android.support.v4.app.FragmentManagerImpl#saveAllState``实现源码会发现Fragment的状态将被保存到FragmentState中，然后多个Framgment对应的FragmentState将以数组的形式被保存至FragmentManagerState的mActive字段中，FragmentManagerState将被保存至FragmentActivity的方法onSaveInstanceState中的参数outState中。

## 恢复
在Activity中初始化的任何Fragments都需要通过tag进行查找，确保在配置更改时避免不必要地重新创建Fragments。

在Activity中正确创建Fragment示例：
```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        if (savedInstanceState != null) { 
           mMyFragment = (MyFragment)  
              getSupportFragmentManager().findFragmentByTag(FRAGMENT_TAG);
        } else if (mMyFragment == null) { 
           mMyFragment = new MyFragment();
        }
    }
```
可以在``onCreateView ``中取出保存的状态数据。

```
 @Nullable
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container,
            @Nullable Bundle savedInstanceState) {
        return null;
    }
```
## 注意事项

保证Fragment恢复状态正常应该

* 在创建Fragment后调用 ``setRetainInstance(boolean)``。
* 创建Fragment前使用FragmentManager根据tag检索片段，。

```
/**
     * Control whether a fragment instance is retained across Activity
*      * re-creation (such as from a configuration change).  This can only
     * be used with fragments not in the back stack.  If set, the fragment
     * lifecycle will be slightly different when an activity is recreated:
     * <ul>
     * <li> {@link #onDestroy()} will not be called (but {@link #onDetach()} still
     * will be, because the fragment is being detached from its current activity).
     * <li> {@link #onCreate(Bundle)} will not be called since the fragment
     * is not being re-created.
     * <li> {@link #onAttach(Activity)} and {@link #onActivityCreated(Bundle)} <b>will</b>
     * still be called.
     * </ul>
     */
    public void setRetainInstance(boolean retain) {
        mRetainInstance = retain;
    }
```