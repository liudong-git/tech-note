# note

- 有两种方法来表示空列表:用一对不包括任何东西的括号来表示，或用符号 nil 来表示空表。
- `let`是一个最常用的 Common Lisp 的操作符之一，它引入新的局部变量（local variable）
- 给`defparameter`传入符号和值，来创建一个全局变量
- 用`defconstant`来定义一个全局的常量
- 检查某些符号，是否为一个全局变量或常量，使用`boundp`函数
- 普遍的赋值操作符（assignment operator）是`setf`
- 如果`setf`的第一个实参是符号（symbol），且符号不是某个局部变量的名字，则`setf`把这个符号设为全局变量
- `(setq *print-case* :capitalize)`

# 列表操作 (List Operations)

- 用函数`cons`来构造列表
- 取出列表元素的基本函数是`car`和`cdr`
- `listp`
- 用`defun`来定义新函数
- 递归 (Recursion)

-----

# 实用 Common Lisp 编程

## 第2章 周而复始: REPL 简介

- read-eval-print loop
- NIL是lisp版本的逻辑假和空值
- 使用`load`加载一个文件
- 另一种加载文件中有用定义的方法是,先用`compile-file`编译,然后再用`load`加载编译后产生的文件,也就是FASL文件-快速加载文件(fast-load file)的简称.`compile-file`将返回FASL文件的名字.`(load (compile-file "file.lisp"))`

### 第一个程序

```
(format t "hello, world!")
(write-line "hello, world!")
(print "hello, world!")
```

## 第3章 简单的数据库

- 属性列表(property list, plist): 从第一个元素开始的所有相间元素都是一个用来描述接下来的那个元素的符号.
- 关键字符号是任何以冒号开始的名字.
- `getf`接受一个plist和一个符号,并返回plist中跟在那个符号后面的值.
- 使用`defvar`宏来定义全局变量,名字中的星号是lisp的全局变量命名约定.
- `t`是标准输出流(\*standard-output\*)的简称
- `~a`指令是美化指令,它的意图是消耗一个实参,然后将其输出成人类可读的形式.这将使得关键字被渲染成不带前导冒号的形式,而字符串也不再有引号了.
- `-t`指令用于制表.`~10t`告诉`format`产生足够的空格,以确保在处理下一个`~a`之前光标移动10列.`~t`指令不使用任何实参.
- `~%` 指令并不消耗任何实参,而是告诉`format`来产生一个换行.
- `setf`宏是CommonLisp最主要的赋值操作符.它将其第一个参数设置成其第二个参数的求值结果.
- 函数`remove-if-not`接受一个谓词和一个原始列表,然后返回一个仅包含原始列表中匹配该谓词的所有元素的新列表.谓词参数可以是任何接受单一参数并能返回布尔值的函数---除了NIL表示假以外其余的都表示真.
- `#'`记号是"获取函数,其名如下"的简称.
- `lambda`并不是函数的名字,它只是一个表明正在定义匿名函数的指示器.但除了缺少名字以外,一个`lambda`表达式看起来很像一个`defun`:单词`lambda`后面紧跟着形参列表,然后是函数体.
- 关键字形参(keyword parameter),形参列表开始处有一个`&key`.如果一个特定的关键字在调用中没有指定,对应的变量将被设置成NIL.
- 为了区分作为实参显示传递给关键字形参的NIL和作为默认值的NIL,可以将简单的名称替换成一个包括形参名,默认值和另一个称为`supplied-p`形参的列表.`supplied-p`形参可被设置成真或假,具体取决于实参在特定的函数调用里是否真的被传入相应的关键字形参中.
- `remove-if`所返回的列表中,所有确实匹配谓词的元素都被删除.
- `reverse`函数授受一个列表作为参数并返回一个逆序的新列表.
- 宏与函数的主要词法差异在于需要用`defmacro`而不是`defun`来定义一个宏.
- 在一个反引用表达式里,任何以逗号开始的子表达式都是被求值的.
- `,@`是`,`的变体,可以将接下来的表达式(必须求值成一个列表)的值嵌入到其外围的列表里.
- `&rest`改变了解析参数的方式.当参数列表里面带有`&rest`时,一个函数或宏可以接受任意数量的实参,它们将被收集到一个单一列表中,并成为那个跟在`&rest`后面的名字对应的变量的值.

## 第4章 语法和语义

