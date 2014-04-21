php-mmseg
=========

## 简介

中文分词引擎LibMMSeg的php扩展

## 依赖

1. php5.4 以上版本 （其他版本目前还没有测试）
2. LibMMSeg 3.2.14版本 http://www.coreseek.cn/opensource/mmseg/
```
    关于mmseg的安装，参考 http://www.coreseek.cn/products/products-install/
```

## 安装

1. 首先安装LibMMSeg
```
cd coreseek-4.1-beta/mmseg-3.2.14
./configure --prefix=/opt/
make 
sudo make install
```
2. 安装php-mmseg
```
cd php-mmseg/mmseg
phpize
./configure --with-mmseg=/opt
make
sudo make install
```
3. 配置php-mmseg

    在php.ini中增加
    ```
    extension=mmseg.so
    mmseg.dict_dir=/opt/etc
    
    ```
    注意， mmseg.dict_dir 配置的是mmseg配置文件和字典所在目录的地址

## 使用

1. 全局字典模式

    该模式引入全文的字典文件，不需要每次调用分词代码之前调用字典，但是要求必须在配置文件中配置好字典的目录
    ```php
    $ret = mmseg_segment("你好，世界");
    var_dump($ret);
    ```
2. 在程序中引入字典
    
    ```php
    $mmseg = mmseg_open("/opt/mmseg/etc");
    $ret = mmseg_segment($mmseg, "你好，世界");
    mmseg_close($mmseg);
    var_dump($ret);
    ```

输出结果（使用mmseg默认的字典）
```php
array(3) {
  [0]=>
  string(6) "你好"
  [1]=>
  string(3) "，"
  [2]=>
  string(6) "世界"
}
```
3. 生成字典文件

类似命令行的如下命令，
```
mmseg -u unigram.txt
```

我们可以使用下面的语句生成词典文件，其中第一个参数为要输入的源文本格式的词典文件， 如unigram.txt , 第二个参数为输出的供分词使用的词典文件

代码
```php
$isDone = mmseg_gendict(dirname(__FILE__) . "/" . "unigram.txt", dirname(__FILE__) . "/" . "unigram.txt.uni" );

```

原文本格式的词典文件格式如下（参考LibMMSeg文档 : http://www.coreseek.cn/opensource/mmseg/ ）
```
河 187
x:187
造假者 1
x:1
台北队 1
x:1
湖边 1
```


## 更多
本扩展由小声团队开发 http://xiaosheng.fm

