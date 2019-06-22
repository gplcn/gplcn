---
layout: post
title: smali语法1
categories: [Android]
description: smali 语法指令
keywords: smali
---

# ***smali***语法 ***1***

	    表中的vx、vy、vz表示某个Dalvik寄存器。根据不同指令可以访问16、256或64K寄存器。
	
	表中lit4、lit8、lit16、lit32、lit64表示字面值（直接赋值），数字是值所占用位的长度。
	
	long和double型的值占用两个寄存器，例：一个在v0寄存器的double值实际占用v0,v1两个寄存器。
	
	boolean值的存储实际是1和0，1为真、0为假；boolean型的值实际是转成int型的值进行操作。
	
	所有例子的字节序都采用高位存储格式，例：0F00 0A00的编译为0F, 00, 0A, 00 存储。
[Dalvik 字节码](https://source.android.google.cn/devices/tech/dalvik/dalvik-bytecode)

<table style="width: 100%;" border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="12%">
<p align="center"><strong>Opcode</strong></p>
<p align="center"><strong>操作码</strong><strong>(hex)</strong></p>
</td>
<td width="16%">
<p align="center"><strong>Opcode name</strong></p>
<p align="center"><strong>操作码名称</strong></p>
</td>
<td width="29%">
<p align="center"><strong>Explanation</strong></p>
<p align="center"><strong>说明</strong></p>
</td>
<td width="41%">
<p align="center"><strong>Example</strong></p>
<p align="center"><strong>示例</strong></p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">00</p>
</td>
<td width="16%">
<p>nop</p>
</td>
<td width="29%">
<p>无操作</p>
</td>
<td width="41%">
<p>0000 - nop</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">01</p>
</td>
<td width="16%">
<p>move vx, vy</p>
</td>
<td width="29%">
<p>移动vy的内容到vx。两个寄存器都必须在最初的256寄存器范围以内。</p>
</td>
<td width="41%">
<p>0110 - move v0, v1</p>
<p>移动v1寄存器中的内容到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">02</p>
</td>
<td width="16%">
<p>move/from16 vx, vy</p>
</td>
<td width="29%">
<p>移动vy的内容到vx。vy可能在64K寄存器范围以内，而vx则是在最初的256寄存器范围以内。</p>
</td>
<td width="41%">
<p>0200 1900 - move/from16 v0, v25</p>
<p>移动v25寄存器中的内容到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">03</p>
</td>
<td width="16%">
<p>move/16</p>
</td>
<td width="29%">
<p>未知<sup>注</sup><sup>4</sup></p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">04</p>
</td>
<td width="16%">
<p>move-wide</p>
</td>
<td width="29%">
<p>未知<sup>注</sup><sup>4</sup></p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">05</p>
</td>
<td width="16%">
<p>move-wide/from16 vx, vy</p>
</td>
<td width="29%">
<p>移动一个long/double值，从vy到vx。vy可能在64K寄存器范围以内，而vx则是在最初的256寄存器范围以内。</p>
</td>
<td width="41%">
<p>0516 0000 - move-wide/from16 v22, v0</p>
<p>移动v0,v1寄存器中的内容到 v22,v23。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">06</p>
</td>
<td width="16%">
<p>move-wide/16</p>
</td>
<td width="29%">
<p>未知<sup>注</sup><sup>4</sup></p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">07</p>
</td>
<td width="16%">
<p>move-object vx, vy</p>
</td>
<td width="29%">
<p>移动对象引用，从vy到vx。</p>
</td>
<td width="41%">
<p>0781 - move-object v1, v8</p>
<p>移动v8寄存器中的对象引用到v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">08</p>
</td>
<td width="16%">
<p>move-object/from16 vx, vy</p>
</td>
<td width="29%">
<p>移动对象引用，从vy到vx。vy可以处理64K寄存器地址，vx可以处理256寄存器地址。</p>
</td>
<td width="41%">
<p>0801 1500 - move-object/from16 v1, v21</p>
<p>移动v21寄存器中的对象引用到v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">09</p>
</td>
<td width="16%">
<p>move-object/16</p>
</td>
<td width="29%">
<p>未知<sup>注</sup><sup>4</sup></p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">0A</p>
</td>
<td width="16%">
<p>move-result vx</p>
</td>
<td width="29%">
<p>移动上一次方法调用的返回值到vx。</p>
</td>
<td width="41%">
<p>0A00 - move-result v0</p>
<p>移动上一次方法调用的返回值到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">0B</p>
</td>
<td width="16%">
<p>move-result-wide vx</p>
</td>
<td width="29%">
<p>移动上一次方法调用的long/double型返回值到vx,vx+1。</p>
</td>
<td width="41%">
<p>0B02 - move-result-wide v2</p>
<p>移动上一次方法调用的long/double型返回值到v2,v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">0C</p>
</td>
<td width="16%">
<p>move-result-object vx</p>
</td>
<td width="29%">
<p>移动上一次方法调用的对象引用返回值到vx。</p>
</td>
<td width="41%">
<p>0C00 - move-result-object v0</p>
<p>移动上一次方法调用的对象引用返回值到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">0D</p>
</td>
<td width="16%">
<p>move-exception vx</p>
</td>
<td width="29%">
<p>当方法调用抛出异常时移动异常对象引用到vx。</p>
</td>
<td width="41%">
<p>0D19 - move-exception v25</p>
<p>当方法调用抛出异常时移动异常对象引用到v25。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">0E</p>
</td>
<td width="16%">
<p>return-void</p>
</td>
<td width="29%">
<p>返回空值。</p>
</td>
<td width="41%">
<p>0E00 - return-void</p>
<p>返回值为void，即无返回值，并非返回null。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">0F</p>
</td>
<td width="16%">
<p>return vx</p>
</td>
<td width="29%">
<p>返回在vx寄存器的值。</p>
</td>
<td width="41%">
<p>0F00 - return v0</p>
<p>返回v0寄存器中的值。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">10</p>
</td>
<td width="16%">
<p>return-wide vx</p>
</td>
<td width="29%">
<p>返回在vx,vx+1寄存器的double/long值。</p>
</td>
<td width="41%">
<p>1000 - return-wide v0</p>
<p>返回v0,v1寄存器中的double/long值。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">11</p>
</td>
<td width="16%">
<p>return-object vx</p>
</td>
<td width="29%">
<p>返回在vx寄存器的对象引用。</p>
</td>
<td width="41%">
<p>1100 - return-object v0</p>
<p>返回v0寄存器中的对象引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">12</p>
</td>
<td width="16%">
<p>const/4 vx, lit4</p>
</td>
<td width="29%">
<p>存入4位常量到vx。</p>
</td>
<td width="41%">
<p>1221 - const/4 v1, #int 2</p>
<p>存入int型常量2到v1。目的寄存器在第二个字节的低4位，常量2在更高的4位。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">13</p>
</td>
<td width="16%">
<p>const/16 vx, lit16</p>
</td>
<td width="29%">
<p>存入16位常量到vx。</p>
</td>
<td width="41%">
<p>1300 0A00 - const/16 v0, #int 10</p>
<p>存入int型常量10到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">14</p>
</td>
<td width="16%">
<p>const vx, lit32</p>
</td>
<td width="29%">
<p>存入int 型常量到vx。</p>
</td>
<td width="41%">
<p>1400 4E61 BC00 - const v0, #12345678 // #00BC614E</p>
<p>存入常量12345678到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">15</p>
</td>
<td width="16%">
<p>const/high16 v0, lit16</p>
</td>
<td width="29%">
<p>存入16位常量到最高位寄存器，用于初始化float值。</p>
</td>
<td width="41%">
<p>1500 2041 - const/high16 v0, #float 10.0 // #41200000</p>
<p>存入float常量10.0到v0。该指令最高支持16位浮点数。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">16</p>
</td>
<td width="16%">
<p>const-wide/16 vx, lit16</p>
</td>
<td width="29%">
<p>存入int常量到vx,vx+1寄存器，扩展int型常量为long常量。</p>
</td>
<td width="41%">
<p>1600 0A00 - const-wide/16 v0, #long 10</p>
<p>存入long常量10到v0,v1寄存器。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">17</p>
</td>
<td width="16%">
<p>const-wide/32 vx, lit32</p>
</td>
<td width="29%">
<p>存入32位常量到vx,vx+1寄存器，扩展int型常量到long常量。</p>
</td>
<td width="41%">
<p>1702 4e61 bc00 - const-wide/32 v2, #long 12345678 // #00bc614e</p>
<p>存入long常量12345678到v2,v3寄存器。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">18</p>
</td>
<td width="16%">
<p>const-wide vx, lit64</p>
</td>
<td width="29%">
<p>存入64位常量到vx,vx+1寄存器。</p>
</td>
<td width="41%">
<p>1802 874b 6b5d 54dc 2b00- const-wide v2, #long 12345678901234567 // #002bdc545d6b4b87</p>
<p>存入long常量12345678901234567到v2,v3寄存器。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">19</p>
</td>
<td width="16%">
<p>const-wide/high16 vx, lit16</p>
</td>
<td width="29%">
<p>存入16位常量到最高16位的vx,vx+1寄存器，用于初始化double 值。</p>
</td>
<td width="41%">
<p>1900 2440 - const-wide/high16 v0, #double 10.0 // #402400000</p>
<p>存入double常量10.0到v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">1A</p>
</td>
<td width="16%">
<p>const-string vx, <em>字符串</em><em>ID</em></p>
</td>
<td width="29%">
<p>存入字符串常量引用到vx，通过<em>字符串</em><em>ID</em>或<em>字符串</em>。</p>
</td>
<td width="41%">
<p>1A08 0000 - const-string v8, "" // string@0000</p>
<p>存入string@0000（字符串表#0条目）的引用到v8。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">1B</p>
</td>
<td width="16%">
<p>const-string-jumbo</p>
</td>
<td width="29%">
<p>未知<sup>注</sup><sup>4</sup></p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">1C</p>
</td>
<td width="16%">
<p>const-class vx, <em>类型</em><em>ID</em></p>
</td>
<td width="29%">
<p>存入类对象常量到vx，通过<em>类型</em><em>ID</em>或<em>类型</em>（如Object.class）。</p>
</td>
<td width="41%">
<p>1C00 0100 - const-class v0, Test3 // type@0001</p>
<p>存入Test3.class（类型ID表#1条目）的引用到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">1D</p>
</td>
<td width="16%">
<p>monitor-enter vx</p>
</td>
<td width="29%">
<p>获得vx寄存器中的对象引用的监视器。</p>
</td>
<td width="41%">
<p>1D03 - monitor-enter v3</p>
<p>获得v3寄存器中的对象引用的监视器。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">1E</p>
</td>
<td width="16%">
<p>monitor-exit</p>
</td>
<td width="29%">
<p>释放vx寄存器中的对象引用的监视器。</p>
</td>
<td width="41%">
<p>1E03 - monitor-exit v3</p>
<p>释放v3寄存器中的对象引用的监视器。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">1F</p>
</td>
<td width="16%">
<p>check-cast vx, <em>类型</em><em>ID</em></p>
</td>
<td width="29%">
<p>检查vx寄存器中的对象引用是否可以转换成<em>类型</em><em>ID</em>对应类型的实例。如不可转换，抛出ClassCastException 异常，否则继续执行。</p>
</td>
<td width="41%">
<p>1F04 0100 - check-cast v4, Test3 // type@0001</p>
<p>检查v4寄存器中的对象引用是否可以转换成Test3（类型ID表#1条目）的实例。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">20</p>
</td>
<td width="16%">
<p>instance-of vx, vy,<em>类型</em><em>ID</em></p>
</td>
<td width="29%">
<p>检查vy寄存器中的对象引用是否是<em>类型</em><em>ID</em>对应类型的实例，如果是，vx存入非0值，否则vx存入0。</p>
</td>
<td width="41%">
<p>2040 0100 - instance-of v0, v4, Test3 // type@0001</p>
<p>检查v4寄存器中的对象引用是否是Test3（类型ID表#1条目）的实例。如果是，v0存入非0值，否则v0存入0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">21</p>
</td>
<td width="16%">
<p>array-length vx, vy</p>
</td>
<td width="29%">
<p>计算vy寄存器中数组引用的元素长度并将长度存入vx。</p>
</td>
<td width="41%">
<p>2111 - array-length v0, v1</p>
<p>计算v1寄存器中数组引用的元素长度并将长度存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">22</p>
</td>
<td width="16%">
<p>new-instance vx, <em>类型</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>类型</em><em>ID</em>或<em>类型</em>新建一个对象实例，并将新建的对象的引用存入vx。</p>
</td>
<td width="41%">
<p>2200 1500 - new-instance v0, java.io.FileInputStream // type@0015</p>
<p>实例化java.io.FileInputStream（类型ID表#15H条目）类型，并将其对象引用存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">23</p>
</td>
<td width="16%">
<p>new-array vx, vy,<em>类型</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>类型</em><em>ID</em>或<em>类型</em>新建一个数组，vy存入数组的长度，vx存入数组的引用。</p>
</td>
<td width="41%">
<p>2312 2500 - new-array v2, v1, char[] // type@0025</p>
<p>新建一个char（类型ID表#25H条目）数组，v1存入数组的长度，v2存入数组的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">24</p>
</td>
<td width="16%">
<p>filled-new-array {<em>参数</em>}, <em>类型</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>类型</em><em>ID</em>或<em>类型</em>新建一个数组并通过<em>参数</em>填充<sup>注</sup><sup>5</sup>。新的数组引用可以得到一个move-result-object指令，前提是执行过filled-new-array 指令。</p>
</td>
<td width="41%">
<p>2420 530D 0000 - filled-new-array {v0,v0},[I // type@0D53</p>
<p>新建一个int（类型ID表#D53H条目）数组，长度将为2并且2个元素将填充到v0寄存器。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">25</p>
</td>
<td width="16%">
<p>filled-new-array-range {vx..vy}, <em>类型</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>类型</em><em>ID</em>或<em>类型</em>新建一个数组并以寄存器范围为参数填充。新的数组引用可以得到一个move-result-object指令，前提是执行过filled-new-array 指令。</p>
</td>
<td width="41%">
<p>2503 0600 1300 - filled-new-array/range {v19..v21}, [B // type@0006</p>
<p>新建一个byte（类型ID表#6条目）数组，长度将为3并且3个元素将填充到v19,v20,v21寄存器<sup>注</sup><sup>4</sup>。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">26</p>
</td>
<td width="16%">
<p>fill-array-data vx, <em>偏移量</em></p>
</td>
<td width="29%">
<p>用vx的静态数据填充数组引用。静态数据的位址是当前指令位置加<em>偏移量</em>的和。</p>
</td>
<td width="41%">
<p>2606 2500 0000 - fill-array-data v6, 00e6 // +0025</p>
<p>用当前指令位置+25H的静态数据填充v6寄存器的数组引用。偏移量是32位的数字，静态数据的存储格式如下：</p>
<p>0003 // 表类型：静态数组数据</p>
<p>0400 // 每个元素的字节数（这个例子是4字节的int型）</p>
<p>0300 0000 // 元素个数</p>
<p>0100 0000 // 元素 #0：int 1</p>
<p>0200 0000 // 元素 #1：int 2</p>
<p>0300 0000 // 元素 #2：int 3</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">27</p>
</td>
<td width="16%">
<p>throw vx</p>
</td>
<td width="29%">
<p>抛出异常对象，异常对象的引用在vx寄存器。</p>
</td>
<td width="41%">
<p>2700 - throw v0</p>
<p>抛出异常对象，异常对象的引用在v0寄存器。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">28</p>
</td>
<td width="16%">
<p>goto <em>目标</em></p>
</td>
<td width="29%">
<p>通过短偏移量<sup>注</sup><sup>2</sup>无条件跳转到<em>目标</em>。</p>
</td>
<td width="41%">
<p>28F0 - goto 0005 // -0010</p>
<p>跳转到当前位置-16（hex 10）的位置，0005是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">29</p>
</td>
<td width="16%">
<p>goto/16<em>目标</em></p>
</td>
<td width="29%">
<p>通过16位偏移量<sup>注</sup><sup>2</sup>无条件跳转到<em>目标</em>。</p>
</td>
<td width="41%">
<p>2900 0FFE - goto/16 002f // -01f1</p>
<p>跳转到当前位置-1F1H的位置，002f是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">2A</p>
</td>
<td width="16%">
<p>goto/32<em>目标</em></p>
</td>
<td width="29%">
<p>通过32位偏移量<sup>注</sup><sup>2</sup>无条件跳转到<em>目标</em>。</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">2B</p>
</td>
<td width="16%">
<p>packed-switch vx, <em>索引表偏移量</em></p>
</td>
<td width="29%">
<p>实现一个switch 语句，case常量是连续的。这个指令使用<em>索引表</em>，vx是在表中找到具体case的指令偏移量的索引，如果无法在表中找到vx对应的索引将继续执行下一个指令（即default case）。</p>
</td>
<td width="41%">
<p>2B02 0C00 0000 - packed-switch v2, 000c // +000c</p>
<p>根据v2寄存器中的值执行packed switch，索引表的位置是当前指令位置+0CH，表如下所示：</p>
<p>0001 // 表类型：packed switch表</p>
<p>0300 // 元素个数</p>
<p>0000 0000 // 基础元素</p>
<p>0500 0000 0: 00000005 // case 0: +00000005</p>
<p>0700 0000 1: 00000007 // case 1: +00000007</p>
<p>0900 0000 2: 00000009 // case 2: +00000009</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">2C</p>
</td>
<td width="16%">
<p>sparse-switch vx, <em>查询表偏移量</em></p>
</td>
<td width="29%">
<p>实现一个switch 语句，case常量是非连续的。这个指令使用<em>查询表</em>，用于表示case常量和每个case常量的偏移量。如果vx无法在表中匹配将继续执行下一个指令（即default case）。</p>
</td>
<td width="41%">
<p>2C02 0c00 0000 - sparse-switch v2, 000c // +000c</p>
<p>根据v2寄存器中的值执行sparse switch ，查询表的位置是当前指令位置+0CH，表如下所示：</p>
<p>0002 // 表类型：sparse switch表</p>
<p>0300 // 元素个数</p>
<p>9cff ffff // 第一个case常量: -100</p>
<p>fa00 0000 // 第二个case常量: 250</p>
<p>e803 0000 // 第三个case常量: 1000</p>
<p>0500 0000 // 第一个case常量的偏移量: +5</p>
<p>0700 0000 // 第二个case常量的偏移量: +7</p>
<p>0900 0000 // 第三个case常量的偏移量: +9</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">2D</p>
</td>
<td width="16%">
<p>cmpl-float vx, vy, vz</p>
</td>
<td width="29%">
<p>比较vy和vz的float值并在vx存入int型返回值<sup>注</sup><sup>3</sup>。</p>
</td>
<td width="41%">
<p>2D00 0607 - cmpl-float v0, v6, v7</p>
<p>比较v6和v7的float值并在v0存入int型返回值。非数值默认为小于。如果参数为非数值将返回-1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">2E</p>
</td>
<td width="16%">
<p>cmpg-float vx, vy, vz</p>
</td>
<td width="29%">
<p>比较vy和vz的float值并在vx存入int型返回值<sup>注</sup><sup>3</sup>。</p>
</td>
<td width="41%">
<p>2E00 0607 - cmpg-float v0, v6, v7</p>
<p>比较v6和v7的float值并在v0存入int型返回值。非数值默认为大于。如果参数为非数值将返回1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">2F</p>
</td>
<td width="16%">
<p>cmpl-double vx, vy, vz</p>
</td>
<td width="29%">
<p>比较vy和vz<sup>注</sup><sup>2</sup>的double值并在vx存入int型返回值<sup>注</sup><sup>3</sup>。</p>
</td>
<td width="41%">
<p>2F19 0608 - cmpl-double v25, v6, v8</p>
<p>比较v6,v7和v8,v9的double值并在v25存入int型返回值。非数值默认为小于。如果参数为非数值将返回-1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">30</p>
</td>
<td width="16%">
<p>cmpg-double vx, vy, vz</p>
</td>
<td width="29%">
<p>比较vy和vz<sup>注</sup><sup>2</sup>的double值并在vx存入int型返回值<sup>注</sup><sup>3</sup>。</p>
</td>
<td width="41%">
<p>3000 080A - cmpg-double v0, v8, v10</p>
<p>比较v8,v9和v10,v11的double值并在v0存入int型返回值。非数值默认为大于。如果参数为非数值将返回1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">31</p>
</td>
<td width="16%">
<p>cmp-long vx, vy, vz</p>
</td>
<td width="29%">
<p>比较vy和vz的long值并在vx存入int型返回值<sup>注</sup><sup>3</sup>。</p>
</td>
<td width="41%">
<p>3100 0204 - cmp-long v0, v2, v4</p>
<p>比较v2和v4的long值并在v0存入int型返回值。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">32</p>
</td>
<td width="16%">
<p>if-eq vx,vy, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx == vy<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。vx和vy是int型值。</p>
</td>
<td width="41%">
<p>32b3 6600 - if-eq v3, v11, 0080 // +0066</p>
<p>如果v3 == v11，跳转到当前位置+66H。0080是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">33</p>
</td>
<td width="16%">
<p>if-ne vx,vy, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx != vy<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。vx和vy是int型值。</p>
</td>
<td width="41%">
<p>33A3 1000 - if-ne v3, v10, 002c // +0010</p>
<p>如果v3 != v10，跳转到当前位置+10H。002c是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">34</p>
</td>
<td width="16%">
<p>if-lt vx,vy, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &lt; vy<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。vx和vy是int型值。</p>
</td>
<td width="41%">
<p>3432 CBFF - if-lt v2, v3, 0023 // -0035</p>
<p>如果v2 &lt; v3，跳转到当前位置-35H。0023是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">35</p>
</td>
<td width="16%">
<p>if-ge vx, vy, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &gt;= vy<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。vx和vy是int型值。</p>
</td>
<td width="41%">
<p>3510 1B00 - if-ge v0, v1, 002b // +001b</p>
<p>如果v0 &gt;= v1，跳转到当前位置+1BH。002b是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">36</p>
</td>
<td width="16%">
<p>if-gt vx,vy, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &gt; vy<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。vx和vy是int型值。</p>
</td>
<td width="41%">
<p>3610 1B00 - if-ge v0, v1, 002b // +001b</p>
<p>如果v0 &gt; v1，跳转到当前位置+1BH。002b是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">37</p>
</td>
<td width="16%">
<p>if-le vx,vy, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &lt;= vy<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。vx和vy是int型值。</p>
</td>
<td width="41%">
<p>3756 0B00 - if-le v6, v5, 0144 // +000b</p>
<p>如果v6 &lt;= v5，跳转到当前位置+0BH。0144是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">38</p>
</td>
<td width="16%">
<p>if-eqz vx, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx == 0<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。vx是int型值。</p>
</td>
<td width="41%">
<p>3802 1900 - if-eqz v2, 0038 // +0019</p>
<p>如果v2 == 0，跳转到当前位置+19H。0038是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">39</p>
</td>
<td width="16%">
<p>if-nez vx, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx != 0<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。</p>
</td>
<td width="41%">
<p>3902 1200 - if-nez v2, 0014 // +0012</p>
<p>如果v2 != 0，跳转到当前位置+18(hex 12)。0014是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">3A</p>
</td>
<td width="16%">
<p>if-ltz vx, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &lt; 0<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。</p>
</td>
<td width="41%">
<p>3A00 1600 - if-ltz v0, 002d // +0016</p>
<p>如果v0 &lt; 0，跳转到当前位置+16H。002d是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">3B</p>
</td>
<td width="16%">
<p>if-gez vx, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &gt;= 0<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。</p>
</td>
<td width="41%">
<p>3B00 1600 - if-gez v0, 002d // +0016</p>
<p>如果v0 &gt;= 0，跳转到当前位置+16H。002d是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">3C</p>
</td>
<td width="16%">
<p>if-gtz vx, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &gt; 0<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。</p>
</td>
<td width="41%">
<p>3C00 1D00 - if-gtz v0, 004a // +001d</p>
<p>如果v0 &gt; 0，跳转到当前位置+1DH。004a是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">3D</p>
</td>
<td width="16%">
<p>if-lez vx, <em>目标</em></p>
</td>
<td width="29%">
<p>如果vx &lt;= 0<sup>注</sup><sup>2</sup>，跳转到<em>目标</em>。</p>
</td>
<td width="41%">
<p>3D00 1D00 - if-lez v0, 004a // +001d</p>
<p>如果v0 &lt;= 0，跳转到当前位置+1DH。004a是目标指令标签。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">3E</p>
</td>
<td width="16%">
<p>unused_3E</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">3F</p>
</td>
<td width="16%">
<p>unused_3F</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">40</p>
</td>
<td width="16%">
<p>unused_40</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">41</p>
</td>
<td width="16%">
<p>unused_41</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">42</p>
</td>
<td width="16%">
<p>unused_42</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">43</p>
</td>
<td width="16%">
<p>unused_43</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">44</p>
</td>
<td width="16%">
<p>aget vx, vy, vz</p>
</td>
<td width="29%">
<p>从int数组获取一个int型值到vx，对象数组的引用位于vy，需获取的元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4407 0306 - aget v7, v3, v6</p>
<p>从数组获取一个int型值到v7，对象数组的引用位于v3，需获取的元素的索引位于v6。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">45</p>
</td>
<td width="16%">
<p>aget-wide vx, vy, vz</p>
</td>
<td width="29%">
<p>从long/double数组获取一个long/double值到vx,vx+1，数组的引用位于vy，需获取的元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4505 0104 - aget-wide v5, v1, v4</p>
<p>从long/double数组获取一个long/double值到v5,vx6，数组的引用位于v1，需获取的元素的索引位于v4。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">46</p>
</td>
<td width="16%">
<p>aget-object vx, vy, vz</p>
</td>
<td width="29%">
<p>从对象引用数组获取一个对象引用到vx，对象数组的引用位于vy，需获取的元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4602 0200 - aget-object v2, v2, v0</p>
<p>从对象引用数组获取一个对象引用到v2，对象数组的引用位于v2，需获取的元素的索引位于v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">47</p>
</td>
<td width="16%">
<p>aget-boolean vx, vy, vz</p>
</td>
<td width="29%">
<p>从boolean数组获取一个boolean值到vx，数组的引用位于vy，需获取的元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4700 0001 - aget-boolean v0, v0, v1</p>
<p>从boolean数组获取一个boolean值到v0，数组的引用位于v0，需获取的元素的索引位于v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">48</p>
</td>
<td width="16%">
<p>aget-byte vx, vy, vz</p>
</td>
<td width="29%">
<p>从byte数组获取一个byte值到vx，数组的引用位于vy，需获取的元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4800 0001 - aget-byte v0, v0, v1</p>
<p>从byte数组获取一个byte值到v0，数组的引用位于v0，需获取的元素的索引位于v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">49</p>
</td>
<td width="16%">
<p>aget-char vx, vy, vz</p>
</td>
<td width="29%">
<p>从char数组获取一个char值到vx，数组的引用位于vy，需获取的元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4905 0003 - aget-char v5, v0, v3</p>
<p>从char数组获取一个char值到v5，数组的引用位于v0，需获取的元素的索引位于v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">4A</p>
</td>
<td width="16%">
<p>aget-short vx, vy, vz</p>
</td>
<td width="29%">
<p>从short数组获取一个short值到vx，数组的引用位于vy，需获取的元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4A00 0001 - aget-short v0, v0, v1</p>
<p>从short数组获取一个short值到v0，数组的引用位于v0，需获取的元素的索引位于v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">4B</p>
</td>
<td width="16%">
<p>aput vx, vy, vz</p>
</td>
<td width="29%">
<p>将vx的int值作为元素存入int数组，数组的引用位于vy，元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4B00 0305 - aput v0, v3, v5</p>
<p>将v0的int值作为元素存入int数组，数组的引用位于v3，元素的索引位于v5。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">4C</p>
</td>
<td width="16%">
<p>aput-wide vx, vy, vz</p>
</td>
<td width="29%">
<p>将vx,vx+1的double/long值作为元素存入double/long数组，数组的引用位于vy，元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4C05 0104 - aput-wide v5, v1, v4</p>
<p>将v5,v6的double/long值作为元素存入double/long数组，数组的引用位于v1，元素的索引位于v4。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">4D</p>
</td>
<td width="16%">
<p>aput-object vx, vy, vz</p>
</td>
<td width="29%">
<p>将vx的对象引用作为元素存入对象引用数组，数组的引用位于vy，元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4D02 0100 - aput-object v2, v1, v0</p>
<p>将v2的对象引用作为元素存入对象引用数组，数组的引用位于v1，元素的索引位于v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">4E</p>
</td>
<td width="16%">
<p>aput-boolean vx, vy, vz</p>
</td>
<td width="29%">
<p>将vx的boolean值作为元素存入boolean数组，数组的引用位于vy，元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4E01 0002 - aput-boolean v1, v0, v2</p>
<p>将v1的boolean值作为元素存入boolean数组，数组的引用位于v0，元素的索引位于v2。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">4F</p>
</td>
<td width="16%">
<p>aput-byte vx, vy, vz</p>
</td>
<td width="29%">
<p>将vx的byte值作为元素存入byte数组，数组的引用位于vy，元素的索引位于vz。</p>
</td>
<td width="41%">
<p>4F02 0001 - aput-byte v2, v0, v1</p>
<p>将v2的byte值作为元素存入byte数组，数组的引用位于v0，元素的索引位于v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">50</p>
</td>
<td width="16%">
<p>aput-char vx, vy, vz</p>
</td>
<td width="29%">
<p>将vx的char值作为元素存入char数组，数组的引用位于vy，元素的索引位于vz。</p>
</td>
<td width="41%">
<p>5003 0001 - aput-char v3, v0, v1</p>
<p>将v3的char值作为元素存入char数组，数组的引用位于v0，元素的索引位于v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">51</p>
</td>
<td width="16%">
<p>aput-short vx, vy, vz</p>
</td>
<td width="29%">
<p>将vx的short值作为元素存入short数组，数组的引用位于vy，元素的索引位于vz。</p>
</td>
<td width="41%">
<p>5102 0001 - aput-short v2, v0, v1</p>
<p>将v2的short值作为元素存入short数组，数组的引用位于v0，元素的索引位于v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">52</p>
</td>
<td width="16%">
<p>iget vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取实例的int型字段到vx，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5210 0300 - iget v0, v1, Test2.i6:I // field@0003</p>
<p>读取int型字段i6（字段表#3条目）到v0，v1寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">53</p>
</td>
<td width="16%">
<p>iget-wide vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取实例的double/long型字段到vx,vx+1<sup>注</sup><sup>1</sup>，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5320 0400 - iget-wide v0, v2, Test2.l0:J // field@0004</p>
<p>读取long型字段l0（字段表#4条目）到v0,v1，v2寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">54</p>
</td>
<td width="16%">
<p>iget-object vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取一个实例的对象引用字段到vx，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>iget-object v1, v2, LineReader.fis:Ljava/io/FileInputStream; // field@0002</p>
<p>读取FileInputStream对象引用字段fis（字段表#2条目）到v1，v2寄存器中是LineReader实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">55</p>
</td>
<td width="16%">
<p>iget-boolean vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取实例的boolean型字段到vx，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>55FC 0000 - iget-boolean v12, v15, Test2.b0:Z // field@0000</p>
<p>读取boolean型字段b0（字段表#0条目）到v12，v15寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">56</p>
</td>
<td width="16%">
<p>iget-byte vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取实例的byte型字段到vx，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5632 0100 - iget-byte v2, v3, Test3.bi1:B // field@0001</p>
<p>读取byte型字段bi1（字段表#1条目）到v2，v3寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">57</p>
</td>
<td width="16%">
<p>iget-char vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取实例的char型字段到vx，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5720 0300 - iget-char v0, v2, Test3.ci1:C // field@0003</p>
<p>读取char型字段bi1（字段表#3条目）到v0，v2寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">58</p>
</td>
<td width="16%">
<p>iget-short vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取实例的short型字段到vx，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5830 0800 - iget-short v0, v3, Test3.si1:S // field@0008</p>
<p>读取short型字段si1（字段表#8条目）到v0，v3寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">59</p>
</td>
<td width="16%">
<p>iput vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器的值存入实例的int型字段，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5920 0200 - iput v0, v2, Test2.i6:I // field@0002</p>
<p>将v0寄存器的值存入实例的int型字段i6（字段表#2条目），v2寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">5A</p>
</td>
<td width="16%">
<p>iput-wide vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx,vx+1寄存器的值存入实例的double/long型字段，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5A20 0000 - iput-wide v0, v2, Test2.d0:D // field@0000</p>
<p>将v0,v1寄存器的值存入实例的double型字段d0（字段表#0条目），v2寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">5B</p>
</td>
<td width="16%">
<p>iput-object vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器的值存入实例的对象引用字段，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5B20 0000 - iput-object v0, v2, LineReader.bis:Ljava/io/BufferedInputStream; // field@0000</p>
<p>将v0寄存器的值存入实例的对象引用字段bis（字段表#0条目），v2寄存器中是BufferedInputStream实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">5C</p>
</td>
<td width="16%">
<p>iput-boolean vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器的值存入实例的boolean型字段，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5C30 0000 - iput-boolean v0, v3, Test2.b0:Z // field@0000</p>
<p>将v0寄存器的值存入实例的boolean型字段b0（字段表#0条目），v3寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">5D</p>
</td>
<td width="16%">
<p>iput-byte vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器的值存入实例的byte型字段，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5D20 0100 - iput-byte v0, v2, Test3.bi1:B // field@0001</p>
<p>将v0寄存器的值存入实例的byte型字段bi1（字段表#1条目），v2寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">5E</p>
</td>
<td width="16%">
<p>iput-char vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器的值存入实例的char型字段，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5E20 0300 - iput-char v0, v2, Test3.ci1:C // field@0003</p>
<p>将v0寄存器的值存入实例的char型字段ci1（字段表#3条目），v2寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">5F</p>
</td>
<td width="16%">
<p>iput-short vx, vy, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器的值存入实例的short型字段，vy寄存器中是该实例的引用。</p>
</td>
<td width="41%">
<p>5F21 0800 - iput-short v1, v2, Test3.si1:S // field@0008</p>
<p>将v0寄存器的值存入实例的short型字段si1（字段表#8条目），v2寄存器中是Test2实例的引用。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">60</p>
</td>
<td width="16%">
<p>sget vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取静态int型字段到vx。</p>
</td>
<td width="41%">
<p>6000 0700 - sget v0, Test3.is1:I // field@0007</p>
<p>读取Test3的静态int型字段is1（字段表#7条目）到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">61</p>
</td>
<td width="16%">
<p>sget-wide vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取静态double/long型字段到vx,vx+1。</p>
</td>
<td width="41%">
<p>6100 0500 - sget-wide v0, Test2.l1:J // field@0005</p>
<p>读取Test2的静态long型字段l1（字段表#5条目）到v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">62</p>
</td>
<td width="16%">
<p>sget-object vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取静态对象引用字段到vx。</p>
</td>
<td width="41%">
<p>6201 0C00 - sget-object v1, Test3.os1:Ljava/lang/Object; // field@000c</p>
<p>读取Object的静态对象引用字段os1（字段表#CH条目）到v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">63</p>
</td>
<td width="16%">
<p>sget-boolean vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取静态boolean型字段到vx。</p>
</td>
<td width="41%">
<p>6300 0C00 - sget-boolean v0, Test2.sb:Z // field@000c</p>
<p>读取Test2的静态boolean型字段sb（字段表#CH条目）到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">64</p>
</td>
<td width="16%">
<p>sget-byte vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取静态byte型字段到vx。</p>
</td>
<td width="41%">
<p>6400 0200 - sget-byte v0, Test3.bs1:B // field@0002</p>
<p>读取Test3的静态byte型字段bs1（字段表#2条目）到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">65</p>
</td>
<td width="16%">
<p>sget-char vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取静态char型字段到vx。</p>
</td>
<td width="41%">
<p>6500 0700 - sget-char v0, Test3.cs1:C // field@0007</p>
<p>读取Test3的静态char型字段cs1（字段表#7条目）到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">66</p>
</td>
<td width="16%">
<p>sget-short vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>读取静态short型字段到vx。</p>
</td>
<td width="41%">
<p>6600 0B00 - sget-short v0, Test3.ss1:S // field@000b</p>
<p>读取Test3的静态short型字段ss1（字段表#CH条目）到v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">67</p>
</td>
<td width="16%">
<p>sput vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器中的值赋值到int型静态字段。</p>
</td>
<td width="41%">
<p>6700 0100 - sput v0, Test2.i5:I // field@0001</p>
<p>将v0寄存器中的值赋值到Test2的int型静态字段i5（字段表#1条目）。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">68</p>
</td>
<td width="16%">
<p>sput-wide vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx,vx+1寄存器中的值赋值到double/long型静态字段。</p>
</td>
<td width="41%">
<p>6800 0500 - sput-wide v0, Test2.l1:J // field@0005</p>
<p>将v0,v1寄存器中的值赋值到Test2的long型静态字段l1（字段表#5条目）。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">69</p>
</td>
<td width="16%">
<p>sput-object vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器中的对象引用赋值到对象引用静态字段。</p>
</td>
<td width="41%">
<p>6900 0c00 - sput-object v0, Test3.os1:Ljava/lang/Object; // field@000c</p>
<p>将v0寄存器中的对象引用赋值到Test3的对象引用静态字段os1（字段表#CH条目）。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">6A</p>
</td>
<td width="16%">
<p>sput-boolean vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器中的值赋值到boolean型静态字段。</p>
</td>
<td width="41%">
<p>6A00 0300 - sput-boolean v0, Test3.bls1:Z // field@0003</p>
<p>将v0寄存器中的值赋值到Test3的boolean型静态字段bls1（字段表#3条目）。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">6B</p>
</td>
<td width="16%">
<p>sput-byte vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器中的值赋值到byte型静态字段。</p>
</td>
<td width="41%">
<p>6B00 0200 - sput-byte v0, Test3.bs1:B // field@0002</p>
<p>将v0寄存器中的值赋值到Test3的byte型静态字段bs1（字段表#2条目）。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">6C</p>
</td>
<td width="16%">
<p>sput-char vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器中的值赋值到char型静态字段。</p>
</td>
<td width="41%">
<p>6C01 0700 - sput-char v1, Test3.cs1:C // field@0007</p>
<p>将v1寄存器中的值赋值到Test3的char型静态字段cs1（字段表#7条目）。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">6D</p>
</td>
<td width="16%">
<p>sput-short vx, <em>字段</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>字段</em><em>ID</em>将vx寄存器中的值赋值到short型静态字段。</p>
</td>
<td width="41%">
<p>6D00 0B00 - sput-short v0, Test3.ss1:S // field@000b</p>
<p>将v0寄存器中的值赋值到Test3的short型静态字段ss1（字段表#BH条目）。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">6E</p>
</td>
<td width="16%">
<p>invoke-virtual {<em>参数</em>}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用带<em>参数</em>的虚拟方法。</p>
</td>
<td width="41%">
<p>6E53 0600 0421 - invoke-virtual { v4, v0, v1, v2, v3}, Test2.method5:(IIII)V // method@0006</p>
<p>调用Test2的method5（方法表#6条目）方法，该指令共有5个参数（操作码第二个字节的4个最高有效位5）<sup>注</sup><sup>5</sup>。参数v4是"this"实例，v0, v1, v2, v3是method5方法的参数，(IIII)V的4个I分表表示4个int型参数，V表示返回值为void。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">6F</p>
</td>
<td width="16%">
<p>invoke-super {<em>参数</em>}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用带<em>参数</em>的直接父类的虚拟方法。</p>
</td>
<td width="41%">
<p>6F10 A601 0100 invoke-super {v1},java.io.FilterOutputStream.close:()V // method@01a6</p>
<p>调用java.io.FilterOutputStream的close（方法表#1A6条目）方法，参数v1是"this"实例。()V表示close方法没有参数，V表示返回值为void。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">70</p>
</td>
<td width="16%">
<p>invoke-direct {<em>参数</em>}, <em>方法名</em></p>
</td>
<td width="29%">
<p>不解析直接调用带<em>参数</em>的方法。</p>
</td>
<td width="41%">
<p>7010 0800 0100 - invoke-direct {v1}, java.lang.Object.&lt;init&gt;:()V // method@0008</p>
<p>调用java.lang.Object 的&lt;init&gt;（方法表#8条目）方法，参数v1是"this"实例<sup>注</sup><sup>5</sup>。()V表示&lt;init&gt;方法没有参数，V表示返回值为void。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">71</p>
</td>
<td width="16%">
<p>invoke-static {<em>参数</em>}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用带<em>参数</em>的静态方法。</p>
</td>
<td width="41%">
<p>7110 3400 0400 - invoke-static {v4}, java.lang.Integer.parseInt:( Ljava/lang/String;)I // method@0034</p>
<p>调用java.lang.Integer 的parseInt（方法表#34条目）静态方法，该指令只有1个参数v4<sup>注</sup><sup>5</sup>，(Ljava/lang/String;)I中的Ljava/lang/String;表示parseInt方法需要String类型的参数，I表示返回值为int型。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">72</p>
</td>
<td width="16%">
<p>invoke-interface {<em>参数</em>}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用带<em>参数</em>的接口方法。</p>
</td>
<td width="41%">
<p>7240 2102 3154 invoke-interface {v1, v3, v4, v5}, mwfw.IReceivingProtocolAdapter.receivePackage:(ILjava/lang/String;Ljava/io/InputStream;)Z // method@0221</p>
<p>调用mwfw.IReceivingProtocolAdapter 接口的receivePackage方法（方法表#221条目），该指令共有4个参数<sup>注</sup><sup>5</sup>，参数v1是"this"实例，v3,v4,v5是receivePackage方法的参数，(ILjava/lang/String;Ljava/io/InputStream;)Z中的I表示int型参数，Ljava/lang/String;表示String类型参数，Ljava/io/InputStream;表示InputStream类型参数，Z表示返回值为boolean型。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">73</p>
</td>
<td width="16%">
<p>unused_73</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">74</p>
</td>
<td width="16%">
<p>invoke-virtual/range {vx..vy}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用以寄存器范围为参数的虚拟方法。该指令第一个寄存器和寄存器的数量将传递给方法。</p>
</td>
<td width="41%">
<p>7403 0600 1300 - invoke-virtual {v19..v21}, Test2.method5:(IIII)V // method@0006</p>
<p>调用Test2的method5（方法表#6条目）方法，该指令共有3个参数。参数v19是"this"实例，v20,v21是method5方法的参数，(IIII)V的4个I分表表示4个int型参数，V表示返回值为void。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">75</p>
</td>
<td width="16%">
<p>invoke-super/range {vx..vy}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用以寄存器范围为参数的直接父类的虚拟方法。该指令第一个寄存器和寄存器的数量将会传递给方法。</p>
</td>
<td width="41%">
<p>7501 A601 0100 invoke-super {v1},java.io.FilterOutputStream.close:()V // method@01a6</p>
<p>调用java.io.FilterOutputStream的close（方法表#1A6条目）方法，参数v1是"this"实例。()V表示close方法没有参数，V表示返回值为void。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">76</p>
</td>
<td width="16%">
<p>invoke-direct/range {vx..vy}, <em>方法名</em></p>
</td>
<td width="29%">
<p>不解析直接调用以寄存器范围为参数的方法。该指令第一个寄存器和寄存器的数量将会传递给方法。</p>
</td>
<td width="41%">
<p>7603 3A00 1300 - invoke-direct/range {v19..21},java.lang.Object.&lt;init&gt;:()V // method@003a</p>
<p>调用java.lang.Object 的&lt;init&gt;（方法表#3A条目）方法，参数v19是"this"实例（操作码第五、第六字节表示范围从v19开始，第二个字节为03表示传入了3个参数），()V表示&lt;init&gt;方法没有参数，V表示返回值为void。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">77</p>
</td>
<td width="16%">
<p>invoke-static/range {vx..vy}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用以寄存器范围为参数的静态方法。该指令第一个寄存器和寄存器的数量将会传递给方法。</p>
</td>
<td width="41%">
<p>7703 3A00 1300 - invoke-static/range {v19..21},java.lang.Integer.parseInt:(Ljava/lang/String;)I // method@0034</p>
<p>调用java.lang.Integer 的parseInt（方法表#34条目）静态方法，参数v19是"this"实例（操作码第五、第六字节表示范围从v19开始，第二个字节为03表示传入了3个参数），(Ljava/lang/String;)I中的Ljava/lang/String;表示parseInt方法需要String类型的参数，I表示返回值为int型。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">78</p>
</td>
<td width="16%">
<p>invoke-interface-range {vx..vy}, <em>方法名</em></p>
</td>
<td width="29%">
<p>调用以寄存器范围为参数的接口方法。该指令第一个寄存器和寄存器的数量将会传递给方法。</p>
</td>
<td width="41%">
<p>7840 2102 0100 invoke-interface {v1..v4}, mwfw.IReceivingProtocolAdapter.receivePackage:(ILjava/lang/String;Ljava/io/InputStream;)Z // method@0221</p>
<p>调用mwfw.IReceivingProtocolAdapter 接口的receivePackage方法（方法表#221条目），该指令共有4个参数<sup>注</sup><sup>5</sup>，参数v1是"this"实例，v2,v3,v4是receivePackage方法的参数，(ILjava/lang/String;Ljava/io/InputStream;)Z中的I表示int型参数，Ljava/lang/String;表示String类型参数，Ljava/io/InputStream;表示InputStream类型参数，Z表示返回值为boolean型。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">79</p>
</td>
<td width="16%">
<p>unused_79</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">7A</p>
</td>
<td width="16%">
<p>unused_7A</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">7B</p>
</td>
<td width="16%">
<p>neg-int vx, vy</p>
</td>
<td width="29%">
<p>计算vx = -vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>7B01 - neg-int v1,v0</p>
<p>计算-v0并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">7C</p>
</td>
<td width="16%">
<p>not-int vx, vy</p>
</td>
<td width="29%">
<p>未知<sup>注</sup><sup>4</sup></p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">7D</p>
</td>
<td width="16%">
<p>neg-long vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 = -(vy,vy+1) 并将结果存入vx,vx+1。</p>
</td>
<td width="41%">
<p>7D02 - neg-long v2,v0</p>
<p>计算-(v0,v1) 并将结果存入(v2,v3)。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">7E</p>
</td>
<td width="16%">
<p>not-long vx, vy</p>
</td>
<td width="29%">
<p>未知<sup>注</sup><sup>4</sup></p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">7F</p>
</td>
<td width="16%">
<p>neg-float vx, vy</p>
</td>
<td width="29%">
<p>计算vx = -vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>7F01 - neg-float v1,v0</p>
<p>计算-v0并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">80</p>
</td>
<td width="16%">
<p>neg-double vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1=-(vy,vy+1) 并将结果存入vx,vx+1。</p>
</td>
<td width="41%">
<p>8002 - neg-double v2,v0</p>
<p>计算-(v0,v1) 并将结果存入(v2,v3)。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">81</p>
</td>
<td width="16%">
<p>int-to-long vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的int型值为long型值存入vx,vx+1。</p>
</td>
<td width="41%">
<p>8106 - int-to-long v6, v0</p>
<p>转换v0寄存器中的int型值为long型值存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">82</p>
</td>
<td width="16%">
<p>int-to-float vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的int型值为float型值存入vx。</p>
</td>
<td width="41%">
<p>8206 - int-to-float v6, v0</p>
<p>转换v0寄存器中的int型值为float型值存入v6。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">83</p>
</td>
<td width="16%">
<p>int-to-double vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的int型值为double型值存入vx,vx+1。</p>
</td>
<td width="41%">
<p>8306 - int-to-double v6, v0</p>
<p>转换v0寄存器中的int型值为double型值存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">84</p>
</td>
<td width="16%">
<p>long-to-int vx, vy</p>
</td>
<td width="29%">
<p>转换vy,vy+1寄存器中的long型值为int型值存入vx。</p>
</td>
<td width="41%">
<p>8424 - long-to-int v4, v2</p>
<p>转换v2,v3寄存器中的long型值为int型值存入v4。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">85</p>
</td>
<td width="16%">
<p>long-to-float vx, vy</p>
</td>
<td width="29%">
<p>转换vy,vy+1寄存器中的long型值为float型值存入vx。</p>
</td>
<td width="41%">
<p>8510 - long-to-float v0, v1</p>
<p>转换v1,v2寄存器中的long型值为float型值存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">86</p>
</td>
<td width="16%">
<p>long-to-double vx, vy</p>
</td>
<td width="29%">
<p>转换vy,vy+1寄存器中的long型值为double型值存入vx,vx+1。</p>
</td>
<td width="41%">
<p>8610 - long-to-double v0, v1</p>
<p>转换v1,vy2寄存器中的long型值为double型值存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">87</p>
</td>
<td width="16%">
<p>float-to-int vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的float型值为int型值存入vx。</p>
</td>
<td width="41%">
<p>8730 - float-to-int v0, v3</p>
<p>转换v3寄存器中的float型值为int型值存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">88</p>
</td>
<td width="16%">
<p>float-to-long vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的float型值为long型值存入vx,vx+1。</p>
</td>
<td width="41%">
<p>8830 - float-to-long v0, v3</p>
<p>转换v3寄存器中的float型值为long型值存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">89</p>
</td>
<td width="16%">
<p>float-to-double vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的float型值为double型值存入vx,vx+1。</p>
</td>
<td width="41%">
<p>8930 - float-to-double v0, v3</p>
<p>转换v3寄存器中的float型值为double型值存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">8A</p>
</td>
<td width="16%">
<p>double-to-int vx, vy</p>
</td>
<td width="29%">
<p>转换vy,vy+1寄存器中的double型值为int型值存入vx。</p>
</td>
<td width="41%">
<p>8A40 - double-to-int v0, v4</p>
<p>转换v4,v5寄存器中的double型值为int型值存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">8B</p>
</td>
<td width="16%">
<p>double-to-long vx, vy</p>
</td>
<td width="29%">
<p>转换vy,vy+1寄存器中的double型值为long型值存入vx,vx+1。</p>
</td>
<td width="41%">
<p>8B40 - double-to-long v0, v4</p>
<p>转换v4,v5寄存器中的double型值为long型值存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">8C</p>
</td>
<td width="16%">
<p>double-to-float vx, vy</p>
</td>
<td width="29%">
<p>转换vy,vy+1寄存器中的double型值为float型值存入vx。</p>
</td>
<td width="41%">
<p>8C40 - double-to-float v0, v4</p>
<p>转换v4,v5寄存器中的double型值为float型值存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">8D</p>
</td>
<td width="16%">
<p>int-to-byte vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的int型值为byte型值存入vx。</p>
</td>
<td width="41%">
<p>8D00 - int-to-byte v0, v0</p>
<p>转换v0寄存器中的int型值为byte型值存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">8E</p>
</td>
<td width="16%">
<p>int-to-char vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的int型值为char型值存入vx。</p>
</td>
<td width="41%">
<p>8E33 - int-to-char v3, v3</p>
<p>转换v3寄存器中的int型值为char型值存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">8F</p>
</td>
<td width="16%">
<p>int-to-short vx, vy</p>
</td>
<td width="29%">
<p>转换vy寄存器中的int型值为short型值存入vx。</p>
</td>
<td width="41%">
<p>8F00 - int-to-short v3, v0</p>
<p>转换v0寄存器中的int型值为short型值存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">90</p>
</td>
<td width="16%">
<p>add-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy + vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9000 0203 - add-int v0, v2, v3</p>
<p>计算v2 + v3并将结果存入v0<sup>注</sup><sup>4</sup>。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">91</p>
</td>
<td width="16%">
<p>sub-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy - vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9100 0203 - sub-int v0, v2, v3</p>
<p>计算v2 – v3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">92</p>
</td>
<td width="16%">
<p>mul-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy * vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9200 0203 - mul-int v0,v2,v3</p>
<p>计算v2 * w3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">93</p>
</td>
<td width="16%">
<p>div-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy / vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9303 0001 - div-int v3, v0, v1</p>
<p>计算v0 / v1并将结果存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">94</p>
</td>
<td width="16%">
<p>rem-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy % vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9400 0203 - rem-int v0, v2, v3</p>
<p>计算v3 % v2并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">95</p>
</td>
<td width="16%">
<p>and-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy 与 vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9503 0001 - and-int v3, v0, v1</p>
<p>计算v0 与 v1并将结果存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">96</p>
</td>
<td width="16%">
<p>or-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy 或 vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9603 0001 - or-int v3, v0, v1</p>
<p>计算v0 或 v1并将结果存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">97</p>
</td>
<td width="16%">
<p>xor-int vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy 异或 vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>9703 0001 - xor-int v3, v0, v1</p>
<p>计算v0 异或 v1并将结果存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">98</p>
</td>
<td width="16%">
<p>shl-int vx, vy, vz</p>
</td>
<td width="29%">
<p>左移vy，vz指定移动的位置，结果存入vx。</p>
</td>
<td width="41%">
<p>9802 0001 - shl-int v2, v0, v1</p>
<p>以v1指定的位置左移v0，结果存入v2。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">99</p>
</td>
<td width="16%">
<p>shr-int vx, vy, vz</p>
</td>
<td width="29%">
<p>右移vy，vz指定移动的位置，结果存入vx。</p>
</td>
<td width="41%">
<p>9902 0001 - shr-int v2, v0, v1</p>
<p>以v1指定的位置右移v0，结果存入v2。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">9A</p>
</td>
<td width="16%">
<p>ushr-int vx, vy, vz</p>
</td>
<td width="29%">
<p>无符号右移vy，vz指定移动的位置，结果存入vx。</p>
</td>
<td width="41%">
<p>9A02 0001 - ushr-int v2, v0, v1</p>
<p>以v1指定的位置无符号右移v0，结果存入v2。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">9B</p>
</td>
<td width="16%">
<p>add-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 + vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>9B00 0305 - add-long v0, v3, v5</p>
<p>计算v3,v4 + v5,v6并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">9C</p>
</td>
<td width="16%">
<p>sub-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 - vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>9C00 0305 - sub-long v0, v3, v5</p>
<p>计算v3,v4 - v5,v6并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">9D</p>
</td>
<td width="16%">
<p>mul-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 * vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>9D00 0305 - mul-long v0, v3, v5</p>
<p>计算v3,v4 * v5,v6并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">9E</p>
</td>
<td width="16%">
<p>div-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 / vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>9E06 0002 - div-long v6, v0, v2</p>
<p>计算v0,v1 / v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">9F</p>
</td>
<td width="16%">
<p>rem-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 % vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>9F06 0002 - rem-long v6, v0, v2</p>
<p>计算v0,v1 % v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A0</p>
</td>
<td width="16%">
<p>and-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 与 vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>A006 0002 - and-long v6, v0, v2</p>
<p>计算v0,v1 与 v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A1</p>
</td>
<td width="16%">
<p>or-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 或 vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>A106 0002 - or-long v6, v0, v2</p>
<p>计算v0,v1 或 v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A2</p>
</td>
<td width="16%">
<p>xor-long vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 异或 vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>A206 0002 - xor-long v6, v0, v2</p>
<p>计算v0,v1 异或 v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A3</p>
</td>
<td width="16%">
<p>shl-long vx, vy, vz</p>
</td>
<td width="29%">
<p>左移vy,vy+1，vz指定移动的位置，结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>A302 0004 - shl-long v2, v0, v4</p>
<p>以v4指定的位置左移v0,v1，结果存入v2,v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A4</p>
</td>
<td width="16%">
<p>shr-long vx, vy, vz</p>
</td>
<td width="29%">
<p>右移vy,vy+1，vz指定移动的位置，结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>A402 0004 - shr-long v2, v0, v4</p>
<p>以v4指定的位置右移v0,v1，结果存入v2,v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A5</p>
</td>
<td width="16%">
<p>ushr-long vx, vy, vz</p>
</td>
<td width="29%">
<p>无符号右移vy,vy+1，vz指定移动的位置，结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>A502 0004 - ushr-long v2, v0, v4</p>
<p>以v4指定的位置无符号右移v0,v1，结果存入v2,v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A6</p>
</td>
<td width="16%">
<p>add-float vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy + vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>A600 0203 - add-float v0, v2, v3</p>
<p>计算v2 + v3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A7</p>
</td>
<td width="16%">
<p>sub-float vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy - vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>A700 0203 - sub-float v0, v2, v3</p>
<p>计算v2 - v3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A8</p>
</td>
<td width="16%">
<p>mul-float vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy * vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>A803 0001 - mul-float v3, v0, v1</p>
<p>计算v0 * v1并将结果存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">A9</p>
</td>
<td width="16%">
<p>div-float vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy / vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>A903 0001 - div-float v3, v0, v1</p>
<p>计算v0 / v1并将结果存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">AA</p>
</td>
<td width="16%">
<p>rem-float vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy % vz并将结果存入vx。</p>
</td>
<td width="41%">
<p>AA03 0001 - rem-float v3, v0, v1</p>
<p>计算v0 % v1并将结果存入v3。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">AB</p>
</td>
<td width="16%">
<p>add-double vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 + vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>AB00 0305 - add-double v0, v3, v5</p>
<p>计算v3,v4 + v5,v6并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">AC</p>
</td>
<td width="16%">
<p>sub-double vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 - vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>AC00 0305 - sub-double v0, v3, v5</p>
<p>计算v3,v4 - v5,v6并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">AD</p>
</td>
<td width="16%">
<p>mul-double vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 * vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>AD06 0002 - mul-double v6, v0, v2</p>
<p>计算v0,v1 * v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">AE</p>
</td>
<td width="16%">
<p>div-double vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 / vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>AE06 0002 - div-double v6, v0, v2</p>
<p>计算v0,v1 / v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">AF</p>
</td>
<td width="16%">
<p>rem-double vx, vy, vz</p>
</td>
<td width="29%">
<p>计算vy,vy+1 % vz,vz+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>AF06 0002 - rem-double v6, v0, v2</p>
<p>计算v0,v1 % v2,v3并将结果存入v6,v7。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B0</p>
</td>
<td width="16%">
<p>add-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx + vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B010 - add-int/2addr v0,v1</p>
<p>计算v0 + v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B1</p>
</td>
<td width="16%">
<p>sub-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx - vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B140 - sub-int/2addr v0, v4</p>
<p>计算v0 – v4并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B2</p>
</td>
<td width="16%">
<p>mul-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx * vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B210 - mul-int/2addr v0, v1</p>
<p>计算v0 * v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B3</p>
</td>
<td width="16%">
<p>div-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx / vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B310 - div-int/2addr v0, v1</p>
<p>计算v0 / v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B4</p>
</td>
<td width="16%">
<p>rem-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx % vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B410 - rem-int/2addr v0, v1</p>
<p>计算v0 % v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B5</p>
</td>
<td width="16%">
<p>and-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx 与 vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B510 - and-int/2addr v0, v1</p>
<p>计算v0 与 v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B6</p>
</td>
<td width="16%">
<p>or-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx 或 vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B610 - or-int/2addr v0, v1</p>
<p>计算v0 或 v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B7</p>
</td>
<td width="16%">
<p>xor-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx 异或 vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>B710 - xor-int/2addr v0, v1</p>
<p>计算v0 异或 v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B8</p>
</td>
<td width="16%">
<p>shl-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>左移vx，vy指定移动的位置，并将结果存入vx。</p>
</td>
<td width="41%">
<p>B810 - shl-int/2addr v0, v1</p>
<p>以v1指定的位置左移v0，结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">B9</p>
</td>
<td width="16%">
<p>shr-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>右移vx，vy指定移动的位置，并将结果存入vx。</p>
</td>
<td width="41%">
<p>B910 - shr-int/2addr v0, v1</p>
<p>以v1指定的位置右移v0，结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">BA</p>
</td>
<td width="16%">
<p>ushr-int/2addr vx, vy</p>
</td>
<td width="29%">
<p>无符号右移vx，vy指定移动的位置，并将结果存入vx。</p>
</td>
<td width="41%">
<p>BA10 - ushr-int/2addr v0, v1</p>
<p>以v1指定的位置无符号右移v0，结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">BB</p>
</td>
<td width="16%">
<p>add-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 + vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>BB20 - add-long/2addr v0, v2</p>
<p>计算v0,v1 + v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">BC</p>
</td>
<td width="16%">
<p>sub-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 - vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>BC70 - sub-long/2addr v0, v7</p>
<p>计算v0,v1 - v7,v8并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">BD</p>
</td>
<td width="16%">
<p>mul-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 * vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>BD70 - mul-long/2addr v0, v7</p>
<p>计算v0,v1 * v7,v8并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">BE</p>
</td>
<td width="16%">
<p>div-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 / vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>BE20 - div-long/2addr v0, v2</p>
<p>计算v0,v1 / v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">BF</p>
</td>
<td width="16%">
<p>rem-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 % vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>BF20 - rem-long/2addr v0, v2</p>
<p>计算v0,v1 % v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C0</p>
</td>
<td width="16%">
<p>and-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 与 vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>C020 - and-long/2addr v0, v2</p>
<p>计算v0,v1 与 v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C1</p>
</td>
<td width="16%">
<p>or-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 或 vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>C120 - or-long/2addr v0, v2</p>
<p>计算v0,v1 或 v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C2</p>
</td>
<td width="16%">
<p>xor-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 异或 vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>C220 - xor-long/2addr v0, v2</p>
<p>计算v0,v1 异或 v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C3</p>
</td>
<td width="16%">
<p>shl-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>左移vx,vx+1，vy指定移动的位置，并将结果存入vx,vx+1。</p>
</td>
<td width="41%">
<p>C320 - shl-long/2addr v0, v2</p>
<p>以v2指定的位置左移v0,v1，结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C4</p>
</td>
<td width="16%">
<p>shr-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>右移vx,vx+1，vy指定移动的位置，并将结果存入vx,vx+1。</p>
</td>
<td width="41%">
<p>C420 - shr-long/2addr v0, v2</p>
<p>以v2指定的位置右移v0,v1，结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C5</p>
</td>
<td width="16%">
<p>ushr-long/2addr vx, vy</p>
</td>
<td width="29%">
<p>无符号右移vx,vx+1，vy指定移动的位置，并将结果存入vx,vx+1。</p>
</td>
<td width="41%">
<p>C520 - ushr-long/2addr v0, v2</p>
<p>以v2指定的位置无符号右移v0,v1，结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C6</p>
</td>
<td width="16%">
<p>add-float/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx + vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>C640 - add-float/2addr v0,v4</p>
<p>计算v0 + v4并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C7</p>
</td>
<td width="16%">
<p>sub-float/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx - vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>C740 - sub-float/2addr v0,v4</p>
<p>计算v0 - v4并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C8</p>
</td>
<td width="16%">
<p>mul-float/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx * vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>C810 - mul-float/2addr v0, v1</p>
<p>计算v0 * v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">C9</p>
</td>
<td width="16%">
<p>div-float/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx / vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>C910 - div-float/2addr v0, v1</p>
<p>计算v0 / v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">CA</p>
</td>
<td width="16%">
<p>rem-float/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx % vy并将结果存入vx。</p>
</td>
<td width="41%">
<p>CA10 - rem-float/2addr v0, v1</p>
<p>计算v0 % v1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">CB</p>
</td>
<td width="16%">
<p>add-double/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 + vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>CB70 - add-double/2addr v0, v7</p>
<p>计算v0,v1 + v7,v8并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">CC</p>
</td>
<td width="16%">
<p>sub-double/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 - vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>CC70 - sub-double/2addr v0, v7</p>
<p>计算v0,v1 - v7,v8并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">CD</p>
</td>
<td width="16%">
<p>mul-double/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 * vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>CD20 - mul-double/2addr v0, v2</p>
<p>计算v0,v1 * v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">CE</p>
</td>
<td width="16%">
<p>div-double/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 / vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>CE20 - div-double/2addr v0, v2</p>
<p>计算v0,v1 / v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">CF</p>
</td>
<td width="16%">
<p>rem-double/2addr vx, vy</p>
</td>
<td width="29%">
<p>计算vx,vx+1 % vy,vy+1并将结果存入vx,vx+1<sup>注</sup><sup>1</sup>。</p>
</td>
<td width="41%">
<p>CF20 - rem-double/2addr v0, v2</p>
<p>计算v0,v1 % v2,v3并将结果存入v0,v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D0</p>
</td>
<td width="16%">
<p>add-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy + lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D001 D204 - add-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 + 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D1</p>
</td>
<td width="16%">
<p>sub-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy - lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D101 D204 - sub-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 - 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D2</p>
</td>
<td width="16%">
<p>mul-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy * lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D201 D204 - mul-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 * 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D3</p>
</td>
<td width="16%">
<p>div-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy / lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D301 D204 - div-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 / 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D4</p>
</td>
<td width="16%">
<p>rem-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy % lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D401 D204 - rem-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 % 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D5</p>
</td>
<td width="16%">
<p>and-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy 与 lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D501 D204 - and-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 与 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D6</p>
</td>
<td width="16%">
<p>or-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy 或 lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D601 D204 - or-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 或 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D7</p>
</td>
<td width="16%">
<p>xor-int/lit16 vx, vy, lit16</p>
</td>
<td width="29%">
<p>计算vy 异或 lit16并将结果存入vx。</p>
</td>
<td width="41%">
<p>D701 D204 - xor-int/lit16 v1, v0, #int 1234 // #04d2</p>
<p>计算v0 异或 1234并将结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D8</p>
</td>
<td width="16%">
<p>add-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy + lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>D800 0201 - add-int/lit8 v0,v2, #int1</p>
<p>计算v2 + 1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">D9</p>
</td>
<td width="16%">
<p>sub-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy - lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>D900 0201 - sub-int/lit8 v0,v2, #int1</p>
<p>计算v2 - 1并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">DA</p>
</td>
<td width="16%">
<p>mul-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy * lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>DA00 0002 - mul-int/lit8 v0,v0, #int2</p>
<p>计算v0 * 2并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">DB</p>
</td>
<td width="16%">
<p>div-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy / lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>DB00 0203 - mul-int/lit8 v0,v2, #int3</p>
<p>计算v2 / 3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">DC</p>
</td>
<td width="16%">
<p>rem-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy % lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>DC00 0203 - rem-int/lit8 v0,v2, #int3</p>
<p>计算v2 % 3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">DD</p>
</td>
<td width="16%">
<p>and-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy 与 lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>DD00 0203 - and-int/lit8 v0,v2, #int3</p>
<p>计算v2 与 3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">DE</p>
</td>
<td width="16%">
<p>or-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy 或 lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>DE00 0203 - or-int/lit8 v0, v2, #int 3</p>
<p>计算v2 或 3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">DF</p>
</td>
<td width="16%">
<p>xor-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>计算vy异或lit8并将结果存入vx。</p>
</td>
<td width="41%">
<p>DF00 0203 | 0008: xor-int/lit8 v0, v2, #int 3</p>
<p>计算v2 异或 3并将结果存入v0。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">E0</p>
</td>
<td width="16%">
<p>shl-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>左移vy，lit8指定移动的位置，并将结果存入vx。</p>
</td>
<td width="41%">
<p>E001 0001 - shl-int/lit8 v1, v0, #int 1</p>
<p>将v0左移1位，结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">E1</p>
</td>
<td width="16%">
<p>shr-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>右移vy，lit8指定移动的位置，并将结果存入vx。</p>
</td>
<td width="41%">
<p>E101 0001 - shr-int/lit8 v1, v0, #int 1</p>
<p>将v0右移1位，结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">E2</p>
</td>
<td width="16%">
<p>ushr-int/lit8 vx, vy, lit8</p>
</td>
<td width="29%">
<p>无符号右移vy，lit8指定移动的位置，并将结果存入vx。</p>
</td>
<td width="41%">
<p>E201 0001 - ushr-int/lit8 v1, v0, #int 1</p>
<p>将v0无符号右移1位，结果存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">E3</p>
</td>
<td width="16%">
<p>unused_E3</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">E4</p>
</td>
<td width="16%">
<p>unused_E4</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">E5</p>
</td>
<td width="16%">
<p>unused_E5</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">E6</p>
</td>
<td width="16%">
<p>unused_E6</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">E7</p>
</td>
<td width="16%">
<p>unused_E7</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">E8</p>
</td>
<td width="16%">
<p>unused_E8</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">E9</p>
</td>
<td width="16%">
<p>unused_E9</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">EA</p>
</td>
<td width="16%">
<p>unused_EA</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">EB</p>
</td>
<td width="16%">
<p>unused_EB</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">EC</p>
</td>
<td width="16%">
<p>unused_EC</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">ED</p>
</td>
<td width="16%">
<p>unused_ED</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">EE</p>
</td>
<td width="16%">
<p>execute-inline {<em>参数</em>}, <em>内联</em><em>ID</em></p>
</td>
<td width="29%">
<p>根据<em>内联</em><em>ID</em><sup>注</sup><sup>6</sup>执行内联方法。</p>
</td>
<td width="41%">
<p>EE20 0300 0100 - execute-inline {v1, v0}, inline #0003</p>
<p>执行内联方法#3，参数v1,v0，其中参数v1为"this"的实例，v0是方法的参数。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">EF</p>
</td>
<td width="16%">
<p>unused_EF</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">F0</p>
</td>
<td width="16%">
<p>invoke-direct-empty</p>
</td>
<td width="29%">
<p>用于空方法的占位符，如Object.&lt;init&gt;。这相当于正常执行了nop指令<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F010 F608 0000 - invoke-direct-empty {v0}, Ljava/lang/Object;.&lt;init&gt;:()V // method@08f6</p>
<p>替代空方法java/lang/Object;&lt;init&gt;。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F1</p>
</td>
<td width="16%">
<p>unused_F1</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">F2</p>
</td>
<td width="16%">
<p>iget-quick vx, vy, <em>偏移量</em></p>
</td>
<td width="29%">
<p>获取vy寄存器中实例指向+<em>偏移位置</em>的数据区的值，存入vx<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F221 1000 - iget-quick v1, v2, [obj+0010]</p>
<p>获取v2寄存器中的实例指向+10H位置的数据区的值，存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F3</p>
</td>
<td width="16%">
<p>iget-wide-quick vx, vy, <em>偏移量</em></p>
</td>
<td width="29%">
<p>获取vy寄存器中实例指向+<em>偏移位置</em>的数据区的值，存入vx,vx+1<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F364 3001 - iget-wide-quick v4, v6, [obj+0130]</p>
<p>获取v6寄存器中的实例指向+130H位置的数据区的值，存入v4,v5。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F4</p>
</td>
<td width="16%">
<p>iget-object-quick vx, vy, <em>偏移量</em></p>
</td>
<td width="29%">
<p>获取vy寄存器中实例指向+<em>偏移位置</em>的数据区的对象引用，存入vx<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F431 0C00 - iget-object-quick v1, v3, [obj+000c]</p>
<p>获取v3寄存器中的实例指向+0CH位置的数据区的对象引用，存入v1。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F5</p>
</td>
<td width="16%">
<p>iput-quick vx, vy, <em>偏移量</em></p>
</td>
<td width="29%">
<p>将vx寄存器中的值存入vy寄存器中的实例指向+<em>偏移位置</em>的数据区<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F521 1000 - iput-quick v1, v2, [obj+0010]</p>
<p>将v1寄存器中的值存入v2寄存器中的实例指向+10H位置的数据区。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F6</p>
</td>
<td width="16%">
<p>iput-wide-quick vx, vy, <em>偏移量</em></p>
</td>
<td width="29%">
<p>将vx,vx+1寄存器中的值存入vy寄存器中的实例指向+<em>偏移位置</em>的数据区<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F652 7001 - iput-wide-quick v2, v5, [obj+0170]</p>
<p>将v2,v3寄存器中的值存入v5寄存器中的实例指向+170H位置的数据区。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F7</p>
</td>
<td width="16%">
<p>iput-object-quick vx, vy, <em>偏移量</em></p>
</td>
<td width="29%">
<p>将vx寄存器中的对象引用存入vy寄存器中的实例指向+<em>偏移位置</em>的数据区<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F701 4C00 - iput-object-quick v1, v0, [obj+004c]</p>
<p>将v1寄存器中的对象引用存入v0寄存器中的实例指向+4CH位置的数据区。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F8</p>
</td>
<td width="16%">
<p>invoke-virtual-quick {<em>参数</em>}, <em>虚拟表偏移量</em></p>
</td>
<td width="29%">
<p>调用虚拟方法，使用目标对象虚拟表<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F820 B800 CF00 - invoke-virtual-quick {v15, v12}, vtable #00b8</p>
<p>调用虚拟方法，目标对象的实例指向位于v15寄存器，方法位于虚拟表#B8条目，方法所需的参数位于v12。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">F9</p>
</td>
<td width="16%">
<p>invoke-virtual-quick/range {<em>参数范围</em>}, <em>虚拟表偏移量</em></p>
</td>
<td width="29%">
<p>调用虚拟方法，使用目标对象虚拟表<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F906 1800 0000 - invoke-virtual-quick/range {v0..v5},vtable #0018</p>
<p>调用虚拟方法，目标对象的实例指向位于v0寄存器，方法位于虚拟表#18H条目，方法所需的参数位于v1..v5。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">FA</p>
</td>
<td width="16%">
<p>invoke-super-quick {<em>参数</em>}, <em>虚拟表偏移量</em></p>
</td>
<td width="29%">
<p>调用父类虚拟方法，使用目标对象的直接父类的虚拟表<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>FA40 8100 3254 - invoke-super-quick {v2, v3, v4, v5}, vtable #0081</p>
<p>调用父类虚拟方法，目标对象的实例指向位于v2寄存器，方法位于虚拟表#81H条目，方法所需的参数位于v3,v4,v5。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">FB</p>
</td>
<td width="16%">
<p>invoke-super-quick/range {<em>参数范围</em>}, <em>虚拟表偏移量</em></p>
</td>
<td width="29%">
<p>调用父类虚拟方法，使用目标对象的直接父类的虚拟表<sup>注</sup><sup>6</sup>。</p>
</td>
<td width="41%">
<p>F906 1B00 0000 - invoke-super-quick/range {v0..v5}, vtable #001b</p>
<p>调用父类虚拟方法，目标对象的实例指向位于v0寄存器，方法位于虚拟表#1B条目，方法所需的参数位于v1..v5。</p>
</td>
</tr>
<tr>
<td width="12%">
<p align="center">FC</p>
</td>
<td width="16%">
<p>unused_FC</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">FD</p>
</td>
<td width="16%">
<p>unused_FD</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">FE</p>
</td>
<td width="16%">
<p>unused_FE</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
<tr>
<td width="12%">
<p align="center">FF</p>
</td>
<td width="16%">
<p>unused_FF</p>
</td>
<td width="29%">
<p>未使用</p>
</td>
<td width="41%">&nbsp;</td>
</tr>
</tbody>
</table>


* Double和long值占用两个寄存器。（例：在vy地址上的值位于vy,vy+1寄存器）

* 偏移量可以是正或负，从指令起始字节起计算偏移量。偏移量在（2字节每1偏移量递增/递减）时解释执行。负偏移量用二进制补码格式存储。偏移量当前位置是指令起始字节。

* 比较操作，如果第一个操作数大于第二个操作数返回正值；如果两者相等，返回0；如果第一个操作数小于第二个操作数，返回负值。

* 调用参数表的编译比较诡异。如果参数的数量大于4并且%4=1，第5（第9或其他%4=1的）个参数将编译在指令字节的下一个字节的4个最低位。奇怪的是，有一种情况不使用这种编译：方法有4个参数但用于编译单一参数，指令字节的下一个字节的4个最低位空置，将会编译为40而不是04。

参考 https://www.cnblogs.com/liweis/p/4653496.html