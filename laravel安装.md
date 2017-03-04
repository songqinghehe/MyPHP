>安装composer

官方给出的命令</br>
curl -sS https://getcomposer.org/installer | php （此命令我执行失败了）</br>
php -r "readfile('https://getcomposer.org/installer');" | php  （此命令第一次没执行成功，重试了一次执行成功）</br>
mv composer.phar /usr/local/bin/composer</br>
composer --version</br>
完毕！</br>

>安装laravel--一键安装包方式

前提需要有php的扩展:openssl/pdo/mbstring/Tokenizer</br>
/tmp</br>
wget http://down.golaravel.com/laravel/laravel-v5.2.15.zip</br>
unzip laravel-v5.2.15.zip</br>
mv laravel-v5.2.15.zip laravel</br>
cp -rf laravel /usr/local/nginx/html</br>
cd /usr/local/nginx/html</br>
浏览器输入：http://192.168.31.69/laravel/public/</br>
报错：403</br>
原因：禁止访问</br>
chmod -R 777 laravel</br>
浏览器输入：http://192.168.31.69/laravel/public/</br>
报错：500</br>
原因：nginx需要配置一个知道此目录下的动作</br>
vi /usr/local/nginx/conf/nginx.conf</br>
修改前</br>
location / {</br>
    root   html;</br>
    index  index.html index.htm index.php;</br>
}</br>
修改后</br>
location / {</br>
    root   html;</br>
    index  index.html index.htm index.php;</br>
    try_files $uri $uri/ /index.php?$query_string;</br>
}</br>
重启nginx</br>
/usr/local/nginx/sbin/nginx -s reload</br>
浏览器输入：http://192.168.31.69/laravel/public/</br>
完毕！</br>
