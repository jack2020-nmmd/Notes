## less的变量
```css
    @orange : #ff6600
    @f18 {font-size : 18px} 
    这是声明了一个orange变量,后面可以直接使用这个变量,当大量使用时要更改方便
    变量的使用和定义都要用@符号
```
## 支持嵌套
```css
    与stylus一样,子元素可直接写在父元素里面,省略父元素,如:
    .class{
        font-size : 16px;
        .btn{
            //这个btn就是class的子元素
            &.confirm{
                //这个不是子元素,而是串联选择器,就是要有btn这个类,还要有confirm才可以
            }
            &:hover{
                //这是在btn类里面悬浮才可以触发
            }
        }
    }
```
## 混合
```css
    混合可以将一个定义好的class A轻松的引入到另一个class B中,从而实现class B继承class A中的所有属性,还可以带参数调用,像使用函数一样.

```
### 单参数混合
```css
    .classA(@radio:5px){//一个函数一样
        border-radius: @radio
    }
    classB调用classA,不带参数:
    .classB{
        font-size : 18px;
        .classA;
    }
    classC调用classA,带参数:
    .classC{
        .classA(10px)
    }
```
### 多参数混合
```css
    .box-shadow(@x:0, @y:0,@z:6px,@color:rgba(0,0,0,0.6)){
        box-shadow:@arguments
    }
这个arguments就是所有参数,就像js里面的伪数组一样,调用:
    .class{
        .box-shadow(0, 1px, 8px)//因为有默认值,最后一个不写就取默认的
    }
```
## 命名空间
```css
    假设一个div有一个类.btn{
        font-size : 18px;
        color : red
    }
    另一个div的类想用这个样式,则:div{
        .btn;
        font-weight : bold //相当于引用了btn的样式再接着添加自己需要的样式
    }
```
## 运算
```css
    @width : 300px
    @c-white : #fff
    .classA{
        width : @width/2;
        border : 4px solid @c-white + #333
    }

```
## 作用域
```css
    @var : red
    .page{
        @var : green
        .classA{
            color : @var;//green
        }
    }
    .classB{
        color : @var // red
    }
```
## 字符串插值
```css
    @base-url : "http://sso.bdtatatic.com"
    .msg{
        width : 20px;
        height : 20px;
        background : #fff url("@{base-url}/5av46545564.png") no-repeat
    }//像@{name}这样的结构:变量要用括号包起来

```