---
layout: page
title: Cisco Router 1941 Licensing
permalink: /devices/cisco/routers/1941/cisco-licensing/
---

# Cisco Router 1941 Licensing

<pre>


Мне подсказали, что я могу активировать дополнительный функционал на определенное время.


    cisco-router-1941#show license
    Index 1 Feature: ipbasek9
    	Period left: Life time
    	License Type: Permanent
    	License State: Active, In Use
    	License Count: Non-Counted
    	License Priority: Medium
    Index 2 Feature: securityk9
    	Period left: Not Activated
    	Period Used: 0  minute  0  second  
    	License Type: EvalRightToUse
    	License State: Not in Use, EULA not accepted
    	License Count: Non-Counted
    	License Priority: None
    Index 3 Feature: datak9
    	Period left: Not Activated
    	Period Used: 0  minute  0  second  
    	License Type: EvalRightToUse
    	License State: Not in Use, EULA not accepted
    	License Count: Non-Counted
    	License Priority: None
    Index 4 Feature: SSL_VPN
    	Period left: Not Activated
            Period Used: 0  minute  0  second  
    	License Type: EvalRightToUse
            License State: Not in Use, EULA not accepted
    	License Count: 0/0  (In-use/Violation)
            License Priority: None
    Index 5 Feature: ios-ips-update
            Period left: Not Activated
    	Period Used: 0  minute  0  second  
            License Type: EvalRightToUse
    	License State: Not in Use, EULA not accepted
            License Count: Non-Counted
    	License Priority: None
    Index 6 Feature: WAAS_Express
    	Period left: Not Activated
            Period Used: 0  minute  0  second  
    	License Type: EvalRightToUse
            License State: Not in Use, EULA not accepted
    	License Count: Non-Counted
            License Priority: None

<br/>

    cisco-router-1941#sh ver



    Technology Package License Information for Module:'c1900'

    ------------------------------------------------------------------------
    Technology    Technology-package                  Technology-package
                  Current              Type           Next reboot  
    ------------------------------------------------------------------------
    ipbase        ipbasek9             Permanent      ipbasek9
    security      None                 None           None
    data          None                 None           None
    NtwkEss       None                 None           None

    Configuration register is 0x2102


<br/>

    cisco-router-1941#conf t
    cisco-router-1941(config)# license boot module c1900 technology-package ?
      NtwkEssSuitek9  NtwkEssSuitek9 technology
      datak9          data technology
      securityk9      security technology


На сайте anticisco написано:

Лицензия Right to use, т.е. на доверии. Вы должны купить лицензию, но активировать вы её не сможете. После 60 дней и после перезагрузки всё будет работать независимо от того, купили вы лицензию или нет.



    cisco-router-1941(config)#license boot module c1900 technology-package datak9



    PLEASE  READ THE  FOLLOWING TERMS  CAREFULLY. INSTALLING THE LICENSE OR
    LICENSE  KEY  PROVIDED FOR  ANY CISCO  PRODUCT  FEATURE  OR  USING SUCH
    PRODUCT  FEATURE  CONSTITUTES  YOUR  FULL ACCEPTANCE  OF  THE FOLLOWING
    TERMS. YOU MUST NOT PROCEED FURTHER IF YOU ARE NOT WILLING TO  BE BOUND
    BY ALL THE TERMS SET FORTH HEREIN.

    Use of this product feature requires  an additional license from Cisco,
    together with an additional  payment.  You may use this product feature
    on an evaluation basis, without payment to Cisco, for 60 days. Your use
    of the  product,  including  during the 60 day  evaluation  period,  is
    subject to the Cisco end user license agreement
    http://www.cisco.com/en/US/docs/general/warranty/English/EU1KEN_.html
    If you use the product feature beyond the 60 day evaluation period, you
    must submit the appropriate payment to Cisco for the license. After the
    60 day  evaluation  period,  your  use of the  product  feature will be
    governed  solely by the Cisco  end user license agreement (link above),
    together  with any supplements  relating to such product  feature.  The
    above  applies  even if the evaluation  license  is  not  automatically
    terminated  and you do  not receive any notice of the expiration of the
    evaluation  period.  It is your  responsibility  to  determine when the
    evaluation  period is complete and you are required to make  payment to
    Cisco for your use of the product feature beyond the evaluation period.

    Your  acceptance  of  this agreement  for the software  features on one
    product  shall be deemed  your  acceptance  with  respect  to all  such
    software  on all Cisco  products  you purchase  which includes the same
    software.  (The foregoing  notwithstanding, you must purchase a license
    for each software  feature you use past the 60 days evaluation  period,
    so  that  if you enable a software  feature on  1000  devices, you must
    purchase 1000 licenses for use past  the 60 day evaluation period.)

    Activation  of the  software command line interface will be evidence of
    your acceptance of this agreement.


    ACCEPT? [yes/no]: y
    % use 'write' command to make license boot config take effect on next boot


<br/>

    cisco-router-1941(config)#exit
    cisco-router-1941#write
    Building configuration...
    [OK]

<br/>

    cisco-router-1941#show license
    Index 1 Feature: ipbasek9
    	Period left: Life time
    	License Type: Permanent
    	License State: Active, In Use
    	License Count: Non-Counted
    	License Priority: Medium
    Index 2 Feature: securityk9
    	Period left: 8  weeks 4  days
    	License Type: EvalRightToUse
    	License State: Active, Not in Use, EULA not accepted
    	License Count: Non-Counted
    	License Priority: None
    Index 3 Feature: datak9
    	Period left: 8  weeks 4  days
    	License Type: EvalRightToUse
    	License State: Active, Not in Use, EULA accepted
    	License Count: Non-Counted
    	License Priority: Low
    Index 4 Feature: NtwkEssSuitek9
    	Period left: 8  weeks 4  days
    	License Type: EvalRightToUse
            License State: Active, Not in Use, EULA not accepted
            License Count: Non-Counted
            License Priority: None
    Index 5 Feature: ios-ips-update
            Period left: 8  weeks 4  days
            License Type: EvalRightToUse
            License State: Active, Not in Use, EULA not accepted
            License Count: Non-Counted
            License Priority: None
    Index 6 Feature: hseck9
    Index 7 Feature: mgmt-plug-and-play
    Index 8 Feature: mgmt-lifecycle
    Index 9 Feature: mgmt-assurance
    Index 10 Feature: mgmt-onplus
    Index 11 Feature: mgmt-compliance


<br/>

    cisco-router-1941#reload

<br/>


    cisco-router-1941# conf t



// Появился pseudowire-class. Теперь можем попробовать настроить l2tp

    cisco-router-1941(config)#p?
    parameter-map     parser   password   performance
    pfr               pfr-map  pnp        policy-map
    ppp               printer  privilege  process
    process-max-time  profile  prompt     pseudowire-class



<!--

 license boot module c3900 technology-package uck9


erase startup, перегрузил


Попробовать

c2900-universalk9-mz.SPA.151-3.T.bin - да
c2900-universalk9-mz.SPA.152-2.T.bin - нет




 у меня получилось только на -

universalk9-mz.151-3.T1.bin - да
universalk9-mz.151-3.T3.bin - нет
universalk9-mz.151-3.T1.bin - нет

-->


http://www.anticisco.ru/forum/viewtopic.php?f=2&t=1615&start=20
http://jesterantik.blogspot.ru/2013/06/trial-advipservices-advsecurity-cisco.html

</pre>
