---
layout: page
title: Ubuntu Unable to locate package
permalink: /linux/desktops/ubuntu/install-package-for-missing-libraries/
---

# Ubuntu Unable to locate package

E: Unable to locate package libstdc++.so.6 (FOR example)


	$ sudo apt-get install apt-file
	$ sudo apt-file update
	$ sudo apt-file find libstdc++.so.6
	$ sudo apt-get install libstdc++6

Подробнее:  
http://askubuntu.com/questions/409821/install-package-for-missing-libraries


В RedHat дистрибутивах, можно поискать на rpmfind.net
