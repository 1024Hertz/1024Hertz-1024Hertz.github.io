---
title: Mybatis Generator配置详解
tags:
  - Mybatis
  - Mybatis Generator
  - 生成器
---

# columnRenamingRule
> 该元素会在根据表中列名计算对象属性名之前先重命名列名，非常适合用于表中的列都有公用的前缀字符串的时候  
> 如列名为：SYS_ID、SYS_NAME等,可以设置searchString为"^SYS_", 并使用空白替换  
> 那么生成的SYS对象中的属性名称就不是sysId,sysName等，而是先被替换为ID,NAME, 然后变成属性：id，name，email

## **注意**
* 如果使用了columnOverride元素，该属性无效

```xml
<columnRenamingRule searchString="" replaceString=""/>
```

# columnOverride
> 用来修改表中某个列的属性，MBG会使用修改后的列来生成domain的属性  
> column:要重新设置的列名
> 一个table元素中可以有多个columnOverride元素

```xml
<columnOverride column="username">
    <!-- 使用property属性来指定列要生成的属性名称 -->
    <property name="property" value="userName"/>
    <!-- javaType用于指定生成的domain的属性类型，使用类型的全限定名 -->
    <property name="javaType" value=""/>
    <!-- jdbcType用于指定该列的JDBC类型 -->
    <property name="jdbcType" value=""/>
</columnOverride>
```

# ignoreColumn
> ignoreColumn设置一个MGB忽略的列，如果设置了改列，那么在生成的domain中，生成的SQL中，都不会有该列出现  
> column:指定要忽略的列的名字  
> delimitedColumnName：参考table元素的delimitAllColumns配置，默认为false  
> 一个table元素中可以有多个ignoreColumn元素
```xml
<ignoreColumn column="deptId" delimitedColumnName=""/>
```