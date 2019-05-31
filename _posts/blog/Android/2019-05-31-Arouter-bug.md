---
layout: post
title: Arouter no match path
categories: Android
description: Arouter踩坑
keywords: Arouter
---
# Arouter no match path

## Java Kotlin混合编程导致No match path
混合编程时要同时配置

```
apply plugin: 'kotlin-kapt'
android {
	defaultConfig {
	       ...
	        javaCompileOptions {
	            annotationProcessorOptions {
	                arguments = [AROUTER_MODULE_NAME: project.getName()]
	            }
	        }
	    }
	
	    kapt {
	        arguments {
	            arg("moduleName", project.getName())
	        }
	        generateStubs = true
	    }
	    
	kapt {
	    arguments {
	        arg("moduleName", project.getName())
	    }
	}

}

dependencies {
	compile 'com.alibaba:arouter-api:xxx'
    annotationProcessor 'com.alibaba:arouter-compiler:xxx'
    kapt 'com.alibaba:arouter-compiler:x.x.x'
    ...
}
```
## 不同的module使用相同的一级路径

分析Arouter常规调用过程 

```
ARouter.getInstance().build(path).navigation();
```

根据调用链接会跳转到

`com.alibaba.android.arouter.launcher._ARouter`

```
 protected Object navigation(final Context context, final Postcard postcard, final int requestCode, final NavigationCallback callback) {
        try {
            LogisticsCenter.completion(postcard);
        } catch (NoRouteFoundException ex) {
            ...
    }
```

查看`LogisticsCenter.completion(postcard);`内部逻辑

```
public synchronized static void completion(Postcard postcard) {
        if (null == postcard) {
            throw new NoRouteFoundException(TAG + "No postcard!");
        }

        RouteMeta routeMeta = Warehouse.routes.get(postcard.getPath());
        if (null == routeMeta) {    // Maybe its does't exist, or didn't load.
            Class<? extends IRouteGroup> groupMeta = Warehouse.groupsIndex.get(postcard.getGroup());  // Load route meta. 
            if (null == groupMeta) {
                throw new NoRouteFoundException(TAG + "There is no route match the path [" + postcard.getPath() + "], in group [" + postcard.getGroup() + "]");
            } else {
                // Load route and cache it into memory, then delete from metas.
                try {
                    if (ARouter.debuggable()) {
                        logger.debug(TAG, String.format(Locale.getDefault(), "The group [%s] starts loading, trigger by [%s]", postcard.getGroup(), postcard.getPath()));
                    }

                    IRouteGroup iGroupInstance = groupMeta.getConstructor().newInstance();
                    iGroupInstance.loadInto(Warehouse.routes);
                    
                    Warehouse.groupsIndex.remove(postcard.getGroup());

                    if (ARouter.debuggable()) {
                        logger.debug(TAG, String.format(Locale.getDefault(), "The group [%s] has already been loaded, trigger by [%s]", postcard.getGroup(), postcard.getPath()));
                    }
                } catch (Exception e) {
                    throw new HandlerException(TAG + "Fatal exception when loading group meta. [" + e.getMessage() + "]");
                }

                completion(postcard);   // Reload
            }
        } else {
            postcard.setDestination(routeMeta.getDestination());
            postcard.setType(routeMeta.getType());
            postcard.setPriority(routeMeta.getPriority());
            postcard.setExtra(routeMeta.getExtra());

            Uri rawUri = postcard.getUri();
            if (null != rawUri) {   // Try to set params into bundle.
                Map<String, String> resultMap = TextUtils.splitQueryParameters(rawUri);
                Map<String, Integer> paramsType = routeMeta.getParamsType();

                if (MapUtils.isNotEmpty(paramsType)) {
                    // Set value by its type, just for params which annotation by @Param
                    for (Map.Entry<String, Integer> params : paramsType.entrySet()) {
                        setValue(postcard,
                                params.getValue(),
                                params.getKey(),
                                resultMap.get(params.getKey()));
                    }

                    // Save params name which need auto inject.
                    postcard.getExtras().putStringArray(ARouter.AUTO_INJECT, paramsType.keySet().toArray(new String[]{}));
                }

                // Save raw uri
                postcard.withString(ARouter.RAW_URI, rawUri.toString());
            }

            switch (routeMeta.getType()) {
                case PROVIDER:  // if the route is provider, should find its instance
                    // Its provider, so it must implement IProvider
                    Class<? extends IProvider> providerMeta = (Class<? extends IProvider>) routeMeta.getDestination();
                    IProvider instance = Warehouse.providers.get(providerMeta);
                    if (null == instance) { // There's no instance of this provider
                        IProvider provider;
                        try {
                            provider = providerMeta.getConstructor().newInstance();
                            provider.init(mContext);
                            Warehouse.providers.put(providerMeta, provider);
                            instance = provider;
                        } catch (Exception e) {
                            throw new HandlerException("Init provider failed! " + e.getMessage());
                        }
                    }
                    postcard.setProvider(instance);
                    postcard.greenChannel();    // Provider should skip all of interceptors
                    break;
                case FRAGMENT:
                    postcard.greenChannel();    // Fragment needn't interceptors
                default:
                    break;
            }
        }
    }
```

Aouter 要求path必须有至少两级的路径，是因为Arouter在寻找route的时候，是通过第一级路径，也就是这里的”approval”来寻找的。Aouter通过”approval”找到了route，并且在groupIndex中删除了这个路径，代表已经加载到了内存。

```
/**
 * DO NOT EDIT THIS FILE!!! IT WAS GENERATED BY AROUTER. */
public class ARouter$$Group$$approval implements IRouteGroup {
  @Override
  public void loadInto(Map<String, RouteMeta> atlas) {
    atlas.put("/approval/customer_ref", RouteMeta.build(RouteType.ACTIVITY, CustomerRefApprovalActivity.class, "/approval/customer_ref", "approval", null, -1, -2147483648));
    atlas.put("/approval/mainactivity", RouteMeta.build(RouteType.ACTIVITY, ApprovalMainActivity.class, "/approval/mainactivity", "approval", new java.util.HashMap<String, Integer>(){{put("mDefalutTab", 8); }}, -1, -2147483648));
    atlas.put("/approval/operate", RouteMeta.build(RouteType.PROVIDER, ApprovalOperateServiceImpl.class, "/approval/operate", "approval", null, -1, -2147483648));
    atlas.put("/approval/pageskip", RouteMeta.build(RouteType.PROVIDER, ApprovalPageSkipServiceImpl.class, "/approval/pageskip", "approval", null, -1, -2147483648));
    atlas.put("/approval/setting/manager", RouteMeta.build(RouteType.PROVIDER, ApprovalSettingManager.class, "/approval/setting/manager", "approval", null, -1, -2147483648));
  }
}
```
相同的module使用了相同的一级路径，在Arouter第一次寻找到route的时候便删除了这个一级路径的group，因为一级路径的重复，再调用另一个module的相同一级路径的路由时，由于之前Warehouse.groupsIndex已经删除，便导致了there’s no route matched的错误