- `S-表达式`的基本元素是列表(list)的原子(atom).列表由括号所包围,并可包含任何数量的由空格所分隔的元素.原子是所有其他内容.
- 列表元素本身也可以是`S-表达式`(换句话说,也就是原子或嵌套的列表).
- 注释从技术角度来讲不是`S-表达式`,它们以分号开始,直到一行的结尾,本质上将被当作空白来处理.
- 字符串是由双引号所包围着的.在字符串中,一个反斜杠会转义接下来的任意字符,使其被包含在字符串里.两个在字符串中**必须被转义**的字符是双引号和反斜杠本身.
- 有十个字符被用于其他句法目的而不能出现在名字里,它们是:开括号和闭括号,双引号和单引号,反引号,逗号,冒号,分号,反斜杠以及竖线.
- 无论是任何地方,同样的名字出现在任何`S-表达式`里,都会用同一个对象去表示它.
- 全局变量名字在开始和结尾处带有"\*".常量名都以"+"开始和结尾.
- 函数调用形式(function call form),宏形式(macro form)和特殊形式(special form).
- 在`Common Lisp`中一个符号可以同时为操作符(函数,宏或特殊操作符)和变量命名.
- 通常作为宏的循环结构都带有一个以`do`开始的名字.
- NIL是唯一一个既是原子又是列表的对象:除了用来表示假以外,它还用来表示空列表.
- 表达式nil,(),'nil以及'()求值结果是相同的---未引用形式将被看成是对值为符号NIL的常值变量的引用来进行求值,而在引用形式中,QUOTE特殊操作符将会直接求解出符号NIL.
- t和't的求值结果完全相同:符号T.
- `=`用来比较数字,`CHAR=`用来比较字符

### 函数调用

函数调用形式的基本语法应如下所示,其中每个参数本身也是一个Lisp形式:

```
(function-name argument*)
```

### 特殊操作符

IF的规则:求值第一个表达式.如果得到非NIL,那么求值下一个表达式并返回它的值.否则,返回第三个表达式的求值,或者如果第三个表达式被省略的话,返回NIL.形式如下:

```
(if test-form then-form [ else-form ])
```

QUOTE操作符接受一个单一表达式作为其"参数"并简单地返回它,不经求值.它被用得相当普遍,读取器中内置了一个它的特别语法形式:`'`

LET用来创建新的变量绑定.

宏形式的求值过程包括两个阶段:首先,宏形式的元素不经求值即被传递到宏函数里;其次,由宏函数所返回的形式(称其为展开式(expansion))按照正常的求值规则进行求值.

### 真,假和等价

符号NIL是唯一的假值,其他所有的都是真值.符号T是标准的真值,可用于需要返回一个非NIL值却又没有其他值可用的情况.

### "通用"等价谓词

- EQ用来测试"对象标识",只有当两对象相同时才是EQ等价.
- EQL可获得与EQ相似的行为,除此之外,它也可以保证当相同类型的两个对象表示相同的数字或字符值时,它们是等价的.
- EQUAL和EQUALP可以操作在所有类型的对象上,它们每个都定义了相比EQL稍微宽松一些的等价性,允许认为不同的对象是等价的.
  - EQUAL相比EQL的宽松之处在于,它将在递归上具有相同结构和内容的列表视为等价.
  - EQUALP在考察两个含有相同字符串的等价时忽略了大小写的区别.

## 第5章 函数

- 宏运行在编译期,它们生成的代码,即当所有宏被展开后将实际构成程序的那些代码,将完全由对函数和特殊操作符的调用所构成.宏本身也是函数---用来生成代码,而不是用来完成实际的程序操作.
- 任何符号都可用作函数名.
- 如果一个字符串紧跟在形参列表之后,那么它应该是一个用来描述函数用途的文档字符串.定义函数时,该文档字符串将被关联到函数名上,并且以后可通`DOCUMENTATION`函数来获取.
- 一个`DEFUN`的主体可由任意数量的Lisp表达式构成,它们将在函数被调用时依次求值,而最后一个表达式的值被作为整个函数的值返回.
- `RETURN-FROM`特殊操作符可用于从函数的任何位置立即返回.
- 如果一个函数以过多或过少的实参来调用的话,Lisp会报错.

### 定义新函数

```
(defun name (parameter*)
  "Optional documentation string."
  body-form*)
```

### 可选形参

- 为了定义一个带有可选形参的函数,在必要形参的名字之后放置符号`&optional`,后接可选形参的名字.
- 如果实参在所有的可选形参被赋值之前用完了,那么其余的可选形参将自动绑定到值`NIL`上.
- 如果想要一个不同于`NIL`的默认值,可以通过将形参名替换成一个含有名字跟一个表达式的列表来指定该默认值.只有在调用者没有传递足够的实参来为可选形参提供值的时候,表达式才会被求值.
- 默认表达式可以引用早先出现在形参列表中的形参.
- 如果要确认一个可选形参的值究竟是被调用者明确指定还是使用了默认值,可以通过在形参标识符的默认值表达式之后添加另一个变量,该变量将在调用者实际为该形参提供了一个实参时被绑定到真值,否则为`NIL`.通常约定,这种变量的名字与对应的真实形参相同,但是带有一个`-supplied-p`后缀.

