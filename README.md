Sass是一个**CSS预处理器**，CSS预处理器是用一种专门的编程语言，进行页面样式设计，然后再编译成正常的CSS文件 

Sass在 CSS 语法的基础上增加了**变量**、**嵌套**、**混合**、**继承**等高级功能。

优点：它能用来**简洁的**、**清晰的**、**结构化**地描述文件样式,更易于**代码的维护**

## Sass与Scss区别
Sass和Scss其实是**同一种东西**，我们平时都称之为Sass,两者之间的不同在于：

1. **文件扩展名** **不同**，sass是“.sass”后缀为扩展名，而scss是以“.scss”后缀为扩展名；
2. **语法书写方式不同**，sass是以严格的缩进式语法规则来书写，也就是说不带大括号{}和分号，而scss的语法书写和CSS语法书写方式非常类似。

`Sass`写法：

   
    $body-fontsize: 12px// 定义变量
	$bg-color: #000  // 定义变量
	body 
        font-size: $body-fontsize 
        background-color: $bg-color
	
`Scss`写法：

    
	$body-fontsize: 12px;// 定义变量
	$bg-color: #000;  // 定义变量
	body { 
        font-size: $body-fontsize; 
        background-color: $bg-color;
	}
	
    //编译后的css:
	
	body { 
        font-size: 12px; 
        background-color: #333;
	}

    
	
Angular中Scss的安装


		npm install node-sass --save-dev //需要手动安装node-sass
	
		ng new My_New_Project --style=scss //自动生成Scss样式文件

## Sass快速入门(以下Sass代码都使用Scss格式)
    
1. **使用变量**

	`sass`使用**`$`**符号来标识变量

 	`Sass`变量的声明和`css`属性的声明很像：

    	    $button-color: #F90;//声明变量
			$button-width: 100px;
			$button-height: 50px;
    		.button{
				width: $button-width;//引用变量
				height: $button-height;
				background-color： $button-color;
			}

			///编译后css****

			.button{
				width: 100px;
				height: 50px;
				background-color： #F90;
			}
	**全局变量**和**局部变量**

		$color: #000； // 定义全局变量
	    .block { 
			color: $color;  // 调用全局变量
		}
		em { 
			$color: #fff;  // 定义局部变量
    		a{
		    	color: $color;  // 调用局部变量
		    }
		}
		span {
		    color: $color;  // 调用全局变量
		}

		//编译后
		.block { 
			color: #000;  
		}
		em a{ 
		    color: #fff;  
		}
		span {
		    color: #000;  
		}




