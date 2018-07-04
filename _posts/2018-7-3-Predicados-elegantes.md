---
layout: post
title: Predicados significativos en lambdas
categories: lambdas
excerpt_separator: <!--more-->
---
Cuando ponemos filtros complejos en expresión ambda, se vuelven difíciles de leer y luego no siempre es evidente lo que pretendemos conseguir.
Para solventar esto, y a además tener un código más elegante podemos extraer los predicados a un método de modo que todo quede mucho más legible.
<!--more-->

Pongamos por caso:
{% highlight java %}
Map<String, Double> ventas = compraventas.get("V").stream()
		 .filter(v -> v.getIndEstadoCv().equals(OmIndEstadoCV.R) ||
		 v.getIndEstadoCv().equals(OmIndEstadoCV.PE)
		 || v.getIndEstadoCv().equals(OmIndEstadoCV.PF) ||
		 v.getIndEstadoCv().equals(OmIndEstadoCV.F))
		 .collect(Collectors.groupingBy(c -> c.getAnioMesCompraventa(),
		 Collectors.summingDouble(c -> c.getCantidadCv().doubleValue())));
{% endhighlight %}

Ahora extraemos los filtros a un método:

{% highlight java %}
private Predicate<OmDetalleCompraventa> getComputableItems() {
		return c -> c.getIndEstadoCv().equals(OmIndEstadoCV.R) || c.getIndEstadoCv().equals(OmIndEstadoCV.PE)
				|| c.getIndEstadoCv().equals(OmIndEstadoCV.PF) || c.getIndEstadoCv().equals(OmIndEstadoCV.F);
	}
{% endhighlight %}

Ahora la expresión queda mucho más legible

{% highlight java %}
		Map<String, Long> compras = compraventas.get("C").stream()
				.filter(getComputableItems())
				.collect(Collectors.groupingBy(c -> c.getAnioMesCompraventa(),
						Collectors.summingLong(c -> c.getCantidadCv().longValue())));
{% endhighlight %}


Dándole una vuelta más a esto podríamos tambien tener un método que nos compusiera una lista de predicados para aplicarlos a una lambda.

Nuestro método de componer filtros podría ser algo así:

{% highlight java %}
private List<Predicate<TmBuquesDetalle>> composeFilters(List<Predicate<TmBuquesDetalle>> lfilter,
			BuquesFilter f) {
		lfilter = new ArrayList<>();
		if (f.getDesBuque() != null && !f.getDesBuque().isEmpty()) {
			lfilter.add(getDesBuqueFilter(f.getDesBuque()));
		}
		if (f.getFecIniVigencia() != null) {
			lfilter.add(getDateFromFilter(f.getFecIniVigencia()));
		}
		if(f.getFecFinVigencia() != null){
			lfilter.add(getDateToFilter(f.getFecFinVigencia()));
		}
		if (f.getTamanyoBuque() != null && !f.getTamanyoBuque().isEmpty()) {
			lfilter.add(getSizeFilter(f.getTamanyoBuque()));
		}
		if(f.isActivo()){
			lfilter.add(getActiveFilter());
		}


		return lfilter;
	}
{% endhighlight %}

y esto sería uno de los filtros
{% highlight java %}
private Predicate<TmBuquesDetalle> getActiveFilter() {
		return dto -> dto.getFecBaja() == null && dto.getFecInserc().before(DateUtil.getToday());
	}
{% endhighlight %}

Una vez que tenemos la lista de predicados, sólo tenemos que aplicarla

{% highlight java %}
private void applyFilters() {
		
		filters = composeFilters(filters, filter);
		if (!filters.isEmpty()) {
			resultList = unfilteredList.stream().filter(filters.stream().reduce(p -> true, Predicate::and))
					.collect(Collectors.toList());
		}
	}
{% endhighlight %}