### 剩余形参

Lisp允许在符号`&rest`之后包括一揽子形参.如果函数带有`&rest`形参,那么任何满足了必要和可选形参之后的其余所有实参就将被收集到一个列表里成为该`&rest`形参的值.

### 关键字形参

- 关键字形参允许调用者指定具体形参相应所使用的值.为了使函数带有关键字形参,在任何必要的`&optional`和`&rest`形参之后,可以加上符号`&key`以及任意数量的关键字形参标识符,后者的格式类似于可选形参标识符.
- 关键字形参也可以提供一个默认值形式以及一个`supplied-p`变量名.
- 在关键字形参和可选形参中,默认值形式都可以引用那些早先出现在形参列表中的形参.

### 混合不同的形参类型

- 声明顺利:首选是必要形参,其次是可选形参,再次是剩余形参,最后才是关键字形参.

### 函数返回值

- 使用`RETURN-FROM`特殊操作符,能立即以任何值从函数中间返回.
- `RETURN-FROM`其第一个"参数"是它想要返回的代码块名.该名名字不被求值,因此无需引用.

### 作为数据的函数---高阶函数

- 在Lisp中,函数只是另一种类型的对象.在用`DEFUN`定义一个函数时,实际上做了两件事:创建一个新的函数对象以及赋予其一个名字.
- 特殊操作符`FUNCTION`提供了用来获取一个函数对象的方法.它接受单一实参并返回与该参数同名的函数.
- `#'`语法是`FUNCTION`的语法糖.
- CommonLisp提供了两个函数用来通过函数对象调用函数:`FUNCALL`和`APPLY`.
  - `FUNCALL`用于编写代码时确切知道传递给函数多少实参时.其第一个实参是被调用的函数对象,其余的实参被传递到该函数中.
  - `APPLY`的第一个参数是一个函数对象,在这个函数对象之后,它期待一个列表而非单独的实参.它会将函数应用在列表中的值上.
  - `APPLY`还接受"孤立"(loose)的实参,只要最后一个参数是个列表.

### 匿名函数

- 觉得没有必要用`DEFUN`来定义一个新函数时,可以使用一个`LAMBDA`表达式创建匿名的函数.
- 可以将`LAMBDA`表达式视为一种特殊类型的函数名,其名字本身直接描述函数的用途.这就解释了为什么可以使用一个带有`#'`的`LAMBDA`表达式来代替一个函数名.
- 在需要传递一个作为参数的函数给另一个函数,并且需要传递的这个函数简单到可以内联表达时,匿名函数特别有用.
- `LAMBDA`表达式的另一项重要用途是制作闭包(closuer),即捕捉了其创建环境信息的函数.

形式如下:

```
(lambda (parameters) body)
```

## 第6章 变量

- CommonLisp支持两种类型的变量:词法(lexical)变量和动态(dynamic)变量.

### 变量的基础知识

- 一个变量可以保存任何类型的值,这些值带有可用于运行期类型检查的类型信息.
- CommonLisp是动态类型的---类型错误会被动态地检测到.
- CommonLisp是一种`强类型`语言,所有类型错误都将被检测到---无法将一个对象作为其不属于的类型的实例来对待.
- 从概念上来说,CommonLisp中所有的值都是对象的引用.将一个变量赋予新值就会改变该变量所指向的对象,而对之前被引用的对象却没有影响.
- 如果一个变量保存了对一个可变对象的引用,那么就可以用该引用来修改些对象,而这种改动将应用于任何带有相同对象引用的代码.
- 引入新变量的另一种方式是使用`LET`特殊操作符.

```
(let (variable*)
  body-form*)
```

- 其中每一个`variable`都是一个变量初始化形式.每个初始化形式要么是一个含有变量名和初值形式的列表,要么就是一个简单的变量名---作为将变量初始化到`NIL`的简略写法.
- 形式体中最后一个表达式的值将作为`LET`表达式的值返回.
- 函数形参和`LET`变量的作用域(变量名可用来引用该绑定的程序区域)被限定在引入该变量的形式之内,该形式即函数定义或`LET`,被称为`绑定形式`.
- 词法变量和动态变量使用两种略有不同的作用域机制,但两者的作用域都被界定在绑定形式之内.
- 如果嵌套了引入同名变量的绑定形式,那么最内层的变量绑定将`覆盖`外层的绑定.
- `LET*`是`LET`的变体,区别在于,在一个`LET`中,被绑定的变量名只能用在`LET`的形式体之内---`LET`形式中变量列表之后的那部分;但在一个`LET*`中,每个变量的初始值形式,都可以引用到那些在变量列表中早先引入的变量.

