
### Php MyAdmin показывает белый экран, без какого либо кода.


    # cat /dev/null > /var/log/httpd/error_log

    # less /var/log/httpd/error_log

    [Sat May 31 15:31:03 2014] [error] [client 192.168.1.6] PHP Fatal error:  Call to undefined function mb_detect_encoding() in /var/www/html/phpmyadmin/libraries/php-gettext/gettext.inc on line 177


    PHP Fatal error:  Call to undefined function mb_detect_encoding()



    # yum install -y php-mbstring

    # service httpd restart


Заработало!


Понадобилось инсталлировать mysqli

    yum install php-mysqli



### The mcrypt extension is missing. Please check your PHP configuration.


    For me the answer was:

    1) Get the Repos from

    wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

    sudo rpm -Uvh epel-release-6*.rpm



    2) Install it via:

    sudo yum update
    sudo yum install -y php-mcrypt*

<!--
3) Edit the mcrypt.ini

cp /etc/php.d/mcrypt.ini /etc/php.d/mcrypt.ini.orig


vi /etc/php.d/mcrypt.ini

Нужно добавить:
extension=/usr/lib64/php/modules/mcrypt.so

sudo service httpd restart

-->


### date_default_timezone_get(): It is not safe to rely on the system's timezone settings.


    # php -i

**********

date

date/time support => enabled
"Olson" Timezone Database Version => 0.system
Timezone Database => internal
PHP Warning:  Unknown: It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected 'Europe/Moscow' for 'MSK/4.0/no DST' instead in Unknown on line 0
Default timezone => Europe/Moscow

Directive => Local Value => Master Value
date.default_latitude => 31.7667 => 31.7667
date.default_longitude => 35.2333 => 35.2333
date.sunrise_zenith => 90.583333 => 90.583333
date.sunset_zenith => 90.583333 => 90.583333
date.timezone => no value => no value

**********

# cp /etc/php.ini /etc/php.ini.orig
# vi /etc/php.ini

date.timezone = Europe/Moscow



<!--
	wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
	sudo rpm -Uvh remi-release-6*.rpm
-->


</pre>





</div>
<?php include_once "../../../../../../_footer.php"?>

</body>

</html>
