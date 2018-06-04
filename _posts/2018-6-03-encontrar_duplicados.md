---
layout: post
title: Lambda para encontrar duplicados
categories: lambdas
excerpt_separator: <!--more-->
---
Lambda para encontrar duplicados en un stream

<!--more-->



{% highlight java %}

	long atributosAgrupados = atributoValores.stream()
				.collect(Collectors.groupingBy(av -> av.getId(), Collectors.counting())).keySet().stream().count();
		long atributos = atributoValores.size();
		if (atributosAgrupados != atributos) {
		......

{% endhighlight %}

Lo que hace el lambda es agrupar el stream por su id (que es una clave compuesta), y contarlos, con lo que se obtiene un map de los elementos y 
su número. 

El número de elementos del map debería ser igual que el número de elmentos del stream, si es que no hay duplicados

