---
layout: post
title:  "Diferencias entre memcpy y memmove"
date:   2018-09-13 12:00:00 +0200
categories: programming
tags: programming memory tricks memcpy memmove cpp
image: programming.png
---

Uno de los elementos más críticos a la hora de programar es el **manejo de la memoria**. Mantener la integridad de la memoria usada, el uso de punteros o la copia de zonas de memoria deben ser realizadas de la manera más segura posible para evitar bugs. 

En este post vemos un ejemplo claro de esto en el que comparamos el uso de la función **memcpy** y **memmove**. Ambas funciones realizan la copia de un número determinado de bytes de un origen a un destino.

{% highlight cpp %}
	void * memcpy ( void * destination, const void * source, size_t num );
	void * memmove ( void * destination, const void * source, size_t num );
{% endhighlight %}

Sin embargo existe una gran diferencia entre ellas. Mientras que memcpy realiza la copia byte a byte de manera directa entre las dos zonas de memoria, la función memmove utiliza un buffer intermedio que permite que las zonas de memoria de origen y destino estén solapadas.

Vamos a ver el ejemplo siguiente en el que se ve el fallo. Imaginemos que se quiere copiar 5 bytes desde la posición 3 del buffer a la posición 5 del buffer. Cuando vayamos a copiar la posición 5, esta habrá sido sobreescrita y se copiará un valor erróneo.

![memcpy error](http://www.equestionanswers.com/c/images/memcpy_upper_overlapping.gif)
<div class="pie">Picture: http://www.equestionanswers.com</div>

En cualquier caso, su vuestro código realiza copias dentro del mismo buffer, probablemente quiera decir que se están sacando y metiendo datos en el continuamente, para lo que es altamente recomendable el uso de buffers circulares, de los cuales hay muchas [implementaciones](https://embeddedartistry.com/blog/2017/4/6/circular-buffers-in-cc) buenas en internet.
