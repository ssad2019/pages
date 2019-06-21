1.命名基本原则

在面向对象编程中，对于类，对象，方法，变量等方面的命名应该本着描述性以及唯一标识性这两大特征来命名，才能保证资源之间不冲突，并且每一个都便于记忆。

命名原则是: 使名称足够长以便有一定的意义，并且足够短以避免冗长。

2.命名基本规范

2.1 编程基本命名规范

(1) 避免难懂的名称，如属性名xxK8，这样的名称会导致多义性

(2) 在面向对象的语言中，在类属性的名称中包含类名是多余的，如 Food.foodName，而是应该使用 Food.name

(3) 使用动词-名词的方法来命名函数，如 getCount()

(4) 只要合适，在变量名的末尾或开头加计算限定符，如 Avg、Sum、Min、Max、Index

(5) 布尔变量名应该包含Is，这意味着Yes/No 或 True/False 值，如 blnIsExist

(6) 即使对于可能仅出现在几个代码行中的生存期很短的变量，仍然使用有意义的名称。仅对于短循环索引使用单字母变量名，如 i 或 j

(7) 不要使用原义数字或原义字符串，而是使用命名常数，如 NUM_DAYS_IN_WEEK ，以便于维护和理解

2.2 分类命名规范

(1) 包的命名 　 

Java包的名字都是由小写单词组成。但是由于Java面向对象编程的特性，每一名Java程序员都可以编写属于自己的Java包，为了保障每个Java包命名的唯一性，在最新的Java编程规范中，要求程序员在自己定义的包的名称之前加上唯一的前缀。由于互联网上的域名称是不会重复的，所以程序员一般采用自己在互联网上的域名称作为自己程序包的唯一前缀。 

例如： package com.example.lianghw.easyeat;

(2) 类的命名

类的名字必须由大写字母开头而单词中的其他字母均为小写；如果类名称由多个单词组成，则每个单词的首字母均应为大写例如TestPage；如果类名称中包含单词缩写，则这个缩写词的每个字母均应大写，如：XMLExample,还有一点命名技巧就是由于类是设计用来代表对象的，所以在命名类时应尽量选择名词。

注意在命名java class时，命名应能够表现类的作用，如Activity的命名应当以Activity结尾，ListViewAdapter的命名应当以ListViewAdapter结尾，其他的工具类应能够通过名字大致看出其作用。 　　

例如: PayActivity, MyTypeListViewAdapter, StoredData, TypeListViewItem

(3) 资源文件的命名

资源文件的命名包括样式xml文件的命名以及资源id的命名。

xml文件的命名格式为: 样式文件的作用(item, activity, btn...)_描述，所有单词均小写，多个单词用 _隔开。

例如: activity_pay, activity_final, item_payment_type1

资源id的命名格式为与控件的命名规范相同，具体见下。

(4) 方法的命名 

方法的名字的第一个单词应以小写字母作为开头，后面的单词则用大写字母开头。

例如: onBackPressed()

(5) 常量的命名 

常量的名字应该都使用大写字母，并且指出该常量完整含义。如果一个常量名称由多个单词组成，则应该用下划线来分割这些单词。

例如: MAX_VALUE

(6) 参数的命名 

参数的命名规范和方法的命名规范相同，而且为了避免阅读程序时造成迷惑，请在尽量保证参数名称为一个单词的情况下使参数的命名尽可能明确

例如: private HashMap<String, Object> getHashMapFirstType(List<Food> listData)

(7) Javadoc注释 

Java除了可以采用我们常见的注释方式之外，Java语言规范还定义了一种特殊的注释，也就是我们所说的Javadoc注释，它是用来记录我们代码中的API的。Javadoc注释是一种多行注释，以/**开头，而以*/结束，注释可以包含一些HTML标记符和专门的关键词。使用Javadoc注释的好处是编写的注释可以被自动转为在线文档，省去了单独编写程序文档的麻烦。 

在每个程序的最开始部分，一般都用Javadoc注释对程序的总体描述以及版权信息，之后在主程序中可以为每个类、接口、方法、字段添加Javadoc注释，每个注释的开头部分先用一句话概括该类、接口、方法、字段所完成的功能，这句话应单独占据一行以突出其概括作用，在这句话后面可以跟随更加详细的描述段落。在描述性段落之后还可以跟随一些以Javadoc注释标签开头的特殊段落，例如上面例子中的@auther和@version，这些段落将在生成文档中以特定方式显示。

例如:

/**

* 加法运算

* 输入两个数，得到两个数之和

* @param a int
* @param b int
* @return a+b int

*/ 

Int add(Int a, Int b)

3.分类命名规范

3.1 基本数据类型命名规范

|数据类型|命名规范|
|-|-
|Integer|int_描述, 如: int_count|
|Boolean|bln_描述, 如: bln_exist|
|Char|chr_描述, 如: chr_input|
|String|str_描述, 如: str_name|
|Short|shr_描述, 如: shr_total|
|Long|lng_描述, 如: lng_total|
|Float|flt_描述, 如: flt_total|
|Double|dbl_描述, 如: dbl_count|
|List|list_描述, 如: list_data|
|Object|obj_描述, 如: obj_key|


3.2 控件命名规范 

|控件|命名规范|
|-|-
|TextView|txt_描述, 如: txt_total|
|Button|btn_描述, 如: btn_confirm|
|ImageView|img_描述, 如: img_restaurant|
|EditText|edit_描述, 如: edit_remark|
|View|view_描述， 如: view_line|
|ListView|lv_描述, 如: lv_type|
|CardView|card_描述, 如: card_info|

3.3 变量命名规范

能够明确表示变量类型和变量描

例如: Food food_item, Intent intent...

3.4 程序规范

工程命名: 描述

应用程序命名: 描述+App

4.代码书写规范

(1) java代码中不出现中文，允许""和注释内出现中文

(2) 使用shape和selector

(3) 复杂布局使用ConstraintLayout

(4) 自适应屏幕，使用dp代替px

(5) 在括号对对齐的位置垂直对齐左括号和右括号

5.注释

软件文档以两种形式存在: 外部和内部。外部文档（如规范、帮助文件和设计文档）在源代码的外部维护，内部文档由开发人员在开发时在源代码中编写的注释组成。以下是几点规范的注释方法:

(1) 一个工程应有一个统一的头文件注释，以说明整个工程的信息、创建日期、版本等等

(2) 对重要的程序加注释进行说明

(3) 避免杂乱的注释，而是应该使用空白将注释同代码分开

(4) 移除所有临时或无关的注释，以避免在日后的维护工作中产生混乱

(5) 注释应对代码进行准确的说明，不应存在歧义

(6) 在整个应用程序中，使用具有一致的标点和结构的统一样式来构造注释