### 词法变量和闭包

- 默认情况下,CommonLisp中所有的绑定形式都将引入`词法作用域`变量.词法作用域的变量只能由那些在文上位于绑定形式之内的代码所引用.
- 当一个匿名函数含有一个对来自封闭作用域之内词法变量的引用时,这个匿名函数被称为一个`闭包`.

### 动态变量

- CommonLisp提供了两种创建全局变量的方式:`DEFVAR`和`DEFPARAMETER`.两种形式都接受一个变量名,一个初始值以及一个可选的文档字符串.
- 在被`DEFVAR`和`DEFPARAMETER`定义以后,该名字可用于任何位置来指向全局变量的当前绑定.
- 全局变量习惯上被命名为以`*`开始和结尾的名字.
- 两种形式的区别在于,`DEFPARAMETER`总是将初始值赋给命名的变量,而`DEFVAR`只有当变量未定义时才这样做.

### 常量

- 常量都是全局的,并且使用`DEFCONSTANT`定义.
- 命名约定,用以`+`开始和结尾的名字来表示常量.

```
(defconstant name initial-value-form [ documentation-string ])
```

### 赋值

- 一旦创建了绑定,可以对它做两件事:获取当前值以及为它设置新值.
- 为绑定赋予新值则要使用`SETF`宏---CommonLisp的通用赋值操作符.
- `SETF`返回最近被赋予的值.

```
(setf place value)
```

### 广义赋值

- `SETF`的第一个参数用来保存值的位置,而第二个参数提供了值.

### 其他修改位置的方式

- 宏`INCF`和`DECF`以默认为1的特殊数量对一个位置的值进行递增和递减.此类宏称为`修改宏`(modify macro),修改是建立在`SETF`之上的宏.
- `ROTATEF`在位置之间轮换它们的值.
- `SHIFTE`与`ROTATEF`相似,除了它将值向左侧移动而不是轮换它们---最后一个参数提供的值移动到倒数第二个参数上,而其他的值将向左移一个,第一个参数的最初值被简单地返回.

## 第7章 宏: 标准控制构造

- 核心语言有了宏,就有可能构造成新的语法.

### WHEN 和 UNLESS

```
(if condition then-form [else-form])
```

*condition*被求值,如果其值非`NIL`,那么*then-form*会被求值并返回其结果.否则,如果有`else-form`,它将被求值并返回结果.如果*condition*是`NIL`并且没有*else-form*,那么`IF`返回`NIL`.

- 特殊操作符`PROGN`可以按顺序执行任意数量的形式并返回最后一个形式的值.

```
(defmacro when (condition &rest body)
  `(if ,condition (progn ,@body)))

(defmacro unless (condition &rest body)
  `(if (not ,condition) (progn ,@body)))
```

### COND

```
(cond
  (test-1 form*)
    .
    .
    .
  (test-N form*))
```

主体中的每个元素都代表一个条件分支,并由一个列表所构成,列表中含有一个条件形式,以及零或多个当该分支被选择时将被求值的形式.这些条件形式按照分支在主体中出现的顺序被依次求值,直到它们中的一个求值为真.这时,该分支中的其余形式将被求值,且分支中最后一个形式的值将作为整个`COND`的返回值.如果该分支的其余形式之后不再含有其他形式,那么就将返回该条件形式的值.

### AND OR 和 NOT

- `NOT`接受单一参数并对其真值取反,当参数为`NIL`时返回`T`,否则返回`NIL`.
- `AND`和`OR`是宏,实现了对任意数量子表达式的逻辑合取和析取操作,并被定义成宏以便支持"短路"特性.

### 循环

### DOLIST 和 DOTIMES

- `DOLIST`在一个列表的元素上循环操作,使用一个依次持有列表中所有后继元素的变量来执行循环体.
- 如果想在列表结束之前中断一个`DOLIST`循环,则可以使用`RETURN`.
- `DOTIMES`是用于循环计数的高级循环构造,其基本模板和`DOLIST`非常相似.其中的`count-form`必须要能求值一个整数.通过每次循环,`var`所持有的整数依次从0到比那个数小1的每一个后继整数.

```
(dolist (var list-form)
  body-form*)

(dotimes (var count-form)
  body-form*)
```

### DO

- `DO`允许绑定任意数量的变量,并且变量值在每次循环中的改变方式也是完全可控的也可以定义测试条件来决定何时终止循环,并可以提供一个形式,在循环结束时进行求值来为`DO`表达式整体生成一个返回值.

```
(do (variable-definition*)
    (end-test-form result-form*)
  statement*)
```

当`end-test-form`求值为真时,`result-form`(结果形式)将被求值,且最后一个结果形式的值将被作为`DO`表达式的值返回.

### 强大的`LOOP`
