---
layout: page
title: Cisco Router 1941 включение и выключение debugging
description: Cisco Router 1941 включение и выключение debugging
keywords: Cisco Router 1941 включение и выключение debugging
permalink: /devices/cisco/routers/1941/debugging/
---

# Cisco Router 1941 включение и выключение debugging

Возможно, для анализа проблем авторизации поможет debug

<br/>

Включить debug:

<pre>
<strong>cisco-router-1941# <code>terminal monitor </code></strong>
</pre>

<pre>
<strong>cisco-router-1941# <code>debug ppp negotiation</code></strong>
</pre>

<pre>
<strong>cisco-router-1941# <code>debug ppp authentication</code></strong>
</pre>

<br/><br/>

Отключить debug:

<pre>
<strong>cisco-router-1941# <code>terminal no monitor </code></strong>
</pre>

<pre>
<strong>cisco-router-1941# <code>no debug ppp negotiation</code></strong>
</pre>

<pre>
<strong>cisco-router-1941# <code>no debug ppp authentication</code></strong>
</pre>
