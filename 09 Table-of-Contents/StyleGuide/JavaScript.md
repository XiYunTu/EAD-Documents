## JavaScript 语言规范

### 变量

声明变量必须加上 var 关键字。

### 常量

常量的形式如: NAMES_LIKE_THIS, 即使用大写字符, 并用下划线分隔；你也可用@const 标记来指明它是一个常量. 但请永远不要使用 const 关键词。

### 分号

总是使用分号。

### 嵌套函数

可以。

### 块内函数声明


	if (x) {
		function foo() {}
	}

不可以。

### 异常

可以。

### 自定义异常

可以。

### 标准特性

总是优于非标准特性。

### 封装基本类型

不要。

### 方法定义

### 闭包

谨慎使用。

### eval()

只用于解析序列化字符串（例如：JSON）；

### with{}

不可以。

### this

仅在对象构造器, 方法, 闭包中使用。

### for-in{}循环

只用于 object/map/hash 的遍历。

### 多行字符串

不可以。

### Array 和 Object声明

使用Array 和 Object 直接量。

### 修改内置对象的原型

不可以。

## Javascript编码风格

### 命名

- 通常 

	- 函数方法名： `functionNamesLikeThis`
	- 变量名： ` variableNamesLikeThis`
	- 类名： `ClassNamesLikeThis`
	- 枚举对象： `EnumNamesLikeThis`
	- 方法名： `methodNamesLikeThis`
	- 常量： `SYMBOLIC_CONSTANTS_LIKE_THIS`
	
- 属性和方法

	- 文件或类中的 私有 属性, 变量和方法名应该以下划线 "_" 开头
	- 保护 属性, 变量和方法名不需要下划线开头, 和公共变量名一样
	
- 方法和函数参数

	- 可选参数以 `opt_` 开头.
	- 函数的参数个数不固定时, 应该添加最后一个参数 `var_args` 为参数的个数. 你也可以不设置 `var_args` 而取代使用 `arguments`。
	- 可选和可变参数应该在 @param 标记中说明清楚。 虽然这两个规定对编译器没有任何影响, 但还是请尽量遵守。

- 命名空间

	- JavaScript 不支持包和命名空间.
	- 不容易发现和调试全局命名的冲突, 多个系统集成时还可能因为命名冲突导致很严重的问题. 为了提高 JavaScript 代码复用率, 我们遵循下面的约定以避免冲突。

- 为全局代码使用命名空间

- 明确命名空间所有权
- 外部代码和内部代码使用不同的命名空间
- 重命名那些名字很长的变量, 提高可读性
- 文件名：文件名应该使用小写字符, 以避免在有些系统平台上不识别大小写的命名方式. 文件名以.js结尾, 不要包含除 - 和 _ 外的标点符号(使用 - 优于 _)。

### 延迟初始化

可以。

### 明确作用域

任何时候都需要。

### 代码格式化

- **大括号**

	分号会被隐式插入到代码中, 所以你务必在同一行上插入大括号。

- **函数参数**

	尽量让函数参数在同一行上. 如果一行超过 80 字符, 每个参数独占一行, 并以4个空格缩进, 或者与括号对齐, 以提高可读性. 尽可能不要让每行超过80个字符。

- **空行**

	使用空行来划分一组逻辑上相关联的代码片段。

### 二元和三元操作符

操作符始终跟随着前行, 这样就不用顾虑分号的隐式插入问题. 如果一行实在放不下, 还是按照上述的缩进风格来换行。

### 括号

不要滥用括号, 只在必要的时候使用它.

对于一元操作符(如`delete`, `typeof` 和 `void` ), 或是在某些关键词(如 `return`, `throw`, `case`, `new` )之后, 不要使用括号.

### 字符串

使用 `'` 优于 `“”`。

### 可见性 (私有域和保护域)

推荐使用 JSDoc 中的两个标记: @private 和 @protected。

### 注释

使用JSDoc。

### 编译

### 提示和技巧

- True 和 False 布尔表达式

	下面的布尔表达式都返回 `false`:

	- `null`;
	- `undefined`;
	- `''` 空字符串;
	- `0`  数字0.

	但小心下面的, 可都返回 `true`:
	- `'0'` 字符串0
	- `[]` 空数组
	- `{}` 空对象

	注意: 还有很多需要注意的地方, 如:
	
	- Boolean('0') == true
	- '0' != true
	- 0 != null
	- 0 == []
	- 0 == false
	- Boolean(null) == false
	- null != true
	- null != false
	- Boolean(undefined) == false
	- undefined != true
	- undefined != false
	- Boolean([]) == true
	- [] != true
	- [] == false
	- Boolean({}) == true
	- {} != true
	- {} != false

- 条件(三元)操作符 (?:)

	三元操作符用于替代下面的代码:

		if (val != 0) {
  			return foo();
		} else {
  			return bar();
		}

	你可以写成:

		return val ? foo() : bar();

- && 和 ||

	二元布尔操作符是可短路的, 只有在必要时才会计算到最后一项.

- 使用 join() 来创建字符串

## 保持一致性

当你在编辑代码之前, 先花一些时间查看一下现有代码的风格. 如果他们给算术运算符添加了空格, 你也应该添加. 如果他们的注释使用一个个星号盒子, 那么也请你使用这种方式.

代码风格中一个关键点是整理一份常用词汇表, 开发者认同它并且遵循, 这样在代码中就能统一表述. 我们在这提出了一些全局上的风格规则, 但也要考虑自身情况形成自己的代码风格. 但如果你添加的代码和现有的代码有很大的区别, 这就让阅读者感到很不和谐. 所以, 避免这种情况的发生.