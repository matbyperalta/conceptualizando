# Iniciando

Un paradigma no es mas que un modelo a seguir, entonces en programación de software se refiere a las diferentes clasificaciones de lenguajes de programación teniendo en cuenta sus caracteristicas y la forma como nos permite enfrentarnos a las solución de problemas. [Wikipedia](https://es.wikipedia.org/wiki/Paradigma_de_programaci%C3%B3n)

"... nunca olvides que un programa pretende resolver una problematica del mundo real, una problematica del mundo real encaja en un modelo, y un modelo tiene características"

Entre los paradigmas comunes tenemos:

<ul>
  <li>imperativo: consiste en una secuencia de instrucciones encadenadas una de tras de otra para dar ordenes a un computador, el valor de los datos es modificado durante la ejecución de sus instrucciones, los datos son manipulados con operaciones booleanas (es, o no es) y bucles, su ejecución es cercana al sistema base (lenguaje de maquina), sus lineas de código son faciles de entender pero requiere bastantes líneas para resolver el problema, algunos lenguajes son: C, C++, Java, Python, etc</>
  <li>declarativo: describe el resulatdo final esperado, y no muestra el paso a paso de sus ordenes, lo que significa que declara automaticamente la via de solución, no determina el como y tiene un nivel de abstracción muy alto. Algunos lenguajes son: prolog, lisp, SQL, etc.</>
</ul>

La programación imperativa se centra en el como, la declarativa en el que. Un ejemplo del mundo real es: un lenguaje imperativo entrega los planos de una casa, mientras que la declarativa las fotos de la casa construida y habitable.

![image](https://user-images.githubusercontent.com/44678730/141789046-cd6237a7-8389-4889-b2cb-7911d039c15d.png)

La grafica anterior representa una taxonomía muy basica de los paradigmas.

Algunos lenguajes soportan multiples paradigmas, para el caso de java, en un principio es dirigido a objetos, a partir de la versión 8 se incluyen mejoras que permiten la programación funcional, pero no perdamos de vistas otros paradigmas soportados por java tales como el reflexivo, generico, ¿cuales otros mas?




## El papel de los genericos
Solo vasta con darle una mirada a la documentación de nuevas interfaces en java 8 tales como [Function](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html) o [Stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html) que en su definición usan genericos ampliamente, 
