---
layout: page
title: Cisco Router 1941 проблемы с подключением к сети Билайн по l2tp
permalink: /devices/cisco/routers/1941/beeline-l2tp-first-problem/
---

# Cisco Router 1941 проблемы с подключением к сети Билайн по l2tp


<br/>


<div align="center" style="border-width: 4px; padding: 10px; border-style: inset; border-color: red; ">

Многое поменялось. Теперь не нуно настраивать l2tp. Смотри подробнее <a href="/devices/cisco/routers/1941/beeline/">здесь</a>

</div>

<br/>


<pre>


cisco-router-1941>en
cisco-router-1941#conf t

================

cisco-router-1941#interface Virtual-PPP1
                   ^
% Invalid input detected at '^' marker.

================

cisco-router-1941(config)#l2tp-class class-1
cisco-router-1941(config-l2tp-class)#pseudowire-class class1
                                      ^
% Invalid input detected at '^' marker.

cisco-router-1941(config-l2tp-class)#encapsulation l2tpv2
                                      ^
% Invalid input detected at '^' marker.

cisco-router-1941(config-l2tp-class)#protocol l2tpv2 corbina
                                      ^
% Invalid input detected at '^' marker.



В общем таких команд нет!

Нашел в интернете информацию,
https://supportforums.cisco.com/thread/2036850

что для моего роутера, необходима лицензия "data license"

</pre>


Далее делал, как <a href="/devices/cisco/routers/1941/cisco-licensing/">здесь</a>
