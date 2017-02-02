---
layout: post
title: Montando Owncloud en mi Orange Pi
excerpt_separator: <!--more-->
---
Owncloud en Orange Pi con algunas particularidades que lo hacen interesante

<!--more-->

El problema a solucionar es el siguiente:
Intenté montar un owncloud en mi hosting compartido en hostgator, pero esto ofrecía algunas dificultades:
1. Si te lees la letra pequeña, está expresamente prohibido
2. Pese a que instala sin problemas, frecuentemente fallaban cosas por out of memory

Así pues, quiero montar un servidor de owncloud en mi Orange pi con las siguientes particularidades:
* Evidentemente no quiero alojar los archivos en la propia SD
* El disco duro externo de la pi no siempre está disponible, así que no es una opción válida
* Tampoco quiero comprar otro disco
* No quiero alojar la base de datos en la pi, gasta memoria y pretendo ampliar los servicios que proporciona.

## Soluciones

Para el almacenamiento he decido montar por webdav un directorio de mi hosting en hostgator. Webdav no rinde muy bien pero
como es para mis archivos personales, quizá baste y sobre.

Para la Base de datos también una en hostgator, permite crear BD MySQL que son accesibles directamente desde el exterior.

### Montando davfs

En primer lugar instalamos davfs2
{% highlight bash %}
sudo apt-get install davfs2
{%endhighlight%}
