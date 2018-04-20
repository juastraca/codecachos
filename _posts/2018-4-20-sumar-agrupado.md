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

{% raw %}

 public static void main(final String[] args) {
        final Map<Integer, Map<String, Double>> tmp = new HashMap<>();
        tmp.put(1, new HashMap<String, Double>() {{
            put("1", 3.45);
            put("2", 1.23);
            put("3", 0.98);
        }});
        tmp.put(2, new HashMap<String, Double>() {{
            put("1", 1.00);
            put("2", 2.00);
            put("3", 3.00);
        }});

        System.out.println(tmp.entrySet().stream()
                .collect(
                    toMap(Map.Entry::getKey, 
                    data -> 
                        data.getValue()
                            .values().stream()
                            .mapToDouble(Number::doubleValue).sum())));
    }
    {% endraw %}
{% highlight java %}
