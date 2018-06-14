---
layout: post
title: Al fin... Mapeos @OneToOne nullables
categories: JPA

---

Casi me había rendido con esto, pero al fín he dado con la solución:

Resulta ser que las relaciones @OneToOne que son nullables dan el famoso problema de las n+1 querys. A menos que se utilice la instrumentación
de las clases al compilar, JPA se ve irremediablemente compelido a desambigüar la situación con una select. Hablando en román paladín, necesita saber si es
nulo porque es nulo o es nulo porque no se ha recuperado.

En fin... que la solución es esta:
{% highlight java %}
	@OneToOne(optional = true, fetch = FetchType.LAZY)
	@JoinColumn(name = "ID_SIMULACION")
	@MapsId
	private SimulacionLog simulacionLog;
{%endhighlight%}

La relación ha de ser unidireccional. Lo prudente en las relaciones @OneToOne es hacerlas unidireccionales y si las necesitas bidireccionales, pues te lo piensas...

La explicación más amplia la tienes aqui <https://vladmihalcea.com/the-best-way-to-map-a-onetoone-relationship-with-jpa-and-hibernate/>