2. **嵌套**
 
	**选择器嵌套**

	css中重复写选择器是非常恼人的。如果要写一大串指向页面中同一块的样式时，往往需要 一遍又一遍地写同一个ID：
	
	css：

        #footer nav li { margin-bottom: 20px }
		#footer nav a { color: #333 }	
		#footer p { background-color: #EEE }

	Sass：

		#footer {
			nav {
				li { margin-bottom: 20px }
				a { color: #333 }
			}
			p { background-color: #EEE }
		}

		 /* 编译后结果 */
		#footer nav li { margin-bottom: 20px  }
		#footer nav a { color: #333 }
		#footer p { background-color: #EEE }
	
    
	**属性嵌套**

		.box {
			border: {
				top: 1px solid red;
				bottom: 1px solid green;
		  	}
		}

		/* css */
		.box {
		    border-top: 1px solid red;
		    border-bottom: 1px solid green;
		}

	**伪类嵌套**

		//父选择器的标识符&amp;

		.button{
			&amp;:hover,
			&amp;:active {  //群组选择器的嵌套
				background-color:#000;
			}
		}
		/* css */
	
		.button:hover,.button:active { 
				background-color:#000;
		}

	
3. **混合器**

	通过sass的混合器实现大段样式的重用

	混合器使用**`@mixin`**标识符定义,样式表中通过`@include`来使用这个混合器

	    @mixin round-border {
	      -moz-border-radius: 5px;
	      -webkit-border-radius: 5px;
	      border-radius: 5px;
	    }
		
		.botton{
			width:100px;
			height:50px;
			background:#EEE;
			@include round-border;
		}
		//sass最终生成
		.botton{
			width:100px;
			height:50px;
			background:#EEE;
			-moz-border-radius: 5px;
		    -webkit-border-radius: 5px;
		    border-radius: 5px;
		}

	混合器传参

	可以通过在`@include`混合器时给混合器传参，来定制混合器生成的精确样式。
	
    	@mixin link-colors($default, $hover ) {
     		color: $default;
      		&amp;:hover { color: $hover; }
    	} 

    	a {
     	 @include link-colors(blue, red);
	    }
	    
	    //Sass最终生成的是：
	    
	    a { color: blue; }
	    a:hover { color: red; }

	有时候可能会很难区分每个参数是什么意思，参数之间是一个什么样的顺序
	sass允许通过语法$name: value的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可。
		
		a {
		@include link-colors(
		  $default: blue,
		  $hover: red
		  );
		}

	**特点**:可传参

	**缺点**:如果在样式文件中调用同一个混合器，会产生多个对应的样式代码，造成代码的冗余。

4. **继承**

	选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过`@extend`语法实现，如下:

		//通过选择器继承继承样式
		.error {
			border: 1px solid red;
		    background-color: #fdd;
		}
		.box {
		    @extend .error; // 修饰的html元素最终的展示效果就好像是class=&quot;box error&quot;
		    border-width: 3px;
		}
    	
    **继承了.error的元素，同时也会继承 error 有关的组合选择器样式。**
	
		.error a{  //应用到.box a
     		color: red;
      		font-weight: 100;
    	}
		//编译后
		.error, .box {
		  width: 70px;
		  height: 70px;
		  border: 1px solid red;
		  background-color: #fdd; 
		}
		
		.box {
		  border-width: 3px; 
		}
		
		.error a, .box a{
		  color: red;
		  font-weight: 100; 
		}
		
	

5. **占位符** **%**
	
	占位符也是一个非常强大的功能，和继承也有着密切的关系。

	比如说，我们有多个类都有相同的代码共有，需要继承同一个基类。

	那么像上边继承的写法有产生了代码的冗余，最终会编译出多余的代码。

	用占位符声明的代码，在**不被@extend调用的情况下，是不会产生任何代码的**。


		%marginTOP5 {  
		  margin-top: 5px;  
		}  

		%paddingTop5{  
		  padding-top: 5px;  
		}  
		  
		.btn {  
		  @extend %marginTOP5;  
		  @extend %paddingTop5;  
		}  
		  
		.block {  
		  @extend %marginTOP5;  
		  
		  span {  
			@extend %marginTOP5;  
		  }  
		}  

		//编译后  
		.btn, .block {  
		  margin-top: 5px;  
		}  
		  
		.btn, .block span {  
		  padding-top: 5px;  
		}  

	
出色的完成了代码合并，且基类并没有被编译出来，只作用与调用了它的类中。




6. **混合器、继承、占位符的比较：**
	

&gt; **混合器：**可以传参数，但是代码冗余，相同样式不会合并选择器。所以适合在我们多次使用相同样式，但是值不同的情况下使用。
	&gt; 
	&gt;**继承：**不能传参数，代码会自动合并，不会冗余。适合于不需要传参的情况下复用代码，并且基类代码具有作用的情况。
	&gt; 
	&gt; **占位符**：同上，但是区别是在基类没有作用的情况下使用。不会占用css文件大小。

7. **插值#{}**

	将一个占位符，替换成一个值。简单的栗子如下：
		
		//给按钮添加边框
		$button-color: #F90;//声明变量
		$button-width: 100px;
		$button-height: 50px;
		$vue: border; 
		@mixin more($wid, $sty, $col){
  			#{$vue}: #{$wid} #{$sty} #{$col}; //其实就是结合混合宏传多个参数;
		}
		
		.button{
			width: $button-width;//引用变量
			height: $button-height;
			background-color: $button-color;
			@include more(2px,solid,#000)
		}
		//编译后
		.button{
			width: 100px;
		    height: 50px;
		    background-color: #F90;
			border: 2px solid #000;
		}
				

	构建选择器:

	
       //用过bootstrap的都知道，它的button有个预定义样式：
	
	     
		//html
		&lt;button type=&quot;button&quot; class=&quot;btn btn-success&quot;&gt;Success&lt;/button&gt;
    	&lt;button type=&quot;button&quot; class=&quot;btn btn-info&quot;&gt;Info&lt;/button&gt;
    	&lt;button type=&quot;button&quot; class=&quot;btn btn-warning&quot;&gt;Warning&lt;/button&gt;
    	&lt;button type=&quot;button&quot; class=&quot;btn btn-danger&quot;&gt;Danger&lt;/button&gt;
		
		//css
		
		@mixin button-style($c, $s, $i, $w, $d, $sc, $ic, $wc, $dc){
		    .#{$c}-success{ background-color: $s; border-color:$sc; }
		    .#{$c}-info   { background-color: $i; border-color:$ic; }
		    .#{$c}-warning{ background-color: $w; border-color:$wc; }
		    .#{$c}-danger { background-color: $d; border-color:$dc; }
		}
		
		.btn{
		  width: 100px;
		  height: 50px;
		  color: #fff;
		}
		@include button-style(&#39;btn&#39;, #5cb85c, #5bc0de, 
		#f0ad4e, #d9534f, #4cae4c, #46b8da, #eea236, #d43f3a); 

		//被编译后
		.btn {
		  color: #fff;
		}
		.btn-success {
		  background-color: #5cb85c;
		  border-color: #4cae4c;
		}
		.btn-info {
		  background-color: #5bc0de;
		  border-color: #46b8da;
		}
		.btn-warning {
		  background-color: #f0ad4e;
		  border-color: #eea236;
		}
		.btn-danger {
		  background-color: #d9534f;
		  border-color: #d43f3a;
		}
		
	
**注意：混合器中使用插值#{}的限制**
	
	
除了构建选择器，在混合器里面是**不能拼接变量名**以及**不能拼接混合器名**。

		
		//错误示范一
		@mixin updated-open {
		    margin-top: 20px;
		    background: #F00;
		}
		$val: &quot;open&quot;;
		.navigation {
		    @include updated-#{$flag}; //不能拼接混合器名，在编译成 CSS 时会报错。
		}
		
		//错误示范二
		$margin-big: 40px;
		$margin-small: 12px;
		
		@mixin set-value($size) {
    		margin-top: $margin-#{$size};//不能拼接变量名，在编译成 CSS 时会报错。
		}
		
		.box {
		    @include set-value(big);
		}

	

8. **静默注释**

	sass另外提供了一种不同于css标准注释格式/* ... */的注释语法，即静默注释。

	    body {
		  color: #333; // 这种注释内容不会出现在生成的css文件中
		  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
		}



9. **Sass运算**
	
	**加法**：参数可以单位，但需要单位同一类型，运算符左右不加空格也可编译。
	
		$sidebar-width: 220px;
		$content-width: 720px;
		$gap-width: 20px;
 		.container {
	 		width: $sidebar-width+$content-width + $gap-width;
	 		margin: 0 auto;
 		}
		//编译后
		.container {
		   width: 960px;
		   margin: 0 auto; 
		}

	**减法**：参数可以单位，但需要单位同一类型，运算符两边**需加上空格**。
		
		$full-width: 960px;
		$sidebar-width: 200px;
		
		.content {
		  width: $full-width -  $sidebar-width;
		}
		//编译后
		.content {
		  width: 760px;
		}
	
	**乘法**：参数可以单位，但需要单位同一类型，一个单位同时声明两个值时会有问题。
		
		//错误
		.box {
			width:10px * 2px;  
		}
		//正确
		.box {
  			width: 10px * 2;//只需要为一个数值提供单位即可
		}
	
	**除法**：直接使用“/”符号做为除号时，将不会生效，编译时既得不到我们需要的效果，也不会报错。
		
		.box {
		  width: 100px / 2;  
		}
		//编译后
		.box {
		  width: 100px / 2;  
		}
		
	添加小括号即可

		.box {
		  width: （100px / 2）;  
		}
		//编译后
		.box {
		  width: 50px;  
		}
	当用**变量进行除法运算**时，“/”符号也会**自动被识别成除法**
		
		$width: 1000px;
		.box {
		  width: $width / 10;  
		}
		
	当两个值带有相同的单位值时，除法运算之后会得到一个**不带单位的数值**

		.box {
		  width: (1000px / 100px);
		}
		//编译后
		.box {
		  width: 10;
		}
