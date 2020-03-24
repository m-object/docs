## 前言：

随着网站的内容的增多和用户访问量的增多，无可避免的是网站加载会越来越慢，受限于带宽和服务器同一时间的请求次数的限制，我们往往需要在此时对我们的网站进行代码优化和服务器配置的优化。

一般情况下会从以下方面来做优化

- 动态页面静态化
- 优化数据库
- 使用负载均衡
- 使用缓存
- 使用CDN加速

现在很多网站在建设的时候都要进行静态化的处理，为什么网站要进行静态化处理呢？我们都知道纯静态网站是所有的网页都是独立的一个html页面，当我们访问的时候不需要经过数据的处理直接就能读取到文件，访问速度就可想而知了，而其对于搜索引擎而言也是非常友好的一个方式。

- <strong>纯静态网站在网站中是怎么实现的？</strong>

纯静态的制作技术是需要先把网站的页面总结出来，分为多少个样式，然后把这些页面做成模板，生成的时候需要先读取源文件然后生成独立的以.html结尾的页面文件，所以说纯静态网站需要更大的空间，不过其实需要的空间也不会大多少的，尤其是对于中小型企业网站来说，从技术上来讲，大型网站想要全站实现纯静态化是比较困难的，生成的时间也太过于长了。不过中小型网站还是做成纯静态的比较，这样做的优点是很多的。

- <strong>而动态网站又是怎么进行静态处理的？</strong>

页面静态化是指将动态页面变成html/htm静态页面。动态页面一般由asp,php,jsp,.net等程序语言编写而成，非常便于管理。但是访问网页时还需要程序先处理一遍，所以导致访问速度相对较慢。而静态页面访问速度快，却又不便于管理。那么动态页面静态化即可以将两种页面的好处集中到一起。


- <strong>静态处理后又给网站带来了哪些好处？</strong>

    - 静态页面相对于动态页面更容易被搜索引擎收录。
    - 访问静态页面不需要经过程序处理，因此可以提高运行速度。
    - 减轻服务器负担。
    - HTML页面不会受Asp相关漏洞的影响。
    
静态处理后的网站相对没有静态化处理的网站来讲还比较有安全性，因为静态网站是不会是黑客攻击的首选对象，因为黑客在不知道你后台系统的情况下，黑 客从前台的静态页面很难进行攻击。同时还具有一定的稳定性，比如数据库或者网站的程序出了问题，他不会干扰到静态处理后的页面，不会因为程序或数据影响而 打不开页面。

搜索引擎蜘蛛程序更喜欢这样的网址，也可以减轻蜘蛛程序的工作负担，虽然有的人会认为现在搜索引擎完全有能力去抓取和识别动态的网址，在这里还是建议大家能做成静态的尽量做成静态网址。

下面我们主要来讲一讲页面静态化这个概念，希望对你有所帮助！

### 什么是HTML静态化:

