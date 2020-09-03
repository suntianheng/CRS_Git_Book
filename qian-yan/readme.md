# 前言

## 介绍

**README为说明文档,SUMMARY为章节目录,一般不需要修改.**

**记录问题使用下面格式:**

| Tikcet | \*此处填写ticket号\(没有不填\) |
| :--- | :--- |
| 问题描述 | 中庚集团的318,档案绑定了联系人,上行报错，Failed insert relation in CRS,pms\_id is not in cust\_contact，手动推送联系人的profile，也报错：Failed update cust\_contact in CRS,f\_account is not in cust\_info |
| 具体原因和解决方案 | 原因是此时程序中单独上行档案允许,单独上行联系人不允许,档案+联系人也不允许\(应该是允许的\),当上行档案+联系人时会先去查询pms\_id,此时上行的消息体数据并未存储到数据库中,查不到相关信息就会报出此问题,需要修改程序代码取用消息体中的数据. |
| 编写人 | carl.sun |
| 时间 | 2020/9/3 |

## GitBook 命令

**这里将介绍GitBook的一些命令**

### 输出gitbook的帮助信息

```cpp
gitbook --help
```

### 生成静态网页并运行服务器

```cpp
gitbook serve
```

### 生成静态网页

```cpp
gitbook build
```

### 生成静态网页时指定gitbook的版本，如果本地没有将先下载

```cpp
gitbook build --gitbook=3.2.3
```

### 列出所有的gitbook版本

```cpp
gitbook ls
```

### 列出远程可用的gitbook版本

```cpp
gitbook ls-remote
```

### 更新到gitbook的最新版本

```cpp
gitbook update
```

### 卸载对应的gitbook版本

```cpp
gitbook uninstall 3.2.3
```

### 安装依赖

```cpp
gitbook install
```

### 指定log的级别

```cpp
gitbook build --log=debug
```

### 输出错误信息

```cpp
gitbook builid --debug
```

