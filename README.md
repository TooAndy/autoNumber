# autoNumber

## 脚本功能
该脚本功能为
- 为markdown文档的标题生成编号（编号可自定义样式）
- 消除已有的编号

注：一次调用只能指定一种样式


## 自定义样式
自定义样式示例：
```python
classic_style = {
    'inherit': 'n',
    'heading': ['x', '*、_cn', '(*)_cn', '*.', '(*)', '*)_en']
}
```

### 样式的结构
样式为字典类型，其中有两个元素 

**inherit:**
该元素决定子标题是否继承父标题的内容，可选的值有'y'和'n'， 
当值为'y'时，可间隔一个半角逗号','指定不同等级的标题间的分隔符，若不指定，则分隔符默认为一个半角句号

**heading:** 
该元素决定每一级标题的具体样式，该元素为包含六个字符串的列表，每个字符串表示一级标题的样式

字符串可由下划线'_'分为两个部分（以字符串中最后一个下划线为界），前半部分为格式，后半部分为编号形式，若没有指定，则默认为数字编号

格式部分：
- x代表不编号（x不能出现在中间等级标题的位置），
- *为编号的占位符，该符号会被替换为编号，
- 其他字符会作为格式的一部分原样保留

编号形式部分：
- 该部分功能为指定生成编号所使用的函数
- 该函数所在作用域应为全局作用域
- 该函数应具有如下格式，其中xxx即编号规则所指定的名字
```python
def generate_xxx(num: int) -> str:
    ...
```

### 内置的编号形式

|名称|含义|
|:-|:-|
|en|英文|
|EN|大写英文|
|cn|中文数字|
|CN|大写中文数字|
|roman|罗马数字|
|hex|十六进制|
|bin|二进制|

### 指定定义在外部模块中的样式
样式的格式和编号形式均可在其他模块中指定，使用时需要指明模块名称，若样式的所有定义均在该脚本内，则直接指示该样式的名称即可

例：样式xxx部分或全部定义于模块abc中时，需要在参数中指定模块名称

```
-s abc,xxx -f c
```


## 编号消除
编号消除功能通过指定样式为'clear'进行调用，需要如下格式
```markdown
# 1 标题
```
整个标题行分为三段，脚本通过消除中间的部分，将两端拼接的方式实现编号的消除 
（若标题行仅为两段，脚本不会进行处理，若标题行多于三段，则只会删除第二段）

## 命令行指南
```
usage: autoNumber.py [-h] [-f [FILES [FILES ...]]] [-s [STYLE]]
                     [--folders [FOLDERS [FOLDERS ...]]]

optional arguments:
  -h, --help            show this help message and exit
  -f [FILES [FILES ...]], --files [FILES [FILES ...]]
                        需要编号/消除编号的文件
  -s [STYLE], --style [STYLE]
                        编号的样式，指定为"clear"即消除已有编号，不指定即使用默认样式
  --folders [FOLDERS [FOLDERS ...]]
                        包含需要编号/消除编号的文件的文件夹
```