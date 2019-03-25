---

layout: post
title: enum在json中的序列化与反序列化
categories: Java
description: 枚举的序列化与反序列化实现
keywords: Json, enum

---
# enum在json中的序列化与反序列化

程序中使用enum表示状态可以增强程序可读性，方便逻辑处理，那么如何将json中的某些字段反序列化时直接转为enum呢？如何在进行参数上传时将enum序列化化成我们预期的值呢？
现在来看如何解决上述需求

eg:枚举如下

~~~
public enum Size {

    S("s"),

    M("m"),

    L("l");

    private final String code;

    Size(String code) {
        this.code = code;
    }

}
~~~

json字符串如下

~~~
	{
		"size":"s"
	}
~~~

需求是将字符串中的``size``反序列化成``S``
## 方法一 使用Jackson
如果保持上面代码不变,直接使用Jackson来序列化``Size.S``得到的结果是``”S“``;如果json串中是``"size":"S"``
我们也可以反序列为``Size.S``，但这在平时的开发中很难各个开发之间对接很少定义的这么刚刚好；

我们接下基于平时场景来解决上述需求：
对枚举做如下更改，然后使用jackosn来进行反序列化操作即可实现反序列化的需求

先看看更改后的源码

~~~
public enum Size {

    S("s"),

    M("m"),

    L("l");

    private final String code;

    Size(String code) {
        this.code = code;
    }

    @JsonCreator
    public static Size from(String code) {
        for (Size size : values()) {
            if (size.code.equals(code)) {
                return size;
            }
        }
        return null;
    }
}
~~~

如上如果进行序列化，还不能得到我们预期的结果，实现我们预期的序列化我们要增加如下方法

~~~
    @JsonValue
    public String getValue() {
        return code;
    }
~~~

* 如果我们删除``@JsonCreator``注解的方法，仅保留 ``@JsonValue``注解过的方法，我们也可以得到预期的反序列化结果；
* 感兴趣的同学，可以撸下Jackson实现，Jackon则会通过``@JsonCreator``来解析，如果没有添加此注解方法，它会通过分析是否有``@JsonValue``注解的方法，如果有则利用该方法来逆向得到预期的值；

再看一个例子，如果枚举类是这样的

~~~
public enum Level {

    HIGH(2),

    MIDDLE(1),

    LOW(0);

    private final int code;

    Level(int code) {
        this.code = code;
    }

}
~~~
json是这样的

~~~
{
	"level":0
}
~~~

然后我们用jackson来解析会发现``level``将被解析成``HIGH ``,但不会解析失败；why?
because 枚举自带属性`` private final int ordinal;`` 这个属性解释是"The ordinal of this enumeration constant (its position in the enum declaration, where the initial constant is assigned an ordinal of zero)." ;jakson进行反序列化时既没发现被``@JsonCreator``注解的方法又没发现被`` @JsonValue``注解的方法，则会通过比较``ordinal ``和json串中的value试图找到对应的枚举(``com.fasterxml.jackson.databind.deser.std.EnumDeserializer#_enumsByIndex``中存放的就是按位置存放的所有枚举值)；

假如我们是这么写的

~~~
public enum Size {

    S("s"),

    M("m"),

    L("l");

    private final String code;

    Size(String code) {
        this.code = code;
    }
    
    @JsonValue
    public String getValue() {
        return code;
    }
}
~~~

* 然后我们来解析上面的``"size":"s"``，我们得到的结果将是``S ``，因为在上面json串value是字符的情况下，没有被``@JsonCreator``但是有被`` @JsonValue``注解的方法，则会在``com.fasterxml.jackson.databind.deser.std.EnumDeserializer#_lookupByName``寻找对应的目标，但如果json串value是数字的话就不是这种情况了，规则可以查看``EnumDeserializer``的方法``deserialize``；
* 被``@JsonCreator``和`` @JsonValue``注解的方法名不限，但``@JsonCreator``注解的方法必须是``static``，否则你得的结果会与预期有差别甚至报错；


其实使用Jackson实现这些需求还有一种办法就是使用``@JsonSerialize``注解，配合写一个类继承JsonSerializer使用，比上面繁琐很多，感兴趣的可以看看。

对枚举解析的具体实现源码可以阅读

* 当没有被``@JsonCreator``注解的方法时
 * ``com.fasterxml.jackson.databind.deser.std.EnumDeserializer``
* 当有被``@JsonCreator``注解的方法时 *
 * `` com.fasterxml.jackson.databind.deser.std.FactoryBasedEnumDeserializer``

## 方法二 使用Gson

~~~

public enum Size {

    @SerializedName("s")
    S("s"),

    @SerializedName("m")
    M("m"),

    @SerializedName("l")
    L("l");

    private final String code;

    Size(String code) {
        this.code = code;
    }
    
}

~~~

* 在每个枚举值上加上``@ SerializedName ``注解也可以实现序列化与反序列化的需求,但总觉得有点旁门邪路，不是正规支持的；
* ``@ SerializedName``只接受字符串``value``,无法指定其他类型的值，如果想序列化成非字符串的值这种方式无法实现；


## 其他
阿里的fastjson也有类似功能，在此不赘述。