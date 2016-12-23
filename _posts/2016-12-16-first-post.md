---
layout: post
title: "Mon premier post"
description: "Une petite experience numerique pour mettre en évidence le théorème central limite."
category: [r, statistics, fr]
tags: [r, statistics, fr]
comments: true
---






Ceci est mon premier article de blog, plus pour mettre en place le site que par intérêt pour le théorème central limite.

## Le théorème central limite

On génère des données binomiales $$\mathcal{B}(1, P)$$.

{% highlight r %}
size <- 1
P <- 0.5
X <- rbinom(n = 20000, size, P)
{% endhighlight %}


D'après le théorème central limite 

$$
\hat{P_n} = \frac{1}{n} \sum_i X_i
$$

doit tendre vers une loi normal quand $$n$$ devient grand

$$
\mathcal{N}(\mu = P,\sigma^2 = \frac{P(1-P)}{n}).
$$

On représente l'histogramme de $$\hat{P_n}$$.

On commence par calculer un échantillon de $$\hat{P_n}$$ à partir de X.

{% highlight r %}
n <- 100
Pn.size <- length(X) / n
Pn <- X %>%
  matrix(nrow = Pn.size, ncol = n) %>%
  array_branch(1) %>%
  map_dbl(mean)
{% endhighlight %}

On trace l'histogramme des $$\hat{P_n}$$ en bleue et la distribution théorique en vert.

{% highlight r %}
breaks <- hist(Pn, plot = FALSE, )$breaks
ggplot(data.frame(Pn = Pn), aes(x = Pn)) +
  geom_histogram(aes(y=..density..), breaks = breaks, fill = "blue4") + 
  geom_density(aes(y=..density..), color = "blue4") + 
  stat_function(fun = dnorm, args = list(mean = P, sd = sqrt(P * (1 - P)) / sqrt(n)), color = "green") +
  xlim(0.15,0.85) + 
  gtheme
{% endhighlight %}

<img src="/blog/figure/source/2016-12-16-first-post/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

On peut aussi calculer un échantillon de $$\hat{P_n}$$ par boostrap, c'est à dire que l'on prend au hasard
$$n$$ individu dans notre échantillon pour calculer $$\hat{P_n}$$ et on recommence.


{% highlight r %}
bootstrap <- function(i, X, n) {
  mean(X[sample.int(length(X), n)])
}
Pn.bootstrap <- 1:Pn.size %>%
  map_dbl(bootstrap, X = X, n = n)
{% endhighlight %}

On trace l'histogramme des $$\hat{P_n}$$ en bleue et la distribution théorétique en vert.

{% highlight r %}
breaks <- hist(Pn.bootstrap, plot = FALSE)$breaks
ggplot(data.frame(Pn.bootstrap = Pn.bootstrap), aes(x = Pn.bootstrap)) +
  geom_histogram(aes(y=..density..), breaks = breaks, fill = "blue4") + 
  geom_density(aes(y=..density..), color = "blue4") + 
  stat_function(fun = dnorm, args = list(mean = P, sd = sqrt(P * (1 - P)) / sqrt(n)), color = "green") +
  xlim(0.15,0.85) + 
  gtheme
{% endhighlight %}

<img src="/blog/figure/source/2016-12-16-first-post/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />
