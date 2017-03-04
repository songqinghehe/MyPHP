>安装composer

官方给出的命令
curl -sS https://getcomposer.org/installer | php （此命令我执行失败了）
php -r "readfile('https://getcomposer.org/installer');" | php  （此命令第一次没执行成功，重试了一次执行成功）
mv composer.phar /usr/local/bin/composer
composer --version
完毕！

>安装laravel--一键安装包方式

前提需要有php的扩展:openssl/pdo/mbstring/Tokenizer
/tmp
wget http://down.golaravel.com/laravel/laravel-v5.2.15.zip
unzip laravel-v5.2.15.zip
mv laravel-v5.2.15.zip laravel
cp -rf laravel /usr/local/nginx/html
cd /usr/local/nginx/html
chmod -R 777 laravel
浏览器输入：http://192.168.31.69/laravel/public/
报错：403
原因：nginx需要配置一个知道此目录下的动作
vi /usr/local/nginx/conf/nginx.conf
修改前
location / {
    root   html;
    index  index.html index.htm index.php;
}
修改后
location / {
    root   html;
    index  index.html index.htm index.php;
    try_files $uri $uri/ /index.php?$query_string;
}
重启nginx
/usr/local/nginx/sbin/nginx -s reload
浏览器输入：http://192.168.31.69/laravel/public/
完毕！
