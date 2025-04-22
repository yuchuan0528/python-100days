# 01：初识Python
Python相关介绍与安装，已完成

---

# 02: 第一个Python程序
![image](https://github.com/user-attachments/assets/14a7ddc0-63df-4b61-a147-648129cf66a6)

---

# 03：Python语言中的变量
1. 计算机的硬件系统：
   
  运算器、控制器（合称CPU，执行运算控制指令）
  存储器（分为内部存储器/内存、外部存储器）
  输入、输出设备（I/O设备）

2. Python数据类型

  (1) int
   
```
    print(0b100)  # 二进制整数，b
    print(0o100)  # 八进制整数，o
    print(100)    # 十进制整数
    print(0x100)  # 十六进制整数，x
```
  (2) float 支持科学计数法
```
print(123.456)    # 数学写法
print(1.23456e2)  # 科学计数法
print(1.23456e-2)  # 科学计数法
```
  (3)str, bool

3. 类型转换
```
print(int('120',base=10)) # 转为10进制（默认）
print(int('123',base=16)) # 转为16进制
print(int(True)) # True->1, False->0
print(bool("hello")) # 字符串非空 -> 
```

---

# 04：Python语言中的运算符
1. 计算运算符
   
```
print(3**2) # 幂
print(302 // 10) # 整除
print(302 % 10) # 取余
```

2. 比较、逻辑运算符

```
print((1 == 1) and (2 != 2))
print((1 == 1) or (2 != 2))
print(not(2 != 3))
```

3. 格式化输出
```
# 方式1
f = float(input('请输入华氏温度: '))
c = (f - 32) / 1.8
print('%.1f华氏度 = %.1f摄氏度' % (f, c)) # %.1f保留一位小数的占位符
print('%d华氏度 = %d摄氏度' % (int(f), int(c))) # %d整数，%s字符型
# 方式2
print(f'{int(f):d}华氏度 = {c:.2f}摄氏度')
```

---

# 05：分支结构
1. if else
```
# BMI判断例子
height = float(input("请输入您的身高(m)"))
weight = float(input("请输入您的体重(kg)"))
BMI = weight/(height)**2

if BMI >= 27:
    print("肥胖")
elif BMI >= 24:
    print("过重")
elif BMI > 18.5:
    print("正常")
else:
    print("体重过轻")
```

2. match case
```
match expression: # 表达式
  case pattern1: # 值，变量等

  case pattern2:

  case _:其他情况
```

# 06：循环结构

1. for in 结构

注意，`range(a,b)`左闭右开，`range(a,b,c)`从a到b步长为c，右开




   

