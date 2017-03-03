>整合nginx与php7

vi /usr/local/nginx/conf/nginx.conf</br>
修改前：</br>
#location ~ \.php$ {</br>
#    root           html;</br>
#    fastcgi_pass   127.0.0.1:9000;</br>
#    fastcgi_index  index.php;</br>
#    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;</br>
#    include        fastcgi_params;</br>
#}</br>

修改后：</br>
location ~ \.php$ {</br>
    root           html;</br>
    fastcgi_pass   127.0.0.1:9000;</br>
    fastcgi_index  index.php;</br>
    fastcgi_param  SCRIPT_FILENAME  /usr/local/nginx/html$fastcgi_script_name;</br>
    include        fastcgi_params;</br>
}</br>

Nginx已经知道怎么把得到的请求传达给PHP，Nginx在得到*.php请求时，会把请求通过9000端口传给PHP</br>
那么只有Nginx自己知道咋找PHP了还不行，还需要PHP知道咋找Nginx</br>
PS：你见过大街上的JJMM约会时有不是相互认识对方，或者是不知道 用啥方法和对方接头的？</br>
这点我们不需要担心，PHP-FPM已经在配置文件中定义了从哪接受PHP请求，我们可以打开配置文件看一下</br>
vi /usr/local/php/etc/php-fpm.d/www.conf</br>
文件中会看到：</br>
listen = 127.0.0.1:9000</br>
我们在前面已经看到过Nginx是通过本机的9000端口将PHP请求转发给PHP的，</br>
上面我们可以看到PHP自己是从本机的9000端口侦听数据 ，Nginx与PHP通过本机的9000端口完成了数据请求</br>

>测试*.php是否生效

cd /usr/local/nginx/html</br>
echo "<?php phpinfo();" > index.php</br>
输入地址：http://192.168.31.69/index.php</br>
