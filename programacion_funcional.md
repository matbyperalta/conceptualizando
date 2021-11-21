# Repasando Programación Funcional

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

De tal forma que al instanciar tenemos.
```markdown
Producto<Integer,Credito> = new Activo<Integer,Credito>(1,new Credito());
```
### Metodos genericos
Incluye sus propios tipos de parametros, se permite metodos estaticos y no estaticos con constructor generico. La sintaxis incluye una lista de parametros dentro de parentisis angulares que aparecen antes del tipo de retorno de la firma el metodo.
```markdown
class Repository {
    public static <K, V> void save(CacheObject<K, V> p1) {

    }
}

 class CacheObject<K, V> {
    private K key;
    private V value;

    public CacheObject(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}

class CustomerType{

}
```
Se puede invocar usando de manera explicita el tipo
```markdown
CacheObject<Integer, CustomerType> p1 = new CacheObject<>(1, new CustomerType());
Repository.<Integer,CustomerType>save(p1);
```
O se puede invocar excluir el tipo y el compilador infiere el tipo
```markdown
CacheObject<Integer, CustomerType> p1 = new CacheObject<>(1, new CustomerType());
Repository.save(p1);
```
### Tipo de parametro delimitado
En algunos casos se necesita restringir el tipo que puede ser usado como argumento, por ejemplo un metodo que opera productos de credito bancario querra solamente aceptar productos que extiendan de la clase credito o sus subclases.
```markdown
class Loan{ }
class InvesmentFree extends Loan{ }
class InterestService{
  public <U extends Loan> void calculateMonthlyRate(U u){
  }
}
```
### Comodines
Se representa por el simbolo interrogante ?, y significa que puede representar cualquier tipo. Puede ser usado en parametros de tipo, campos, variables locales, en el retorno, pero nunca puede ser usado como argumento en la invocación de un metodo, instanciación de una clase o un super tipo.
### Comodines con delimitación superior
Se usa para relajar la restricción de una variable. Por ejemplo, si necesita un metodo que trabaje con List<InvesmentFree>, List<Rotary>, List<BuyWallet>.
```markdown
public static void process(List<? extends Loan> list) { /* ... */ }
```
En este caso <? extends Loan>, Loan es cualquier tipo, y ? es cualquier Loan o cualquier subtipo de Loan. Ejemplo de uso.
```markdown
public static void process(List<? extends Loan> list) {
    for (Loan elem : list) {
        // ...
    }
}
```

## A lo que vinimos
Programación funcional es tal y como suena, ¡funciones!, entonces vamos al principio.

Una función es una regla de correspondencia entre dos conjuntos de tal manera que a cada elemento del primer conjunto le corresponde uno y sólo un elemento del segundo conjunto.

![image](https://user-images.githubusercontent.com/44678730/142350984-26873740-ebb8-4a39-8c0b-09bdedfeb7c5.png)


En un sentido mas extricto, podemos decir una función como un aparato o sistma, el cual recibe una entrada o dominio, el proceso que realice con el dominio es la función, y el resultado es el contrdominio.

![image](https://user-images.githubusercontent.com/44678730/142351001-09136ea7-5221-45c5-bda4-f6707beee93e.png)

En el mundo real podemos representarlo como una maquina dispensadora de alimentos, le das como entrada el alimento que quieres, y te entrega un alimento.




