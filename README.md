# SimpleDistributed-Spider
闲来更新一下github。
教大家，如何简单的几个parser. 就可以去做一些分布式的爬虫任务。 主要实用场景是爬取 “树状分叉”的大型网站，

### 实用场景 
- 现在很多大型的网站的结构 一般是先分几个大类，然后每个大类底下再会有些更加细分的类别, 这就是我所谓的 "树状分叉结构"

- 比如我们要爬取，赶集网全国所有的二手车信息。我们首先爬到每个城市的链接并缓存起来，然后从缓存的城市链接里去——> 爬去剖析出所有的页面链接并缓存——>
   然后从每个页面中读取单个信息的详情页链接并缓存  ——>然后就是解析详情页
 
-  这个思路具有很大的普遍性，我去年就用这个方法 爬过很多的大型网站，房天下全国租房信息，58全国的贷款信息。QQ游戏论坛留言等等，，


### 反爬
- 如果要爬取 大量的信息，势必会遭遇一些反爬虫的阻吉 。一般能通过更换ip的方法来跳过。

- 里我们再*download.py*中的**Downloader**中 留了一个 result_testing（对返回的结果进行检测，判断是否遭遇了反爬） 和change_proxy（获取新ip) 的接口。
可以根据自己的任务 去继承Downloader 改写这两个方法。

### 健壮性
- *downloader.py*中的Downloader类。 内置了 爬取重试次数和爬取间隔的设置
- *keep.py* 监控进程，可以做到。进程挂掉后自启。
### 排程
- 我们在myspider里。将每一次的分层 都用一个类来表达。（直接继承 NodeProcess 就好了）.写好自己的parser 。
- 然后实例化第一个类 startNode . ，在根据排程的顺序写一个列表layer_obj_list 就Ok了
- run.py 本身已经做好了排程
### 署和运行
- 首先在主服务器上安装redis ，用于做缓存队列使用
- 将整个文件夹放到不到的服务器上
- 服务器上先运行initialize.py 初始化整个任务，然后在不同服务器直接运行keep.py

### 代理ip
- 等我的下篇 ，我会传一个代理ip池搭建的方法。然后轻松愉快的通过rest 调取最新被确认过可以用的ip
--- 
 大家只要写 每个分层的parse类 就可以了 即自己编辑my_spider.py 和my_config.py 就好了。 
