---
layout: post
title: Lambda para sumar agrupado
excerpt_separator: <!--more-->
---

<!--more-->

LLevaba tiempo detrás de esto, y al final lo he conseguido :-)

{% highlight java %}

	public Map<String, Double> getTotalsFees() {
		return parent.getOpMercado().getOmEconomicoFees().stream().collect(
				Collectors.groupingBy(f -> f.getId().getIndCobropago().name(),
						Collectors.summingDouble(f -> f.getImpFee().doubleValue())));
		
	}

{% endhighlight %}

Luego también he visto otra solución en StackOverFlow que parece interesante....
{% highlight java %}


{% highlight java %}
