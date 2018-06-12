---
layout: post
title: Filtrar resultados por un atributo de una entidad hija
categories: {JPA, CriteriaBuilder}
excerpt_separator: <!--more-->
---

En esta ocasión, vamos a filtrar registros que tengan una determinada cartera de clientes, por ejemplo. Cada agente tiene varias carteras
pero a nosotros nos interesan sólo aquellos que tengan una determinada cartera.

Para ello lo primero que hacemos es declarar la lista de carteras:

{% highlight java %}
ListJoin<Agente, Cartera> carteraJoin = root.joinList("carteras");
query.distinct(true);
{%endhighlight%}

El resto se realiza de manera natural esto es componiendo el predicado en base a esta join

{% highlight java %}
builder.equal(carteraJoin.get("id").get("idCartera"), idCartera);
{%endhighlight%}