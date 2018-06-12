---
layout: post
title: Seleccionar la m�xima versi�n paso a paso
categories: {JPA, CriteriaBuilder}
excerpt_separator: <!--more-->
---
C�mo seleccionar la m�xima versi�n de un registro con JPA y CriteriaBuilder.

<!--more-->

En muchas ocasiones nos encontramos con tablas en las que s�lo se insertan registros porque se desea mantener un versionado.

##Caso 1: Por n�mero de versi�n (OmDatosGeneraleSpecification)

Lo primero que tenemos que hacer es definir la subquery con la que haremos el join de la entidad con si misma:

{% highlight java %}

	Subquery<Long> subqueryVersion = query.subquery(Long.class);
	Root<OmDatosGenerale> entity1 = subqueryVersion.from(OmDatosGenerale.class);
	Expression<Long> expMaxVersion = builder.greatest(entity1.<Long> get("numVersion"));

{% endhighlight %}
Con esto decimos que la subquery va a devolver un Long y que seleccionaremos desde la entidad OmDatosGenerale 
tambi�n que queremos el m�ximo numero de versi�n; dicho en SQL es algo as� como:
{% highlight sql %}
select max(num_version) from ...
{% endhighlight %}
