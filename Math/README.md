# Introduction

由于涉及到数学公式，因此我们首先介绍一下如何在GitBook Editor中使用`Latex`。

首先，所有Latex语法必须包含在一对`$$`符号之间，如：

```
$$ E=mc^2. $$
```

效果：$$ E=mc^2. $$

### 上下标

- `^{}`: 上标
- `_{}`: 下标

`f_2 = ax^3 + bx^2 + c`: $$ f_2 = ax^3 + bx^2 + c $$

### 分式

- `\frac{m}{n}`: n分之m
- `{m \over n}`: n分之m

`a = \frac{1}{3} + {20 \over b + c}`: $$a = \frac{1}{3} + {20 \over b + c}$$

### 开方

- `\sqrt{}`: 开平方
- `\sqrt[m]{n}`: n开m次方

`a = \sqrt{b} + \sqrt[3]{c}`: $$a = \sqrt{b} + \sqrt[3]{c}$$

### 累计求和

- `\sum_{i=m}^{n}`: 从m到n求和

`a = \sum_{i=1}^{100}f(i)`: $$a = \sum_{i=1}^{100}f(i)$$

### 累计求积

- `\prod_{i=m}^{n}`: 从m到n求积

`a = \prod_{i=1}^{100}f(i)`: $$a = \prod_{i=1}^{100}f(i)$$

### 积分

- `\int_{i=m}^{n}`: 从m到n积分

`f(x) = \int_{x=0}^{\pi \over 2}ax^2 + bx + c`: $$f(x) = \int_{x=0}^{\pi \over 2}ax^2 + bx + c$$

### 向量

- `\vec a`: a向量
- `\overrightarrow{AB}`: A到B的向量

`\vec a = \vec b + \overrightarrow{c} + \overrightarrow{AB} + \vec{CD}`: $$\vec a = \vec b + \overrightarrow{c} + \overrightarrow{AB} + \vec{CD}$$

### 省略号

- `\cdots`

`\sum_{i=1}^{n}C_i = C_1 + C_2 + C_3 + \cdots + C_n`: $$\sum_{i=1}^{n}C_i = C_1 + C_2 + C_3 + \cdots + C_n$$

### 向下大括号

- `\underbrace{m}`
- `\underbrace{m}_{n}`
- `\underbrace{m}_{n}_{p}..._{q}`: 可以一直往下加


`\underbrace{a+b+\cdots+z}`: $$\underbrace{a+b+\cdots+z}$$

`\underbrace{a+b+\cdots+z}_{26}`: $$\underbrace{a+b+\cdots+z}_{26}$$

`\underbrace{a+b+\cdots+z}_{26}_{27}_{28}_\cdots_n`: $$\underbrace{a+b+\cdots+z}_{26}_{27}_{28}_\cdots_n$$


### 横杠

- `\overline{m+n}`: m+n公式上面加上横杠
- `\underline{m+n}`: m+n公式下面加上横杠

`\overline{m+n}`: $$\overline{m+n}$$

`\underline{m+n}`: $$\underline{m+n}$$

最后附上一个常用LaTeX公式和符号的链接：[LaTeX公式和符号](http://mohu.org/info/symbols/symbols.htm)

