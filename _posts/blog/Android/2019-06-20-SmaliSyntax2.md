---
layout: post
title: smali语法2
categories: [Android]
description: Android逆向分析
keywords: smali
---

# ***smali***语法 ***2***

## 一 、smali数据类型


### 1．Dalvik字节码

Davlik字节码中，寄存器都是32位的，能够支持任何类型，64位类型（Long/Double）用2个连续的寄存器表示；

Dalvik字节码有两种类型：原始类型；引用类型（包括对象和数组）

##### 原始类型

			
| v  	| void 只能用于返回值类型	|
|:----	|:------------------	|
| Z   	| boolean					|
| B   	| byte						|
| S   	| short					|
| C   	| char						|
| I   	| int						|
| J   	| long（64位）				|
| F   	| float					|
| D   	| double（64位）			|

 
##### 对象类型

	Lpackage/name/ObjectName; ———— 相当于java中的package.name.ObjectName;
	
	L：表示这是一个对象类型
	
	package/name ： 该对象所在的包
	
	;：表示对象名称的结束

### 2．数组的表示形式

	[I  :表示一个整形的一维数组，相当于java的int[]；对于多维数组，只要增加[ 就行了，[[I = int[][];注：每一维最多255个；
	对象数组的表示形式：[Ljava/lang/String表示一个String的对象数组；

### 3．方法的表示形式

	Lpackage/name/ObjectName;->methodName(III)Z
          Lpackage/name/ObjectName  表示类型
          methodName   表示方法名
          III   表示参数（这里表示为3个整型参数）说明：方法的参数是一个接一个的，中间没有隔开


### 4．字段的表示形式

	Lpackage/name/ObjectName;->FieldName:Ljava/lang/String；即表示：包名，字段名和字段类型

### 5．寄存器指定

	有两种方式指定一个方法中有多少寄存器是可用的：
	.registers  指令指定了方法中寄存器的总数
	.locals    指令表明了方法中非参寄存器的总数，出现在方法中的第一行	 

### 6．方法的表示

	方法有直接方法和虚方法两种，直接方法的声明格式如下：
	.method<访问权限>[修饰关键字]<方法原型>
	<.locals>
	[.parameter]
	[.prologue]
	[.line]
	<代码体>
	.end method
	 
	访问权限有public、private等，修饰关键字有static、constructor等。方法原型描述了方法的名称、参数与返回值。
	.registers指定了方法中寄存器的总数
	.locals指定了方法中非参寄存器的总数（局部变量的个数）；
	.parameter指定了方法的参数；
	.prologue指定了代码的开始处；
	.line指定了该处指令在源代码中的位置。该处指令在源代码中的位置。
* 构造函数的返回类型为V，名字为<init>。

### 7．方法的传参

	当一个方法被调用的时候，方法的参数被置于最后N个寄存器中；
	
	例如：一个方法有2个参数，5个寄存器（v0\~v4），那么，参数将置于最后2个寄存器（v3和v4）。非静态方法中的第一个参数总是调用该方法的对象。
	
* 对于静态方法除了没有隐含的this参数外，其他都一样

### 8．寄存器的命名方式：V命名、P命名

	第一个寄存器就是方法中的第一个参数寄存器。比较：使用P命名是为了防止以后如果在方法中增加寄存器，需
	要对参数寄存器重新进行编号的缺点。特别说明一下：Long和Double类型是64位的，需要2个连续的寄存器
	
* 例如：对于非静态方法LMyObject->myMethod(IJZ)V，有4个参数：LMyObject(隐含的),int,long,boolean
需要5个寄存器（因为long占有2个连续的寄存器）来存储参数：
			
|P0  		|this（非静态方法中这个参数是隐含的)|
|:-------	|:-----------------------------|
|P1 		| I (int)							 |
|P2，P3 	| J (long)						 |
|P4   		| Z(bool)							 |

*如果是静态的就是三个参数

|P0  		|I (int)		|
|:-------	|:----------	|
|P1，P2 	|J (long)		|
|P3 		|Z(bool)		|


 

## 二、成员变量

```
# static fields        定义静态变量的标记
# instance fields      定义实例变量的标记
# direct methods       定义静态方法的标记
# virtual methods      定义非静态方法的标记

```

  

## 三、控制条件

```
"if-eq vA, vB, :cond_**"   	如果vA等于vB则跳转到:cond_**
"if-ne vA, vB, :cond_**"   	如果vA不等于vB则跳转到:cond_**
"if-lt vA, vB, :cond_**"   	如果vA小于vB则跳转到:cond_**
"if-ge vA, vB, :cond_**"   	如果vA大于等于vB则跳转到:cond_**
"if-gt vA, vB, :cond_**"   	如果vA大于vB则跳转到:cond_**
"if-le vA, vB, :cond_**"    如果vA小于等于vB则跳转到:cond_**
"if-eqz vA, :cond_**"   	如果vA等于0则跳转到:cond_**
"if-nez vA, :cond_**"   	如果vA不等于0则跳转到:cond_**
"if-ltz vA, :cond_**"    	如果vA小于0则跳转到:cond_**
"if-gez vA, :cond_**"   	如果vA大于等于0则跳转到:cond_**
"if-gtz vA, :cond_**"   	如果vA大于0则跳转到:cond_**
"if-lez vA, :cond_**"    	如果vA小于等于0则跳转到:cond_**

 z 既可以表示zero（0） 也可以是null、或者false;具体看逻辑。
```

## 四、switch分支语句

```
.method private packedSwitch(I)Ljava/lang/String;
    .locals 1
    .parameter "i"
    .prologue
    .line 21
    const/4 v0, 0x0
    .line 22
    .local v0, str:Ljava/lang/String;  #v0为字符串，0表示null
    packed-switch p1, :pswitch_data_0  #packed-switch分支，pswitch_data_0指定case区域
    .line 36
    const-string v0, "she is a person"  #default分支
    .line 39
    :goto_0      #所有case的出口
    return-object v0 #返回字符串v0
    .line 24
    :pswitch_0    #case 0
    const-string v0, "she is a baby"
    .line 25
    goto :goto_0  #跳转到goto_0标号处
    .line 27
    :pswitch_1    #case 1
    const-string v0, "she is a girl"
    .line 28
    goto :goto_0  #跳转到goto_0标号处
    .line 30
    :pswitch_2    #case 2
    const-string v0, "she is a woman"
    .line 31
    goto :goto_0  #跳转到goto_0标号处
    .line 33
    :pswitch_3    #case 3
    const-string v0, "she is an obasan"
    .line 34
    goto :goto_0  #跳转到goto_0标号处
    .line 22
    nop
    :pswitch_data_0
    .packed-switch 0x0    #case  区域，从0开始，依次递增
        :pswitch_0  #case 0
        :pswitch_1  #case 1
        :pswitch_2  #case 2
        :pswitch_3  #case 3
    .end packed-switch
.end method
```

* packed-switch 指令。p1为传递进来的 int 类型的数值，pswitch_data_0 为case 区域，在 case 区域中，第一条指令“.packed-switch”指定了比较的初始值为0 ，pswitch_0~ pswitch_3分别是比较结果为“case 0 ”到“case 3 ”时要跳转到的地址。可以发现，标号的命名采用 pswitch_ 开关，后面的数值为 case 分支需要判断的值，并且它的值依次递增。再来看看这些标号处的代码，每个标号处都使用v0 寄存器初始化一个字符串，然后跳转到了goto_0 标号处，可见goto_0 是所有的 case 分支的出口。另外，“.packed-switch”区域指定的case 分支共有4 条，对于没有被判断的 default 分支，会在代码的 packed-switch指令下面给出。
* 至此，有规律递增的 switch 分支就算是搞明白了。最后，将这段 smali
代码整理为Java代码如下。

```
private String packedSwitch(int i) {
    String str = null;
    switch (i) {
        case 0:
            str = "she is a baby";
            break;
        case 1:
            str = "she is a girl";
            break;
        case 2:
            str = "she is a woman";
            break;
        case 3:
            str = "she is an obasan";
            break;
        default:
            str = "she is a person";
            break;
    }
    return str;
}
```

现在我们来看看无规律的case 分支语句代码会有什么不同

```
.method private sparseSwitch(I)Ljava/lang/String;
    .locals 1
    .parameter "age"
    .prologue
    .line 43
    const/4 v0, 0x0
    .line 44
    .local v0, str:Ljava/lang/String;
    sparse-switch p1, :sswitch_data_0  # sparse-switch分支，sswitch_data_0指定case区域
    .line 58
    const-string v0, "he is a person"  #case default
    .line 61
    :goto_0    #case 出口
    return-object v0  #返回字符串
    .line 46
    :sswitch_0    #case 5
    const-string v0, "he is a baby"
    .line 47
    goto :goto_0 #跳转到goto_0标号处
    .line 49
    :sswitch_1    #case 15
    const-string v0, "he is a student"
    .line 50
    goto :goto_0 #跳转到goto_0标号处
    .line 52
    :sswitch_2    #case 35
    const-string v0, "he is a father"
    .line 53
    goto :goto_0 #跳转到goto_0标号处
    .line 55
    :sswitch_3    #case 65
    const-string v0, "he is a grandpa"
    .line 56
    goto :goto_0 #跳转到goto_0标号处
    .line 44
    nop
    :sswitch_data_0
    .sparse-switch            #case 区域
        0x5 -> :sswitch_0     #case 5(0x5)
        0xf -> :sswitch_1     #case 15(0xf)
        0x23 -> :sswitch_2    #case 35(0x23)
        0x41 -> :sswitch_3    #case 65(0x41)
    .end sparse-switch
.end method
```

* 按照分析packed-switch 的方法，我们直接查看 sswitch_data_0 标号处的内容。可以看到“.sparse-switch ”指令没有给出初始case 的值，所有的case 值都使用“case 值 -> case 标号”的形式给出。此处共有4 个case ，它们的内容都是构造一个字符串，然后跳转到goto_0 标号处，代码架构上与packed-switch 方式的 switch 分支一样。

* 最后，将这段smali 代码整理为Java 代码如下。

```
private String sparseSwitch(int age) {
    String str = null;
    switch (age) {
        case 5:
            str = "he is a baby";
            break;
        case 15:
            str = "he is a student";
            break;
        case 35:
            str = "he is a father";
            break;
        case 65:
            str = "he is a grandpa";
            break;
        default:
            str = "he is a person";
            break;
    }
    return str;
}
```
 

## 五、try/catch 语句

```
.method private tryCatch(ILjava/lang/String;)V
    .locals 10
    .parameter "drumsticks"
    .parameter "peple"
    .prologue
    const/4 v9, 0x0
                                                                                                                                                              .line 19
    try_start_0                                                              # 第1个try开始
    invoke-static {p2}, Ljava/lang/Integer;->parseInt(Ljava/lang/String;)I   #将第2个参数转换为int 型
    :try_end_0                                                               # 第1个try结束
    .catch Ljava/lang/NumberFormatException; {:try_start_0 .. :try_end_0} : catch_1  # catch_1
     move-result v1          #如果出现异常这里不会执行，会跳转到catch_1标号处
                                                                                                                                                               .line 21
    .local v1, i:I            #.local声明的变量作用域在.local声明与.end local 之间
   :try_start_1               #第2个try 开始
    div-int v2, p1, v1        # 第1个参数除以第2个参数
    .line 22
    .local v2, m:I    
    mul-int v5, v2, v1        #m * i
    sub-int v3, p1, v5        #v3  = p1 - v5
    .line 23
    .local v3, n:I
    const-string v5, "\u5171\u6709%d\u53ea\u9e21\u817f\uff0c%d
        \u4e2a\u4eba\u5e73\u5206\uff0c\u6bcf\u4eba\u53ef\u5206\u5f97%d
        \u53ea\uff0c\u8fd8\u5269\u4e0b%d\u53ea"   # 格式化字符串
    const/4 v6, 0x4
    new-array v6, v6, [Ljava/lang/Object;
    const/4 v7, 0x0
    .line 24
    invoke-static {p1}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v8
    aput-object v8, v6, v7
    const/4 v7, 0x1
    invoke-static {v1}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v8
    aput-object v8, v6, v7
    const/4 v7, 0x2
    invoke-static {v2}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v8
    aput-object v8, v6, v7
    const/4 v7, 0x3
    invoke-static {v3}, Ljava/lang/Integer;->valueOf(I)Ljava/lang/Integer;
    move-result-object v8
 
    aput-object v8, v6, v7
    .line 23
    invoke-static {v5, v6}, Ljava/lang/String;
        ->format(Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/String;
    move-result-object v4
    .line 25
    .local v4, str:Ljava/lang/String;
    const/4 v5, 0x0
    invoke-static {p0, v4, v5}, Landroid/widget/Toast;
        ->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)
        Landroid/widget/Toast;
    move-result-object v5
    invoke-virtual {v5}, Landroid/widget/Toast;->show()V # 使用Toast 显示格式化后的结果
   :try_end_1                                            #第2个try 结束
    .catch Ljava/lang/ArithmeticException; {:try_start_1 .. :try_end_1} : catch_0    # catch_0
    .catch Ljava/lang/NumberFormatException; {:try_start_1 .. :try_end_1} : catch_1  # catch_1
    .line 33
    .end local v1           #i:I
    .end local v2           #m:I
    .end local v3           #n:I
    .end local v4           #str:Ljava/lang/String;
    :goto_0 
    return-void             # 方法返回
    .line 26 
    .restart local v1       #i:I
    :catch_0   
    move-exception v0
    .line 27
    .local v0, e:Ljava/lang/ArithmeticException;
                                                                                                                                                              :try_start_2                                       #第3个try 开始
    const-string v5, "\u4eba\u6570\u4e0d\u80fd\u4e3a0"           #“人数不能为0”
    const/4 v6, 0x0
    invoke-static {p0, v5, v6}, Landroid/widget/Toast;
        ->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)
        Landroid/widget/Toast;
    move-result-object v5
    invoke-virtual {v5}, Landroid/widget/Toast;->show()V         #使用Toast 显示异常原因
    :try_end_2                                                   #第3个try 结束
    .catch Ljava/lang/NumberFormatException; {:try_start_2 .. :try_end_2} :catch_1
     goto :goto_0 #返回
    .line 29
    .end local v0           #e:Ljava/lang/ArithmeticException;
    .end local v1           #i:I
    :catch_1 
    move-exception v0
    .line 30
    .local v0, e:Ljava/lang/NumberFormatException;
    const-string v5, "\u65e0\u6548\u7684\u6570\u503c\u5b57\u7b26\u4e32" 
    #“无效的数值字符串”
    invoke-static {p0, v5, v9}, Landroid/widget/Toast;
        ->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)
        Landroid/widget/Toast;
    move-result-object v5
    invoke-virtual {v5}, Landroid/widget/Toast;->show()V # 使用Toast 显示异 常原因
    goto :goto_0                                         #返回
.end method
```

* 代码中的try语句块使用try_start_开头的标号注明，以try_end_开头的标号结束。第一个try语句的开头标号为try_start_0，结束标号为 try_end_0。使用多个try语句块时标号名称后面的数值依次递增，本实例代码中最多使用到了try_end_2。
在try_end_0 标号下面使用“.catch”指令指定处理到的异常类型与catch的标号，格式如下：
`.catch < 异常类型> {<try起始标号> .. <try 结束标号>} <catch标号>`

* 查看catch_1标号处的代码发现，当转换 String 到int 时发生异常会弹出“无效的数值字符串”的提示。对于代码中的汉字，baksmali 在反编译时将其使用Unicode进行编码，因此，在阅读前需要使用相关的编码转换工具进行转换。

* 仔细阅读代码会发现在try_end_1标号下面使用“.catch”指令定义了 catch_0与catch_1两个catch。catch_0标号的代码开头又有一个标号为try_start_2的try 语句块，其实这个try语句块是虚构的，假如下面的代码。

```
private void a() {
    try {
        ……
        try {
            ……
        } catch (XXX) {
            ……
        }
    } catch (YYY) {
        ……
    }      
}
```
* 当执行内部的try语句时发生了异常，如果异常类型为XXX，则内部catch就会捕捉到并执行相应的处理代码，如果异常类型不是 XXX，那么就会到外层的 catch中去查找异常处理代码，这也就是为什么实例的try_end_1标号下面会有两个catch的原因，另外，如果在执行XXX异常的处理代码时又发生了异常，这个时候该怎么办？此时这个异常就会扩散到外层的catch中去，由于XXX异常的外层只有一个YYY的异常处理，这时会判断发生的异常是否为YYY类型，如果是就会进行处理，不是则抛给应用程序。回到本实例中来，如果在执行内部的ArithmeticException异常处理时再次发生别的异常，就会调用外层的 catch进行异常捕捉，因此在try_end_2标号下面有一个 catch_1就很好理解了。
最后，将这段 smali 代码整理为Java 代码如下。

```
private void tryCatch(int drumsticks, String peple) {
        try {
            int i = Integer.parseInt(peple);
            try {
                int m = drumsticks / i;
                int n = drumsticks - m * i;
                String str = String.format("共有%d只鸡腿，%d个人平分，每人可分得%d只，还剩下%d只",drumsticks, i, m, n);
                Toast.makeText(MainActivity.this, str,Toast.LENGTH_SHORT).show();
            } catch (ArithmeticException e) {
                Toast.makeText(MainActivity.this, " 人数不能为0",Toast.LENGTH_SHORT).show();
            }
        } catch (NumberFormatException e) {
            Toast.makeText(MainActivity.this, " 无效的数值字符串",Toast.LENGTH_SHORT).show();
        }        
}
```

### finally语句块
源码：

```
try {
            ServerSocket serverSocket= new ServerSocket(10000);
            Socket socket=serverSocket.accept();
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
           int abc=5;
           Toast.makeText(this, "sssss ", Toast.LENGTH_SHORT).show();
    }
```

* finally 语句块作用：执行一些必要代码。即不管出现异常与否，在finally中的代码都会被执行
执行时机：针对所有catch语句之后，退出方法之前将被执行（即先执行catch里面的代码，但在throw之前将转向finally）。finally中返回的结果将可以覆盖catch中返回的结果
对应的smail代码如下：

```
:try_start_0
    new-instance v2, Ljava/net/ServerSocket;        #ServerSocket v2 = null;   
    const/16 v3, 0x2710                             # v3 = 10000;
    invoke-direct {v2, v3}, Ljava/net/ServerSocket;-><init>(I)V  # v2 = new ServerSocket(v3);
    .line 21
    .local v2, serverSocket:Ljava/net/ServerSocket;
    invoke-virtual {v2}, Ljava/net/ServerSocket;->accept()Ljava/net/Socket; # v2.accept( );
    :try_end_0
    .catchall {:try_start_0 .. :try_end_0} :catchall_0  
//上一句处理start_0对应的异常块是catchall_0   也就是finally
    .catch Ljava/io/IOException; {:try_start_0 .. :try_end_0} :catch_0
//上一句处理start_0对应的异常块是catch_0，catch_0异常块先执行，之后再执行catchall_0
```

相对应的smali代码为： 

```
:try_start_0
    new-instance v2, Ljava/net/ServerSocket;
    const/16 v3, 0x2710
    invoke-direct {v2, v3}, Ljava/net/ServerSocket;-><init>(I)V
    .line 21
    .local v2, serverSocket:Ljava/net/ServerSocket;
    invoke-virtual {v2}, Ljava/net/ServerSocket;->accept()Ljava/net/Socket;
    :try_end_0

    .catchall {:try_start_0 .. :try_end_0} :catchall_0
    .catch Ljava/io/IOException; {:try_start_0 .. :try_end_0} :catch_0
    .line 27
    const/4 v0, 0x5      #正常流程 即未发生异常
    .line 28
    .local v0, abc:I
    const-string v3, "sssss "
    invoke-static {p0, v3, v5}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;
    move-result-object v3
    invoke-virtual {v3}, Landroid/widget/Toast;->show()V
    .line 32
    .end local v2        #serverSocket:Ljava/net/ServerSocket;
    :goto_0
    return-void
    .line 22
    .end local v0        #abc:I
  
   :catch_0              #当发生异常时执行
    move-exception v1
    .line 24
    .local v1, e:Ljava/io/IOException;
    
    :try_start_1
      invoke-virtual {v1}, Ljava/io/IOException;->printStackTrace()V
    :try_end_1
    .catchall {:try_start_1 .. :try_end_1} :catchall_0    #异常部分执行完毕，转而执行finally
  
    .line 27
    const/4 v0, 0x5
    .line 28
    .restart local v0       #abc:I
    const-string v3, "sssss "
    invoke-static {p0, v3, v5}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;
    move-result-object v3
    invoke-virtual {v3}, Landroid/widget/Toast;->show()V
    goto :goto_0
    .line 25
    .end local v0           #abc:I
    .end local v1           #e:Ljava/io/IOException;
    
  #finally代码定义部分
    :catchall_0
    move-exception v3
    .line 27
    const/4 v0, 0x5
    .line 28
    .restart local v0       #abc:I
    const-string v4, "sssss "
    invoke-static {p0, v4, v5}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;
    move-result-object v4
    invoke-virtual {v4}, Landroid/widget/Toast;->show()V
    .line 30
    throw v3
```

## 六、for循环

```
# virtual methods
.method public onClick(Landroid/view/View;)V
.locals 9
.parameter “v”

.prologue
.line 36
invoke-virtual {p1}, Landroid/view/View;->getId()I  # 非静态方法参数中隐含的第一个参数p0为this指针, p1为第一个参数, 即View对象

move-result v6     # 把上次的计算结果给第七个寄存器,v6=p1.getId(), v6中为View对象的id

packed-switch v6, :pswitch_data_0    # switch(v6)

# —————– 程序出口开始 ——————
.line 58
:goto_0         # for循环出口
return-void     # return;
# —————– 程序出口结束 ——————

# —————– 获取控件内容开始 ——————
.line 39
:pswitch_0
iget-object v6, p0, Lcom/sanwho/crackdemo/ForActivity$1;->this$0:Lcom/sanwho/crackdemo/ForActivity;  # v6保存this指针

const v7, 0x7f080001   # v7 = txtValue1, 该id保存在public.xml中

invoke-virtual {v6, v7}, Lcom/sanwho/crackdemo/ForActivity;->findViewById(I)Landroid/view/View; # findViewById(txtValue1)

move-result-object v4  # v4为txtValue1对应的View对象

check-cast v4, Landroid/widget/EditText;  # 将View对象转换成EditText, 完成后v4中是txtValue1对象, 失败会抛出ClassCastException异常

.line 40
.local v4, txtValue1:Landroid/widget/EditText;
iget-object v6, p0, Lcom/sanwho/crackdemo/ForActivity$1;->this$0:Lcom/sanwho/crackdemo/ForActivity;

const v7, 0x7f080003  # v7 = txtValue2

invoke-virtual {v6, v7}, Lcom/sanwho/crackdemo/ForActivity;->findViewById(I)Landroid/view/View;

move-result-object v5  # v5为txtValue2对应的View对象

check-cast v5, Landroid/widget/EditText;  # 将View对象转换成EditText, 完成后v5中是txtValue2对象

.line 41
.local v5, txtValue2:Landroid/widget/EditText;
invoke-virtual {v4}, Landroid/widget/EditText;->getText()Landroid/text/Editable;    # 根据.line 39处可知，v4中为txtValue1对象

move-result-object v6   # v6 = txtValue1.getText();

invoke-interface {v6}, Landroid/text/Editable;->toString()Ljava/lang/String;

move-result-object v6   # v6 = txtValue1.getText().toString();

invoke-static {v6}, Ljava/lang/Integer;->parseInt(Ljava/lang/String;)I

move-result v1    # v1 = Integer.parseInt(v6); 也就是起始数值

.line 42
.local v1, from:I
invoke-virtual {v5}, Landroid/widget/EditText;->getText()Landroid/text/Editable;    # 根据.line 40处可知，v5中为txtValue2对象

move-result-object v6  # v6 = txtValue2.getText();

invoke-interface {v6}, Landroid/text/Editable;->toString()Ljava/lang/String;

move-result-object v6  # v6 = txtValue2.getText().toString();

invoke-static {v6}, Ljava/lang/Integer;->parseInt(Ljava/lang/String;)I

move-result v0    # v0 = Integer.parseInt(v6); 也就是结束数值

# —————– 获取控件内容结束 ——————

.line 43
.local v0, end:I
if-le v1, v0, :cond_0   # if v1 <= v0, 即起始数值 <= 结束数值, 则跳到cond_0

# —————– 起始数值 > 结束数值时开始 ——————
.line 45
iget-object v6, p0, Lcom/sanwho/crackdemo/ForActivity$1;->this$0:Lcom/sanwho/crackdemo/ForActivity;

const-string v7, “\u8d77\u59cb\u6570\u503c\u4e0d\u80fd\u5927\u4e8e\u7ed3\u675f\u6570\u503c!”  # 起始数值不能大于结束数值

#calls: Lcom/sanwho/crackdemo/ForActivity;->MessageBox(Ljava/lang/String;)V
invoke-static {v6, v7}, Lcom/sanwho/crackdemo/ForActivity;->access$0(Lcom/sanwho/crackdemo/ForActivity;Ljava/lang/String;)V

goto :goto_0

# —————– 起始数值 > 结束数值时结束 ——————

# —————– 起始数值 <= 结束数值时开始 —————–
.line 49
:cond_0
const/4 v3, 0x0   # v3 = 0, 即int sum = 0;

.line 50
.local v3, sum:I
move v2, v1       # v2 = v1, v2即源码中的i变量

.local v2, i:I
:goto_1           # for循环主要入口
if-le v2, v0, :cond_1   # if 当前数值 <= 结束数值, 跳到cond_1;  否则循环结束, 显示累加结果

.line 54
iget-object v6, p0, Lcom/sanwho/crackdemo/ForActivity$1;->this$0:Lcom/sanwho/crackdemo/ForActivity;  # v6指向MessageBox方法

new-instance v7, Ljava/lang/StringBuilder;    # v7为StringBuilder对象

const-string v8, “\u7d2f\u52a0\u7ed3\u679c\uff1a”  # v8 = “累加结果：”

invoke-direct {v7, v8}, Ljava/lang/StringBuilder;-><init>(Ljava/lang/String;)V  # 以v8为参数调用StringBuilder构造函数

invoke-static {v3}, Ljava/lang/Integer;->toString(I)Ljava/lang/String; # 把int型的sum值转成字符串

move-result-object v8   # v8 = Integer.toString(v3); 此时v8中为sum的值

invoke-virtual {v7, v8}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;   # 把累加结果和sum的值进行追加

move-result-object v7  # v7 为 “累加结果：” + Integer.toString(sum)的StringBuilder对象;

invoke-virtual {v7}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;  # 将v7转为字符串对象

move-result-object v7   # v7 = “累加结果：” + Integer.toString(sum);

#calls: Lcom/sanwho/crackdemo/ForActivity;->MessageBox(Ljava/lang/String;)V
invoke-static {v6, v7}, Lcom/sanwho/crackdemo/ForActivity;->access$0(Lcom/sanwho/crackdemo/ForActivity;Ljava/lang/String;)V  # 调用MessageBox显示字符串

goto :goto_0  # 跳到goto_0
# —————– 起始数值 <= 结束数值时结束 —————–

.line 52
:cond_1           # 加1操作入口
add-int/2addr v3, v2   # v3 = v3 + v2, 即sum += i

.line 50
add-int/lit8 v2, v2, 0x1  # v2 = v2 + 1, , 即i = i + 1

goto :goto_1   # 跳到for循环入口继续比对

.line 36
nop

:pswitch_data_0
.packed-switch 0x7f080004
:pswitch_0
.end packed-switch
.end method
```
源码

```
Button.OnClickListener onClickListener = new Button.OnClickListener()
{
    @Override
    public void onClick(View v)
    {
        switch (v.getId())
        {
        case R.id.btnSubmit:
            EditText txtValue1 = (EditText) findViewById(R.id.txtValue1);
            EditText txtValue2 = (EditText) findViewById(R.id.txtValue2);
            int from = Integer.parseInt(txtValue1.getText().toString());
            int end = Integer.parseInt(txtValue2.getText().toString());
            if (from > end){
                MessageBox("起始数值不能大于结束数值!");
            }
            else
            {
                 int sum = 0;
                 for (int i = from; i <= end; i++){
                    
                     sum += i;
                 }
                 MessageBox("累加结果：" + Integer.toString(sum));
            }
            break;
        }
    }
};

private void MessageBox(String str)
{        
    Toast.makeText(this, str, Toast.LENGTH_LONG).show();
}
```

## 七、内部类
Java 语言允许在一个类的内部定义另一个类，这种在类中定义的类被称为内部类（Inner Class）。内部类可分为成员内部类、静态嵌套类、方法内部类、匿名内部类。前面我们曾经说过，baksmali 在反编译dex 文件的时候，会为每个类单独生成了一个 smali 文件，内部类作为一个独立的类，它也拥有自己独立的smali 文件，只是内部类的文件名形式为“[外部类]$[内部类].smali ”，例如下面的类。

```
class Outer {
   class Inner{}
}

```

baksmali 反编译上述代码后会在同一目录生成两个文件：Outer.smali 与Outer$Inner.smali。

```
public class MainActivity extends Activity {    
    private Button btnAnno;
    private Button btnCheckSN;
    private EditText edtSN;  
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btnAnno = (Button) findViewById(R.id.btn_annotation);
        btnCheckSN = (Button) findViewById(R.id.btn_checksn);
        edtSN = (EditText) findViewById(R.id.edt_sn);
        btnAnno.setOnClickListener(new OnClickListener() {
           @Override
            public void onClick(View v) {
                getAnnotations();                
            }
        });
        
        btnCheckSN.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                SNChecker checker = new SNChecker(edtSN.getText().toString());
                String str = checker.isRegistered() ? "注册码正确" : "注册码错误";
                Toast.makeText(MainActivity.this, str, Toast.LENGTH_SHORT).show();
            }
        });
    }
    
    private void getAnnotations() {
        try {
            Class<?> anno = Class.forName("com.droider.anno.MyAnno");
            if (anno.isAnnotationPresent(MyAnnoClass.class)) {
                MyAnnoClass myAnno = anno.getAnnotation(MyAnnoClass.class);
                Toast.makeText(this, myAnno.value(), Toast.LENGTH_SHORT).show();
            }
            Method method = anno.getMethod("outputInfo", (Class[])null);
            if (method.isAnnotationPresent(MyAnnoMethod.class)) {
                MyAnnoMethod myMethod = method.getAnnotation(MyAnnoMethod.class);
                String str = myMethod.name() + " is " + myMethod.age() + " years old.";
                Toast.makeText(this, str, Toast.LENGTH_SHORT).show();
            }
            Field field = anno.getField("sayWhat");
            if (field.isAnnotationPresent(MyAnnoField.class)) {
                MyAnnoField myField = field.getAnnotation(MyAnnoField.class);
                Toast.makeText(this, myField.info(), Toast.LENGTH_SHORT).show();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.activity_main, menu);
        return true;
    }
    
    public class SNChecker {
        private String sn;
        public SNChecker(String sn) {
            this.sn = sn;
        }
        
        public boolean isRegistered() {
            boolean result = false;
            char ch = '\0';
            int sum = 0;
            if (sn == null || (sn.length() < 8)) return result;
            int len = sn.length();
            if (len == 8) {
                ch = sn.charAt(0);
                switch (ch) {
                    case 'a':
                    case 'f':
                        result = true;
                        break;
                    default:
                        result = false;
                        break;
                }
                if (result) {
                    ch = sn.charAt(3);
                    switch (ch) {
                        case '1':
                        case '2':
                        case '3':
                        case '4':
                        case '5':
                            result = true;
                            break;
                        default:
                            result = false;
                            break;
                    }
                }
            } else if (len == 16) {
                for (int i = 0; i < len; i++) {
                    char chPlus = sn.charAt(i);
                    sum += (int) chPlus;
                }
                result = ((sum % 6) == 0) ? true : false;
            }
            return result;
        }
    }
}
```

MainActivity$ SNChecker.smali 文件，这个SNChecker 就是MainActivity的一个内部类。打开这个文件，代码结构如下。

```
.class public Lcom/droider/crackme0502/MainActivity$SNChecker;
.super Ljava/lang/Object;
.source "MainActivity.java"


# annotations
.annotation system Ldalvik/annotation/EnclosingClass;
    value = Lcom/droider/crackme0502/MainActivity;
.end annotation

.annotation system Ldalvik/annotation/InnerClass;
    accessFlags = 0x1
    name = "SNChecker"
.end annotation


# instance fields
.field private sn:Ljava/lang/String;

.field final synthetic this$0:Lcom/droider/crackme0502/MainActivity;


# direct methods
.method public constructor <init>(Lcom/droider/crackme0502/MainActivity;Ljava/lang/String;)V
    .locals 0
    .parameter
    .parameter "sn"

    .prologue
    .line 83
    iput-object p1, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->this$0:Lcom/droider/crackme0502/MainActivity;

    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    .line 84
    iput-object p2, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    .line 85
    return-void
.end method


# virtual methods
.method public isRegistered()Z
    .locals 10

    .prologue
    const/16 v9, 0x8

    const/4 v7, 0x0

    .line 88
    const/4 v4, 0x0

    .line 89
    .local v4, result:Z
    const/4 v0, 0x0

    .line 90
    .local v0, ch:C
    const/4 v6, 0x0

    .line 91
    .local v6, sum:I
    iget-object v8, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    if-eqz v8, :cond_0

    iget-object v8, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    invoke-virtual {v8}, Ljava/lang/String;->length()I

    move-result v8

    if-ge v8, v9, :cond_1

    :cond_0
    move v5, v4

    .line 126
    .end local v4           #result:Z
    .local v5, result:I
    :goto_0
    return v5

    .line 92
    .end local v5           #result:I
    .restart local v4       #result:Z
    :cond_1
    iget-object v8, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    invoke-virtual {v8}, Ljava/lang/String;->length()I

    move-result v3

    .line 93
    .local v3, len:I
    if-ne v3, v9, :cond_3

    .line 94
    iget-object v8, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    invoke-virtual {v8, v7}, Ljava/lang/String;->charAt(I)C

    move-result v0

    .line 95
    sparse-switch v0, :sswitch_data_0

    .line 101
    const/4 v4, 0x0

    .line 104
    :goto_1
    if-eqz v4, :cond_2

    .line 105
    iget-object v7, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    const/4 v8, 0x3

    invoke-virtual {v7, v8}, Ljava/lang/String;->charAt(I)C

    move-result v0

    .line 106
    packed-switch v0, :pswitch_data_0

    .line 115
    const/4 v4, 0x0

    :cond_2
    :goto_2
    move v5, v4

    .line 126
    .restart local v5       #result:I
    goto :goto_0

    .line 98
    .end local v5           #result:I
    :sswitch_0
    const/4 v4, 0x1

    .line 99
    goto :goto_1

    .line 112
    :pswitch_0
    const/4 v4, 0x1

    .line 113
    goto :goto_2

    .line 119
    :cond_3
    const/16 v8, 0x10

    if-ne v3, v8, :cond_2

    .line 120
    const/4 v2, 0x0

    .local v2, i:I
    :goto_3
    if-lt v2, v3, :cond_4

    .line 124
    rem-int/lit8 v8, v6, 0x6

    if-nez v8, :cond_5

    const/4 v4, 0x1

    :goto_4
    goto :goto_2

    .line 121
    :cond_4
    iget-object v8, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    invoke-virtual {v8, v2}, Ljava/lang/String;->charAt(I)C

    move-result v1

    .line 122
    .local v1, chPlus:C
    add-int/2addr v6, v1

    .line 120
    add-int/lit8 v2, v2, 0x1

    goto :goto_3

    .end local v1           #chPlus:C
    :cond_5
    move v4, v7

    .line 124
    goto :goto_4

    .line 95
    :sswitch_data_0
    .sparse-switch
        0x61 -> :sswitch_0
        0x66 -> :sswitch_0
    .end sparse-switch

    .line 106
    :pswitch_data_0
    .packed-switch 0x31
        :pswitch_0
        :pswitch_0
        :pswitch_0
        :pswitch_0
        :pswitch_0
    .end packed-switch
.end method
```
* 发现它有两个注解定义块“Ldalvik/annotation/EnclosingClass;”与“Ldalvik/annotation/ InnerClass; ”、两个实例字段sn 与this$0 、一个直接方法 init()、一个虚方法isRegistered() 。注解定义块我们稍后进行讲解。先看它的实例字段，sn 是字符串类型，this$0 是MainActivity类型，synthetic 关键字表明它是“合成”的，那 this$0 到底是个什么东西呢？

* 其实this$0 是内部类自动保留的一个指向所在外部类的引用。左边的 this 表示为父类的引用，右边的数值0 表示引用的层数。我们看下面的类。

```
public class Outer {    //this$0 
     public class FirstInner {        //this$1 
            public class SecondInner {      //this$2 
                public class ThirdInner {
                } 
        } 
    }
}
```

每往里一层右边的数值就加一，如 ThirdInner类访问 FirstInner 类的引用为this$1 。在生成的反汇编代码中，this$X 型字段都被指定了synthetic 属性，表明它们是被编译器合成的、虚构的，代码的作者并没有声明该字段。

我们再看看MainActivity$SNChecker的构造函数，看它是如何初始化的。代码如下。

```
# direct methods
.method public constructor <init>(Lcom/droider/crackme0502/MainActivity;Ljava/lang/String;)V
    .locals 0
    .parameter       #第一个参数MainActivity引用
    .parameter "sn"  #第二个参数字符串sn

    .prologue
    .line 83
    #将MainActivity引用赋值给this$0
    iput-object p1, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->this$0:Lcom/droider/crackme0502/MainActivity;

    #调用默认的构造函数
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    .line 84
    #将sn字符串的值赋给sn字段
    iput-object p2, p0, Lcom/droider/crackme0502/MainActivity$SNChecker;->sn:Ljava/lang/String;

    .line 85
    return-void
.end method
```

* 对于一个非静态的方法而言，会隐含的使用p0寄存器当作类的this 引用。因此，这里的确是使用了3 个寄存器：p0表示MainActivity$SNChecker自身的引用，p1表示MainActivity的引用，p2表示sn 字符串。另外，从 MainActivity$SNChecker的构造函数可以看出，内部类的初始化共有以下 3 个步骤：首先是保存外部类的引用到本类的一个 synthetic字段中，以便内部类的其它方法使用，然后是调用内部类的父类的构造函数来初始化父类，最后是对内部类自身进行初始化。


* 一个方法中指定的寄存器个数 
	* 在一个方法（method）中有两中方式指定有多少个可用的寄存器。指令.registers指令指定了在这个方法中有多少个可用的寄存器，指令.locals指明了在这个方法中非参（non-parameter）寄存器的数量。然而寄存器的总数也包括保存方法参数的寄存器。

* 参数是如何传递的？
	* 当一个方法被调用时，该方法的参数被保存在最后N个寄存器中。如果一个方法有2个参数和5个寄存器(V0-V4)，参数将被保存在最后的2个寄存器内V3和V4.

* 非静态方法的第一个参数，总是被方法调用的对象。
	* 例如，你写了一个非静态方法LMyObject;->callMe(II)V。这个方法有2个int参数，但在这两个整型参数前面还有一个隐藏的参数LMyObject；所以这个方法总共有3个参数。
	* 比如说，在方法中指定有5个寄存器(V0-V4)，只用.register指令指定5个，或者使用.locals指令指定2个（2个local寄存器+3个参数寄存器）。该方法被调用的时候，调用方法的对象（即this引用）会保存在V2中，第一个参数在V3中，第二个参数在v4中。
	* 除了不包含this隐藏参数，对于静态方法都是相同的。

* 寄存器名称
	* 有两种寄存器的命名方式，对于参数寄存器有普通的V命名方式和P命名方式。在方法（method）中第一个参数寄存器，是使用P方式命名的第一个寄存器，让我们回到前面的例子中，有三个参数和5个寄存器，下面的这个表显示了对每个寄存器的普通V命名方式，后面是P方式命名的参数寄存器。


v0		| 	|the first local register
:---	|:---|:---	
v1	 	|		|the second local register
v2		|p0		|the first parameter register
v3		|p1		|the second parameter register
v4		|p2		|the third parameter register

* You can reference parameter registers by either name - it makes no difference.

* 引入参数寄存器的目的
	* P命名方式被引入去解决，在编辑smail代码时候共同的烦恼。

	* 假设你有一个方法（mehtod），这个方法带有一些参数，并且你需要添加一些代码到这个方法中，这时发现需要一些额外的寄存器，你会想“没有什么大不了的。我只需要使用.registers指令添加寄存器数量就可以了。”
	* 不幸的是没有想象的那么容易，请记住，方法中方法的参数被保存在最后的寄存器里。如果你增加了寄存器的数量，达到让寄存器中的参数被传入的目的。所以你不得不使用.registers指令重新分配参数寄存器的编号。
	* 但如果在方法中P命名方式，被用来引用参数寄存器。你将很容易的在方法中去修改寄存器数量，而不用去担心现有寄存器的编号。

* 注意：在默认的baksmali中，参数寄存器将使用P命名方式，如果出于某种原因你要禁用P命名方式，而要强制使用V命名方式，应当使用-p/--no-parameter-registers选项。

* Long/Double values
	* 正如前面提到的，long和double类型都是64位，需要2个寄存器。当你引用参数的时候一定要记住，例如：你有一个非静态方法LMyObject;->MyMethod(IJZ)V,LMyObject方法的参数为int、long、bool。所以这个方法的所有参数需要5个寄存器。


		|p0			|this	|
		|:------	|:----	|	
		|p1			|I	 	|
		|p2, p3	|J		|
		|p4			|Z		|	
	
另外当你调用方法后，你必须在寄存器列表，调用指令中指明，两个寄存器保存了double-wide宽度的参数。

### 关于几个调用方法指令： invoke-virtual、invoke-direct、invoke-super介绍。

* 涉及到Java强大的动态扩展能力，这一特性使得可以在类运行期间才能确定某些目标方法的实际引用，称为动态连接；也有一部分方法的符号引用在类加载阶段或第一次使用时转化为直接引用，这种转化称为静态解析。

* 在Java语言中，符合“编译器可知，运行期不可变”这个要求的方法主要有静态方法和私有方法两大类，前者与类型直接关联，后者在外部不可被访问，这两种方法都不可能通过继承或别的方式重写出其他的版本，因此它们都适合在类加载阶段进行解析。 

* invoke-static 是类静态方法的调用，编译时，静态确定的；
* invoke-virtual 虚方法调用，调用的方法运行时确认实际调用，和实例引用的实际对象有关，动态确认的，一般是带有修饰符protected或public的方法；
* invoke-direct 没有被覆盖方法的调用，即不用动态根据实例所引用的调用，编译时，静态确认的，一般是private或<init>方法；
* invoke-super 直接调用父类的虚方法，编译时，静态确认的。
* invokeinterface 调用接口方法，调用的方法运行时确认实际调用，即会在运行时才确定一个实现此接口的对象。

参考：
https://www.cnblogs.com/eustoma/p/8991297.html