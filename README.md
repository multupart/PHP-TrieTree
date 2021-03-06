# PHP-TrieTree
## 这是一个PHP的字典树

- v1.0
	- 命中一个后返回
- v2.0
	- 支持命中多个返回
	- 支持在树梢增加自定义数组 [替换内容] 
	- 性能提升10倍
- v3.0
    - 增加删除特性
        - 删除整棵关键词树
    - 解决命中不全BUG
    - 3.1
        - 增加词频统计

## 注意
- 在即时场景中（即时更新关键词），如果关键词数量较大，到十万甚至百万级别，尽量不要使用CGI模式，首次加载需要较大的性能开销，多个进程同时使用会造成一定的内存浪费，整体性能会下降，会拖垮web服务。这种情况下建议使用swoole单独封装服务，目前十万级别的关键词，已经在生产环境中验证过并运行良好。
- 要严格控制关键词深度，关键词不宜过长，汉字的话最好10个汉字以内。
- 在非即时场景中可以使用计划任务、常驻脚本等方式对内容进行处理。

## 示例
```php
<?php
require "../src/TrieTree.php";


$testArr = array("张三","张四","王五","张大宝","张三四","张氏家族","王二麻子");

$tree = new \AbelZhou\Tree\TrieTree();

foreach ($testArr as $str){
    $tree->append($str);
}

$res = $tree->getTree();

var_dump($res);

$res = $tree->search("有一个叫张三的哥们");
var_dump($res);

$res = $tree->search("我叫李四喜");
var_dump($res);

//删除
$res = $tree->delete("张三");
//删除整棵树 连带“张三”和张三下的“张三四”一并删除
$tree->delete("张三",true);
```

## 使用场景
- 敏感词过滤
- 内链建设

## 性能
test目录下有个1.5w左右的敏感词。
mac下检索耗时2~5毫秒左右
这些敏感词来自网络，不是很全。

## composer安装
```
composer require abelzhou/php-trie-tree
```

