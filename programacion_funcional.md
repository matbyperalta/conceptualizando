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

Algunos lenguajes soportan multiples paradigmas, para el caso de java, en principio es dirigido a objetos, a partir de la versión 8 se incluyen mejoras que permiten la programación funcional, pero no perdamos de vistas otros paradigmas soportados por java tales como el reflexivo, generico, ¿cuales otros mas?




## El papel de los genericos
Solo vasta con darle una mirada a la documentación de nuevas interfaces en java 8 tales como [Function](https://docs.oracle.com/javase/8/docs/api/java/util/function/Function.html) o [Stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html) que en su definición usan genericos ampliamente, genericos en programación funcional es como una cerveza para la resaca.

Sin mas preambulo, si quieres conocer con mayor detalle sobre genericos recomiendo leer la documentación de oracle java sobre genericos, [Generics](https://docs.oracle.com/javase/tutorial/java/generics/index.html), por el momento, el presente articulo da una breve introducción.

Un generico es la forma mas general de definir una clase, interfaz o metodo, permitiendo usar el mismo código con diferentes parametros, adicionalmente, brinda mayor estabilidad teniendo en cuenta que elimina los problemas de casting.

### Tipos Genéricos
Se refiere a clases o interfaces que son parametrizados usando tipos

En una clase Producto que trabaja con objetos de cualquier tipo:

```markdown
public class Producto {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

Teniendo en cuenta la genericidad del atributo object, no hay forma de verificar el código en tiempo de compilación. Ahora demos un vistazo al mismo código usando genericos.

```markdown
public class Producto<T> {
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```
Tenga en cuenta que en la definición de una clase generica se puede usar mas de un parametro de tipo generico.

```markdown
class name<T1, T2, ..., Tn> { /* ... */ 
```
### Convención de nombres
<ul>
<li>E - Elemento (Usado en las colecciones)</li>
<li>K - Clave</li>
<li>N - Número</li>
<li>T - Tipo</li>
<li>V - Valor</li>
<li>S,U,V etc. - 2nd, 3rd, 4th tipos</li>
</ul>

### Multiples parametros de tipo
El siguiente ejemplo es sobre una interfaz que define dos parametros, uno para representar una clave y el otro para un valor.

```markdown
public interface Producto<K, V> {
    public K getClave();
    public V getValor();
}

public class Activo<K, V> implements Producto<K, V> {

    private K clave;
    private V valor;

    public Activo(K clave, V valor) {
	this.clave = clave;
	this.valor = valor;
    }

    public K getClave()	{ return clave; }
    public V getValor() { return valor; }
}
```

De tal forma que al instanciarlo tenemos.
```markdown
Producto<Integer,Credito> = new Activo<Integer,Credito>(1,new Credito());
```


