# Python读写文件

## Python读写CSV文件
CSV（Comma Separated Values）全称逗号分隔值文件是一种简单、通用的文件格式，被广泛的应用于应用程序（数据库、电子表格等）数据的导入和导出以及异构系统之间的数据交换。
因为CSV是纯文本文件，不管是什么操作系统和编程语言都是可以处理纯文本的，而且很多编程语言中都提供了对读写CSV文件的支持，因此CSV格式在数据处理和数据科学中被广泛应用。

**写入csv文件**
1. pandas包：`pd.to_csv()`
2. 自带的`csv`模块，通过`.writer`函数返回`csvwriter`对象，然后通过该对象的`writerow`或者`writerows`将数据写入csv中。例如：

```python
import csv
import random

with open('scores.csv', 'w') as file:
    writer = csv.writer(file) # 返回csvwriter对象
    writer.writerow(['姓名', '语文', '数学', '英语'])
    names = ['关羽', '张飞', '赵云', '马超', '黄忠']
    for name in names:
        scores = [random.randrange(50, 101) for _ in range(3)]
        scores.insert(0, name)
        writer.writerow(scores)
```

**读入csv文件**
1. pandas包：`pd.read_csv()`
2. 自带的`csv`模块，通过`.reader`函数返回`csvreader`对象（是一个迭代器），然后通过`next`或者`for-in`循环读取。


## 读写Excel文件
如果要兼容 Excel 2007 以前的版本，也就是xls格式的 Excel 文件，可以使用三方库`xlrd`和`xlwt`，前者用于读 Excel 文件，后者用于写 Excel 文件。
如果使用较新版本的 Excel，即xlsx格式的 Excel 文件，可以使用 `openpyxl`库，当然这个库不仅仅可以操作Excel，还可以操作其他基于 Office Open XML 的电子表格文件。

1. 基于xlrd读取 Excel 文件

```python
import xlrd

# 使用xlrd模块的open_workbook函数打开指定Excel文件并获得Book对象（工作簿）
wb = xlrd.open_workbook('阿里巴巴2020年股票数据.xls')

sheetnames = wb.sheet_names() # 通过Book对象的sheet_names方法可以获取所有表单名称
print(sheetnames)

sheet = wb.sheet_by_name(sheetnames[0]) # 通过sheet_by_name,获得指定的表单名称获取Sheet对象（工作表）

print(sheet.nrows, sheet.ncols) # 通过Sheet对象的nrows和ncols属性获取表单的行数和列数
for row in range(sheet.nrows):
    for col in range(sheet.ncols):
        # 通过Sheet对象的cell方法获取指定Cell对象（单元格）
        # 通过Cell对象的value属性获取单元格中的值
        value = sheet.cell(row, col).value
        # 对除首行外的其他行进行数据格式化处理
        if row > 0:
            # 第1列的xldate类型先转成元组再格式化为“年月日”的格式
            if col == 0:
                # xlrd.xldate_as_tuple(xldate,datemode) ，将时间返回为元组，函数的第二个参数只有0和1两个取值
                # 其中0代表以1900-01-01为基准的日期，1代表以1904-01-01为基准的日期
                value = xlrd.xldate_as_tuple(value, 0)
                value = f'{value[0]}年{value[1]:>02d}月{value[2]:>02d}日'
            # 其他列的number类型处理成小数点后保留两位有效数字的浮点数
            else:
                value = f'{value:.2f}'
        print(value, end='\t')
    print()
# 获取最后一个单元格的数据类型
# 0 - 空值，1 - 字符串，2 - 数字，3 - 日期，4 - 布尔，5 - 错误
last_cell_type = sheet.cell_type(sheet.nrows - 1, sheet.ncols - 1) # sheet.cell_type获取数据类型
print(last_cell_type)
# 获取第一行的值（列表）
print(sheet.row_values(0))
# 获取指定行指定列范围的数据（列表）
# 第一个参数代表行索引，第二个和第三个参数代表列的开始（含）和结束（不含）索引
print(sheet.row_slice(3, 0, 5))
```   

2. 基于`xlwt`写excel文件。

通过`xlwt`模块的`Workbook`类创建工作簿对象，通过工作簿对象的`add_sheet`方法添加工作表，通过工作表对象的`write`方法向其中写入数据，然后通过工作簿对象的`save`方法写入指定文件。

```python
import random

import xlwt

student_names = ['关羽', '张飞', '赵云', '马超', '黄忠']
scores = [[random.randrange(50, 101) for _ in range(3)] for _ in range(5)]

wb = xlwt.Workbook() # 创建工作簿对象（Workbook）

sheet = wb.add_sheet('一年级二班') # 创建工作表对象（Worksheet）
titles = ('姓名', '语文', '数学', '英语')
for index, title in enumerate(titles):
    sheet.write(0, index, title) #  添加表头数据

for row in range(len(scores)):
    sheet.write(row + 1, 0, student_names[row]) # 将学生姓名和考试成绩写入单元格
    for col in range(len(scores[row])):
        sheet.write(row + 1, col + 1, scores[row][col])
# 保存Excel工作簿
wb.save('考试成绩表.xls')
```

在通过`write`方法写入时，还可以写入公式：

```python
sheet.write(nrows, ncols, xlwt.Formula(f'sum(G2:G{nrows})))
```

3. 基于`openpyxl`对Excel文件操作，可以进行读写。

读取Excel文件：`openpyxl.load_workbook`方法，对于获取指定位置的元素，既可以向前文方法中，使用`cell`，也可以使用切片操作。

```python
import datetime

import openpyxl

# 加载一个工作簿 ---> Workbook
wb = openpyxl.load_workbook('阿里巴巴2020年股票数据.xlsx')

print(wb.sheetnames) # 获取工作表的名字 .sheetnames

sheet = wb.worksheets[0] # 获取工作表 ---> Worksheet

print(sheet.dimensions) # 获得单元格的范围

print(sheet.max_row, sheet.max_column) # 获得行数和列数

# 获取指定单元格的值
print(sheet.cell(3, 3).value)
print(sheet['C3'].value)
print(sheet['G255'].value)

# 获取多个单元格（嵌套元组）
print(sheet['A2:C5'])

# 读取所有单元格的数据
for row_ch in range(2, sheet.max_row + 1):
    for col_ch in 'ABCDEFG':
        value = sheet[f'{col_ch}{row_ch}'].value
        if type(value) == datetime.datetime:
            print(value.strftime('%Y年%m月%d日'), end='\t')
        elif type(value) == int:
            print(f'{value:<10d}', end='\t')
        elif type(value) == float:
            print(f'{value:.4f}', end='\t')
        else:
            print(value, end='\t')
    print()
    
```

除此之外还可以写入、绘制图表等
