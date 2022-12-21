# trim

## 功能

将参数 `expr` 中从左侧和右侧开始部分连续出现的空格去掉

## 语法

```Haskell
trim(expr);
```

## 参数说明

`expr`: 支持的数据类型为 VARCHAR

## 返回值说明

返回值的数据类型为 VARCHAR

## 示例

去除字符串a左侧和c右侧共5个空格。

```Plain Text
MySQL > SELECT trim("   ab c  ");
+-------------------+
| trim('   ab c  ') |
+-------------------+
| ab c              |
+-------------------+
1 row in set (0.00 sec)
```
