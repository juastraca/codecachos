---
layout: post
title: Seleccionar la máxima versión paso a paso
categories: {JPA, CriteriaBuilder}
excerpt_separator: <!--more-->
---
Cómo seleccionar la máxima versión de un registro con JPA y CriteriaBuilder.

<!--more-->

En muchas ocasiones nos encontramos con tablas en las que sólo se insertan registros porque se desea mantener un versionado.

##Caso 1: Por número de versión (OmDatosGeneraleSpecification)

Lo primero que tenemos que hacer es definir la subquery con la que haremos el join de la entidad con si misma:

{% highlight java %}

	Subquery<Long> subqueryVersion = query.subquery(Long.class);
	Root<OmDatosGenerale> entity1 = subqueryVersion.from(OmDatosGenerale.class);
	Expression<Long> expMaxVersion = builder.greatest(entity1.<Long> get("numVersion"));

{% endhighlight %}
Con esto decimos que la subquery va a devolver un Long y que seleccionaremos desde la entidad OmDatosGenerale 
también que queremos el máximo numero de versión; dicho en SQL es algo así como:
{% highlight sql %}
select max(num_version) from ...
{% endhighlight %}