![什么是HTML静态化](http://i1.wp.com/justcode.ikeepstudying.com/wp-content/uploads/2015/10/20150602132938.png?resize=700%2C376)

常说的页面静态化分为两种，一种是伪静态，即url 重写，一种是真静态化。
在PHP网站开发中为了网站推广和SEO等需要，需要对网站进行全站或局部静态化处理，PHP生成静态HTML页面有多种方法，比如利用PHP模板、缓存等实现页面静态化。

PHP静态化的简单理解就是使网站生成页面以静态HTML的形式展现在访客面前，PHP静态化分纯静态化和伪静态化，两者的区别在于PHP生成静态页面的处理机制不同。

PHP伪静态：利用Apache mod_rewrite实现URL重写的方法。

### HTML静态化的好处:

1. 减轻服务器负担，浏览网页无需调用系统数据库。
2. 有利于搜索引擎优化SEO，Baidu、Google都会优先收录静态页面，不仅被收录的快还收录的全；
3. 加快页面打开速度，静态页面无需连接数据库打开速度较动态页面有明显提高；
4. 网站更安全，HTML页面不会受php程序相关漏洞的影响；观看一下大一点的网站基本全是静态页面，而且可以减少攻击，防sql注入。数据库出错时，不影响网站正常访问。
5. 数据库出错时，不影响网站的正常访问。

最主要是可以增加访问速度,减轻服务器负担,当数据量有几万，几十万或是更多的时候你知道哪个更快了. 而且还容易被搜索引擎找到。生成html文章虽操作上麻烦些，程序上繁杂些，但为了更利于搜索，为了速度更快些，更安全，这些牺牲还是值得的。

### 实现HTML静态化的策略与实例讲解:

#### 基本方式

- `file_put_contents()`函数 
- 使用php内置缓存机制实现页面静态化 `—output-bufferring.`

    ![使用php内置缓存机制实现页面静态化](http://i1.wp.com/justcode.ikeepstudying.com/wp-content/uploads/2015/10/20150602140407.png?resize=691%2C405)
    
##### 方法1:利用PHP模板生成静态页面

PHP模板实现静态化非常方便，比如安装和使用PHP Smarty实现网站静态化。

在使用Smarty的情况下，也可以实现页面静态化。下面先简单说一下使用Smarty时通常动态读取的做法。

<strong>一般分这几步：</strong>

1. 通过URL传递一个参数(ID);
2. 然后根据此ID查询数据库;
3. 取得数据后根据需要修改显示内容;
4. assign需要显示的数据;
5. display模板文件。

<strong>Smarty静态化过程只需要在上述过程中添加两个步骤。</strong>

1. 在1之前使用`ob_start()`打开缓冲区。
2. 在5之后使用`ob_get_contents()`获取内存未输出内容，然后使用fwrite()将内容写入目标html文件。

> 根据上述描述，此过程是在网站前台实现的，而内容管理(添加、修改、删除)通常是在后台进行，为了能有效利用上述过程，可以使用一点小手段，那就是Header()。具体过程是这样的：在添加、修改程序完成之后，使用`Header()`跳到前台读取，这样可以实现页面HTML化，然后在生成html后再跳回后台管理侧，而这两个跳转过程是不可见的。

##### 方法2:使用PHP文件读写功能生成静态页面

```php
$out1 = "<html><head><title>PHP网站静态化教程</title></head><body>欢迎访问PHP网站开发教程网www.leapsoul.cn，本文主要介绍PHP网站页面静态化的方法</body></html>";
$fp = fopen("leapsoulcn.html", "w");
if (!$fp) {
    echo "System Error";
    exit();
} else {
    fwrite($fp, $out1);
    fclose($fp);
    echo "Success";
} 
```
##### 方法三：使用PHP输出控制函数`(Output Control）/ob`缓存机制生成静态页面

输出控制函数`(Output Control)`也就是使用和控制缓存来生成静态HTML页面，也会使用到PHP文件读写函数。

比如某个商品的动态详情页地址是: `http://xxx.com?goods.php?gid=112`
那么这里我们根据这个地址读取一次这个详情页的内容，然后保存为静态页，下次有人访问这个商品详情页动态地址时，我们可以直接把已生成好的对应静态内容文件输出出来。

示例一：
```php
ob_start();
echo "<html>" .
    "<head>" .
    "<title>PHP网站静态化教程</title>" .
    "</head>" .
    "<body>欢迎访问PHP网站开发教程网www.leapsoul.cn，本文主要介绍PHP网站页面静态化的方法</body>" .
    "</html>";
$out1 = ob_get_contents();
ob_end_clean();
$fp = fopen("leapsoulcn.html", "w");
if (!$fp) {
    echo "System Error";
    exit();
} else {
    fwrite($fp, $out1);
    fclose($fp);
    echo "Success";
}
```

示例二：

```php
$gid = $_GET['gid'] + 0;//商品id
$goods_statis_file = "goods_file_" . $gid . ".html";//对应静态页文件
$expr = 3600 * 24 * 10;//静态文件有效期，十天
if (file_exists($goods_statis_file)) {
    $file_ctime = filectime($goods_statis_file);//文件创建时间
    if ($file_ctime + $expr-- > time()) {//如果没过期
        echo file_get_contents($goods_statis_file);//输出静态文件内容
        exit;
    } else {//如果已过期
        unlink($goods_statis_file);//删除过期的静态页文件
        ob_start();
        //从数据库读取数据，并赋值给相关变量
        //include ("xxx.html");//加载对应的商品详情页模板
        $content = ob_get_contents();//把详情页内容赋值给$content变量
        file_put_contents($goods_statis_file, $content);//写入内容到对应静态文件中
        ob_end_flush();//输出商品详情页信息
    }
} else {
    ob_start();
    //从数据库读取数据，并赋值给相关变量
    //include ("xxx.html");//加载对应的商品详情页模板
    $content = ob_get_contents();//把详情页内容赋值给$content变量
    file_put_contents($goods_statis_file, $content);//写入内容到对应静态文件中
    ob_end_flush();//输出商品详情页信息
}
```
我们知道使用PHP进行网站开发，一般执行结果直接输出到游览器，为了使用PHP生成静态页面，就需要使用输出控制函数控制缓存区，以便获取缓存区的内容，然后再输出到静态HTML页面文件中以实现网站静态化。

PHP生成静态页面的思路为：首先开启缓存，然后输出了HTML内容（你也可以通过include将HTML内容以文件形式包含进来），之后获取缓存中的内容，清空缓存后通过PHP文件读写函数将缓存内容写入到静态HTML页面文件中。

获得输出的缓存内容以生成静态HTML页面的过程需要使用三个函数：`ob_start()、ob_get_contents()、ob_end_clean()`。

1. `ob_start`函数一般主要是用来开启缓存，注意使用`ob_start`之前不能有任何输出，如空格、字符等。
2. `ob_get_contents`函数主要用来获取缓存中的内容以字符串形式返回，注意此函数必须在`ob_end_clean`函数之前调用，否则获取不到缓存内容。
3. `ob_end_clean`函数主要是清空缓存中的内容并关闭缓存，成功则返回True，失败则返回False
    
##### 方法四：使用nosql从内存中读取内容(其实这个已经不算静态化了而是缓存);

以memcache为例：
```php
$gid  = $_GET['gid']+0;//商品id  
$goods_statis_content = "goods_content_".$gid;//对应键  
$expr = 3600*24*10;//有效期，十天  
   
$mem = new Memcache;  
$mem->connect('memcache_host', 11211);  
   
$mem_goods_content = $mem->get($goods_statis_content);   
if($mem_goods_content){  
      echo $mem_goods_content;  
}else{  
  ob_start();  
  //从数据库读取数据，并赋值给相关变量  
  //include ("xxx.html");//加载对应的商品详情页模板  
  $content = ob_get_contents();//把详情页内容赋值给$content变量  
  $mem->add($goods_statis_content,$content, false, $expr);  
  ob_end_flush();//输出商品详情页信息  
}  
```
> memcached是键值一一对应，key默认最大不能超过128个字节，value默认大小是1M，因此1M大小满足大多数网页大小的存储。

<strong>尊重他人知识</strong>

本文转载：https://blog.csdn.net/luyaran/article/details/52556431?utm_source=app