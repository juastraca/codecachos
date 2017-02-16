---
layout: post
title: Cliente VPN Windows
excerpt_separator: <!--more-->
---
Particularidades de conexion en windows a OpenVPN en una red con seguridad paranoica.
Estoy montando una vpn para conectarme a mi orange pi desde una red windows con seguridad absurda. Y me estan surgiendo
algunos problemillas. 

<!--more-->

El primero de ellos es que por alguna razón no funciona el DNS que le inyecto por el servidor VPN, así que
hay que configurar el DNS para mi VPN desde windows.

Establecer DNS 
ejecutar netsh y listar interfaces
{% highlight bash %}
interface ip show config
{%endhighlight%}
Con esto averigüo el nombre de la interfaz de red que me interesa. En mi caso se llama "Local Area Connection 2"
luego:
{% highlight bash %}
interface ip set dns "Local Area Connection 2" static 8.8.8.8
{%endhighlight%}

