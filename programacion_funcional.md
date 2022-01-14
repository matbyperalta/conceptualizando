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


En un sentido mas extricto, podemos pensar en una función como un aparato o sistma, el cual recibe una entrada o dominio, el proceso que realice con el dominio es la función, y el resultado es el contradominio.

![image](https://user-images.githubusercontent.com/44678730/142351001-09136ea7-5221-45c5-bda4-f6707beee93e.png)

En el mundo real podemos representarlo como una maquina dispensadora de alimentos, le das como entrada el alimento que quieres, y te entrega un alimento.

Y para los que tuvieron la fortuna de saborear las matematicas no podemos olvidarnos del famoso:
![image](https://user-images.githubusercontent.com/44678730/142774093-3b95c407-fa34-4c30-b8e4-1538f12d109f.png)

Todo un clasico, el número que entra a la maquina (función) se denota con una letra, al número que sale se denota con f(x). Esta notación matematica es crucial para la definición sintactica de las expresiones lambda incluidas en Java SE 8.


### Expresiones lambda

Antes de hablar de las expresiones lambda recordemos a sus antecesores, su majestad la Clase Interna Anónima y a sus nobles primas las Interfaces Funcionales.
 
La <b>clase interna anónima</b> permite implementar clases para generar objetos que solo se usaran una vez y por tanto no se le volvera a hacer una referencia, no tiene nombre, se declara y se crea en la misma declaración. Se usa reemplazando las variables de un objeto por la palabra reservada `new`. Uno de los ejempos mas habituales es la implementación de la interfaz `Runnable`.

```markdown
MyThread myThread = new MyThread(); //donde MyThread implementa la interfaz Runnable
Thread runner = new Thread(myThread);
runner.start();
```

Pero con su majestad la clase interna anónima tenemos un resultado eficiente
```markdown
Thread runner = new Thread(new Runnable() {
    public void run() {
        //el hilo hace su trabajo aquí
    }
});
runner.start();
```

Sin embargo, una clase separada que implementa Runnable es necesaria por cada hilo que se requiera, muchos dicen que al colocar la clase como parametro en la creación del objeto es mucho mas facil de leer, a mi no me parece, en lo que si estoy de acuerdo, es que es muy poco elegante al usar mucho código para definir un objeto o un metodo.

Las <b>interfaces funcionales</b> son conocidas desde Java SE 8 por definir un solo metodo, este tipo de interfaz fueron conocidas en el pasado como  Single Abstract Method type (SAM).

```markdown
package java.lang;
public interface Runnable {
  void run();
}
```

El uso de interfaces funcionales con clases internas anónimas es un patron comun en Java, y el uso de interfaces funcionales se usan en expresiones lambda. Finalmente, no olvidemos que este tipo de codificación no resuelve el problema de la verticalidad.

### Sintaxis de expresiones lambda

Las expresiones lambda resuelven la verticalidad de 5 lineas de código en una sola sentencia. Una expresión lambda es compuesta por tres partes:

<table>
  <tbody>
    <tr>
      <td>Lista de Argumentos</td>
      <td>Simbolo flecha</td>
      <td>Cuerpo</td>
    </tr>
  </tbody>
  <tbody>
    <tr>
      <td><code>(int x, int y)</code></td>
      <td><code>-&gt;</code></td>
      <td><code>x + y</code></td>
    </tr>
  </tbody>
</table>

El cuerpo puede ser una expresión simple o un bloque de sentencia, en la expresión simple el cuerpo es evaluado y retornado, en la expresión bloque el cuerpo es evaluado como el cuerpo de un metodo y una sentencia de retorno devuelve el control a quien invoca el metodo anónimo.

Siguiendo con el ejemplo de la interfaz Runnable, vamos a la señorita en acción:

```markdown
public class RunnablePrueba{
  public static void main(String [] vargArg){
    //Clase interna anonima
    Runnable anonimo = new Runnable(){
      @Override
      public run(){
        System-out.printl("... anonimo por aqui! ...");
      }
    };
    //expresion lambda
    Runnable lambda = () -> Systme.out.println("... lambda por aqui! ...");

    anonimo.run();
    lambda.run();
  }
}
```
La expresión lambda convierte 5 líneas de código en 1 línea de código.

Ahora un ejemplo de orden ascendente y descendente:
```markdowon
public class Main {
    public static void main(String[] varg) {
        List<Persona> personaList = Persona.crearList();
        // Orden ascendente con clase interna
        Collections.sort(personaList, new Comparator<Persona>() {
            public int compare(Persona p1, Persona p2) {
                return p1.getNombre().compareTo(p2.getNombre());
            }
        });
        System.out.println("=== Orden ascendente nombre ===");
        for (Person p : personList) {
            p.imprimirNombre();
        }
        // Orden  descendente con expresion lambda
        System.out.println("=== Orden descendente nombre ===");
        Collections.sort(personList, (Persona p1, Persona p2) -> p2.getNombre().compareTo(p1.getNombre()));
        for (Persona p : personList) {
            p.imprimirNombre();
        }
	System.out.println("=== Orden descendente SurName ===");
        Collections.sort(personList, (p1, p2) -> p2.getSurName().compareTo(p1.getSurName()));
        for (Person p : personList) {
            p.printName();
        }
    }
}
```
La primera expresión lambda declara el tipo del parámetro, en la segunda expresión lambda no. Lambda soporta "target typing" permitiendo inferir el tipo del parámetro dependiendo del contexto en cual se esta usando.

## Caso de uso de una consulta comun
Un programa para un caso de uso comun es hacer busquedas en colecciones de registros que coincidan con condiciones especificas. Dada una lista de clientes, varios criterios son usados para establecer contacto con los clientes.

En este ejemplo, el mensaje necesita ser enviado a tres diferentes tipos de clientes:
- Monoproductos: clientes con un solo producto.
- Bi-producto: clientes con dos productos.
- Multiproductos: cliente con tres productos.

Clase Customer
```markdowon
public class Customer {
  private String firstName;
  private String lastName;
  private int age;
  private Gender gender;
  private String eMail;
  private String phone;
  private String address;
  private List<Product> listProduct;
```

La clase Customer usa un builder para crear nuevos objetos. Una lista simple de productos por cliente es creada con el método createCustomerList.

```markdowon
public static List<Customer> createCustomerList(){
    List<Customer> people = new ArrayList<>();
    
    people.add(
      new Builder()
            .firstName("Mario")
            .lastName("Vargas")
            .age(20)
            .gender(Gender.MALE)
            .email("mario.vargas@example.com")
            .phoneNumber("201-121-4678")
            .address("Calle 152 c 72-89")
            .listProduct(Product.createProductByCustomer())
            .build() 
      );
    
    people.add(
      new Builder()
            .firstName("Yeny")
            .lastName("Perez")
            .age(25)
            .gender(Gender.FEMALE)
            .email("yeny.perez@example.com")
            .phoneNumber("202-123-4678")
            .address("Calle 152 c 72-90")
            .listProduct(Product.createProductByCustomer())
            .build() 
      );
    
    people.add(
      new Builder()
            .firstName("Rosa")
            .lastName("Blanco")
            .age(26)
            .gender(Gender.MALE)
            .email("rosa.blanco@example.com")
            .phoneNumber("202-123-4678")
            .address("Calle 152 c 72-91")
            .listProduct(Product.createProductByCustomer())
            .build()
    );
    
    people.add(
      new Builder()
            .firstName("James")
            .lastName("Rodriguez")
            .age(45)
            .gender(Gender.MALE)
            .email("james.rodriguez@example.com")
            .phoneNumber("333-456-1233")
            .address("Calle 152 c 72-92")
            .listProduct(Product.createProductByCustomer())
            .build()
    );
    
    people.add(
      new Builder()
            .firstName("Joe")
            .lastName("Arroyo")
            .age(68)
            .gender(Gender.MALE)
            .email("joe.arroyo@example.com")
            .phoneNumber("112-111-1111")
            .address("Calle 152 c 72-89")
            .listProduct(Product.createProductByCustomer())
            .build()
    );
    
    people.add(
      new Builder()
            .firstName("Phil")
            .lastName("Anselmo")
            .age(55)
            .gender(Gender.MALE)
            .email("phil.anselmo@examp;e.com")
            .phoneNumber("222-33-1234")
            .address("Calle 152 c 72-89")
            .listProduct(Product.createProductByCustomer())
            .build()
    );
    
    people.add(
      new Builder()
            .firstName("Betty")
            .lastName("La Fea")
            .age(84)
            .gender(Gender.FEMALE)
            .email("betty.lafea@example.com")
            .phoneNumber("211-33-1234")
            .address("Calle 152 c 72-89")
            .listProduct(Product.createProductByCustomer())
            .build()
    );
    
    return people;
  }
```
### Un primer intento
Con una clase Customer y los creterios de busqueda definidos, se escribe la clase FilterContactMethods. Una posible solución define una metodo por cada caso de uso:

Clase FilterContactMethods
```markdowon
public class FilterContactMethods {

    public void callCustomerMonoProduct(List<Customer> c1) {
        for (Customer c : c1) {
            if (c.getListProduct().size() == 1) {
                call(c);
            }
        }
    }

    public void callCustomerBiProduct(List<Customer> c1) {
        for (Customer c : c1) {
            if (c.getListProduct().size() == 2) {
                mail(c);
            }
        }
    }

    public void callCustomerMultiProduct(List<Customer> c1) {
        for (Customer c : c1) {
            if (c.getListProduct().size() >= 3) {
                eMail(c);
            }
        }
    }

    public void call(Customer c) {
        System.out.println("Calling " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getPhone());
    }

    public void mail(Customer c) {
        System.out.println("Mailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getAddress());
    }

    public void eMail(Customer c) {
        System.out.println("EMailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.geteMail());
    }
}
```
Se observa como los métodos callCustomerMonoProduct, callCustomerBiProduct y callCustomerMultiProduct describen el tipo de comportamiento que tiene a lugar. Cada criterio de busqueda es claramente definido y hace una llamado a cada acción. Sin embargo, el diseño tiene aspectos negativos:

- No sigue el principio DRY
	- Cada método repite un mecanismo de bucle.
	- El criterio de seleccion debe ser escrito por cada método.
- Se requiere un largo número de método para implimentar cada caso de uso.
- El código es inflexible. Si hay un cambio en las condiciones se debe modificar gran parte del código, haciendo el código poco mantenible.

### Refactorizar los métodos
¿Como podemos arreglar este desproposito?, los criterios de busqueda son un buen lugar para empezar, entonces separemos las condiciones de busqueda en métodos separados.

Clase FilterContactMethods2

```markdowon
public class FilterContactMethods2 {

    public void callCustomerMonoProduct(List<Customer> c1) {
        for (Customer c : c1) {
            if (isMono(c)) {
                call(c);
            }
        }
    }

    public void callCustomerBiProduct(List<Customer> c1) {
        for (Customer c : c1) {
            if (isBi(c)) {
                mail(c);
            }
        }
    }

    public void callCustomerMultiProduct(List<Customer> c1) {
        for (Customer c : c1) {
            if (isMulti(c)) {
                eMail(c);
            }
        }
    }

    public boolean isMono(Customer c){
        return c.getListProduct().size() == 1;
    }

    public boolean isBi(Customer c){
        return c.getListProduct().size() == 2;
    }

    public boolean isMulti(Customer c){
        return c.getListProduct().size() >= 3;
    }

    public void call(Customer c) {
        System.out.println("Calling " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getPhone());
    }

    public void mail(Customer c) {
        System.out.println("Mailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getAddress());
    }

    public void eMail(Customer c) {
        System.out.println("EMailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.geteMail());
    }
}
```

Los criterios de busqueda son encapsulados en métodos, lo cual es una mejora con respecto al enfoque anterior, las condiciones de pruebas pueden ser reusadas. Sin embargo, hay aun código repetido y un método separado es requerido para cada caso de uso. ¿Hay una mejor forma de pasar los criterios de busqueda a los métodos?

### Clases anónimas
Antes de las expresiones lambda, su majesta la clase interna anónima dominaba el territorio. Por ejemplo, si tenemos una interfaz MyTest con un solo método test que retorna un boolean puede ser una opción (una interfaz funcional). Los criterios de busqueda pueden ser pasados cuando el método es llamado. Una interfaz tal como:
```markdowon
public interface Test<T>{
    public boolean test(T t);
}
```
Ahora actualizamos la clase FilterContactMethods y tenemos:
```markdowon
public class FilterContactMethods3 {

    public void callCustomerMonoProduct(List<Customer> c1, Test<Customer> cTest) {
        for (Customer c : c1) {
            if (cTest.test(c)) {
                call(c);
            }
        }
    }

    public void callCustomerBiProduct(List<Customer> c1, Test<Customer> cTest) {
        for (Customer c : c1) {
            if (cTest.test(c)) {
                mail(c);
            }
        }
    }

    public void callCustomerMultiProduct(List<Customer> c1, Test<Customer> cTest) {
        for (Customer c : c1) {
            if (cTest.test(c)) {
                eMail(c);
            }
        }
    }

    public void call(Customer c) {
        System.out.println("Calling " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getPhone());
    }

    public void mail(Customer c) {
        System.out.println("Mailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getAddress());
    }

    public void eMail(Customer c) {
        System.out.println("EMailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.geteMail());
    }
}
```
Esta es otra mejora, solo se necesitan tres métodos para realizar las operaciones de los casos de uso. Sin embargo, veremos que hay un tanto de fealdad cuando los metodos son llamados. A continiuación la clase de prueba evidenciando dicha fealdad:
```markdowon
public class Main {
    public static void main(String[] varg) {

        FilterContactMethods filterContactMethods = new FilterContactMethods();
        List<Customer> listCustomer = Customer.createCustomerList();
        System.out.println("\n==== Test 01 ====");
        System.out.println("=== Calling all customer ===\n");

        filterContactMethods.callCustomerMonoProduct(listCustomer);
        filterContactMethods.callCustomerBiProduct(listCustomer);
        filterContactMethods.callCustomerMultiProduct(listCustomer);

        System.out.println("\n==== Test 02 ====");
        System.out.println("=== Calling all customer refactor ===\n");

        FilterContactMethods2 filterContactMethods2 = new FilterContactMethods2();
        filterContactMethods2.callCustomerMonoProduct(listCustomer);
        filterContactMethods2.callCustomerBiProduct(listCustomer);
        filterContactMethods2.callCustomerMultiProduct(listCustomer);

        System.out.println("\n==== Test 03 ====");
        System.out.println("=== Calling all customer anonymous class ===\n");

        FilterContactMethods3 filterContactMethods3 = new FilterContactMethods3();
        filterContactMethods3.callCustomerMonoProduct(listCustomer,
                new Test<Customer>() {
                    @Override
                    public boolean test(Customer c) {
                        return c.getListProduct().size() == 1;
                    }
                });

        filterContactMethods3.callCustomerBiProduct(listCustomer,
                new Test<Customer>() {
                    @Override
                    public boolean test(Customer c) {
                        return c.getListProduct().size() == 2;
                    }
                });

        filterContactMethods3.callCustomerMultiProduct(listCustomer,
                new Test<Customer>() {
                    @Override
                    public boolean test(Customer c) {
                        return c.getListProduct().size() >= 3;
                    }
                });

    }
}
```
En las rutinas de prueba para FilterContactMethods3 podemos ver el problema de la verticalidad, este código es dificil de leer, y adicionalmente tenemos que escribir un criterio de busqueda para cada caso de uso.

### La expresión lambda al rescate
La expresión lambda resuelve los problemas y las fealdades anteriormente descritas

### java.util.function
En el ejemplo de la clase interna anónima, la interfaz funcional Test es pasada como clase anónima a los métodos. En java 8, esta interfaz no es necesaria, ya que posee el paquete java.util.function con un conjunto de interfaces funcionales estandar, para nuestro caso de uso, la interfaz funcional Predicate reune lo que necesitamos para nuestro caso de uso.

```markdowon
public class FilterContactMethods4 {
    public void callCustomerMonoProduct(List<Customer> c1, Predicate<Customer> cTest) {
        for (Customer c : c1) {
            if (cTest.test(c)) {
                call(c);
            }
        }
    }

    public void callCustomerBiProduct(List<Customer> c1, Predicate<Customer> cTest) {
        for (Customer c : c1) {
            if (cTest.test(c)) {
                mail(c);
            }
        }
    }

    public void callCustomerMultiProduct(List<Customer> c1, Predicate<Customer> cTest) {
        for (Customer c : c1) {
            if (cTest.test(c)) {
                eMail(c);
            }
        }
    }

    public void call(Customer c) {
        System.out.println("Calling " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getPhone());
    }

    public void mail(Customer c) {
        System.out.println("Mailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.getAddress());
    }

    public void eMail(Customer c) {
        System.out.println("EMailing " + c.getFirstName() + " " + c.getLastName() + " " + c.getAge() + " at " + c.geteMail());
    }
}
```

### Problema de verticalidad resuelto
La expresión lambda resuelve el problema y permite el reúso fácil de cualquier expresión.

```markdowon
public class Main {
    public static void main(String[] varg) {

        List<Customer> listCustomer = Customer.createCustomerList();
        System.out.println("\n==== Test 04 ====");
        System.out.println("=== Calling all customer predicate function ===\n");

        FilterContactMethods4 filterContactMethods4 = new FilterContactMethods4();
        Predicate<Customer> monoProduct = mono -> mono.getListProduct().size() == 1;
        Predicate<Customer> biProduct = bi -> bi.getListProduct().size() == 2;
        Predicate<Customer> multiProduct = multi -> multi.getListProduct().size() >= 3;

        filterContactMethods4.callCustomerMonoProduct(listCustomer, monoProduct);
        filterContactMethods4.callCustomerBiProduct(listCustomer, biProduct);
        filterContactMethods4.callCustomerMultiProduct(listCustomer, multiProduct);
    }
}
```
Fealdad resuelta, el código es compacto y fácil de leer.

## A Java 8 le gusta la vida fácil
Gracias al paquete java.util.function, la vida es mas facil. Fue introducido en java 8 para implementar programación funcional. Las interfaces funcionales proporcionan los tipos destino de las expresiones lambda. Cada interfaz funcional tiene un método abstracto llamado método funcional a los que se adaptan o emparejan los parámetros de la expresión lambda y el tipo de retorno.


![image](https://user-images.githubusercontent.com/44678730/149440523-8b9dc2c1-86ba-4667-b891-2cba24830828.png)

Un conjunto de interfaces estandar son entregadas como punto de inicio para los desarrolladores, algunas de las mas importantes son:

- Predicate: Una propiedad del objeto pasado como argumento
- Consumer: Una acción a realizar con el objeto pasado como argumento
- Function: Transforma una T a un R
- Supplier: Provee una instancia de una T (como una fabrica)

### Function




