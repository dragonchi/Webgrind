webgrind
========

xdebug profiler分析
安装xdebug
sudo apt-get install php-pear
sudo apt-get install php5-dev
sudo pecl install xdebug

下载安装编译完后，在php.ini 中加入
extension=xdebug.so
和
[zend]
zend_extension = /usr/lib/php5/20100525+lfs/xdebug.so  

[Xdebug]  
 
xdebug.auto_trace=On 
 
xdebug.collect_params=On 
 
xdebug.collect_return=On 
 
xdebug.trace_output_dir="/tmp/xdebug" 
 
xdebug.profiler_enable=On 
 
xdebug.profiler_output_dir="/tmp/xdebug" 
 
xdebug.profiler_output_name="cachegrind.out.%c" 


sudo mkdir -p /tmp/xdebug
sudo chmod 777 /tmp/xdebug

sudo mkdir -p /tmp/storage
sudo chmod 777 /tmp/storage

xdebug profiler分析工具部署
源码：http://code.google.com/p/webgrind/downloads/detail?name=webgrind-release-1.0.zip&can=2&q=
下载webgrind，放置到web服务器目录下面，然后打开config.php文件，找到如下两行，修改为正确的值：
static $storageDir = '/tmp/storage';
static $profilerDir = '/tmp/xdebug';

nginx虚拟主机配置
server{
server_name www.xdebug.com;
root /home/chidl/workspace/project/webgrind;
location / {
index index.html index.php;
}
location ~ \.php$ {
fastcgi_pass 127.0.0.1:9000;
#fastcgi_pass unix:/var/run/php5-fpm.sock;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;
}
}
