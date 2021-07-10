# Java 11 certificacion OCP
## Fundamentals
## Modifiers
### Reference Modifiers
Los siguientes son modificadores de referencia validos para usar en <span style="color: #e9c46a;">class, interface, enum</span>

<span style="color: #e9c46a;">final y abstract</span> son polos opuestos por tanto no se pueden usar al mismo tiempo

<span style="color: #e9c46a;"> static </span> no se puede ocupar en outher class

| Modifier  | Description |
| ------------- | ------------- |
| <span style="color: #e9c46a;">public  | Si es public el archivo debe de guardarse con el mismo nombre y extension .java |
| <span style="color: #e9c46a;">protected  | Solo valido para nested types  a nivel de clase|
| <span style="color: #e9c46a;">private  | Solo valido para nested types  a nivel de clase|
| <span style="color: #e9c46a;">(none)  | package privacy es asociado implicitamente |

### <span style="color: #2a9d8f;">final modifier definition

1. Se puede inicializar objetos de una clase final

2. No se puede tener una subclase de una clase final

3. Cualquier clase puede tener elementos final incluyendo una clase final

4. Una clase final puede extender de otra clase  
como ejemplo encontramos a la clase String

| Colocacion de final  | Efecto |
| ------------- | ------------- |
| <span style="color: #e9c46a;">class | No es posible sobrescribir | 
| <span style="color: #e9c46a;">field  | Su valor no puede cambiar |
| <span style="color: #e9c46a;">method | No es posible sobrescribir method| 


### <span style="color: #2a9d8f;">abstract modifier definition

En java abstract significa que requiere implementacion

1. usar <span style="color: #e9c46a;">metodo abstract</span> requiere que la clase tambien
incluya abstract

2. la class abstract</span> no se puede inicializar

3. si se extiende es necesario implementar los metodos,
a menos que se extienda por otra clase que tambien es abstract

4. se puede agregar la palabra final en una subclase para  sobrescribir el metodo



## Nested classes

Nested class son <span style="color: #e9c46a;">clases que estan definidas en el cuerpo de otra clase</span>  
Existen dos tipos de nested class en java:
1. non static nested class tambien llamada <span style="color: #2a9d8f;">inner class
2. <span style="color: #2a9d8f;">static nested class



<span style="color: #e9c46a;">Existen tres tipos de inner class

1. Inner member class: Es una clase que se define directamente en el cuerpo de otra clase

2. An local inner class: es definido en bloque de codigo, normalmente dentro de un metodo de una clase

3. An anonymous class: es un tipo de variable especial de local inner class en el que la declaracion y inicializacion se realiza en el  mismo statment

Recordatorio

1. this solo se puede acceder desde metodos de intancia
2. los bloques y metodos de instancia pueden acceder a atributos static y not static
3. los bloques y metodos estaticos unicamente pueden acceder a atributos static

4. enum,interface solo pueden ser parte de una clase inner static
ya que se solo pueden estar en el contexto static

5. los metodos static se pueden sobrecargar pero no se pueden sobrescribir

### <span style="color: #2a9d8f;"> Ejemplo de implementacion inner class

0. No se pueden acceder a los atributos o metodos directamente de una inner no static class (solo si el field es static)




1. Para crear una instancia de una innerclass (no static), siempre 
es necesario tener una referencia de EnclosingClass y usar la referencia 
seguida de la palabra new (la referencia es f)

    ```
    EnclosingClass.InnerMemberClass j = f.new InnerMemberClass();
    ```
2. Se puede definir un member de un innerclass not static en EnclosingClass
    ```
    public InnerMemberClass innerMemberClass = new InnerMemberClass();
    ```
3. tener cuidado en preguntas de certificacion ya que si existe el member de un inner class este se prodra acceder como una propiedad 
(Es posible que el nombre del field tenga el mismo nombre que el inner member clas)

    ```
    public InnerMemberClass InnerMemberClass = new InnerMemberClass();
    // recordar que los fields que no contengan static son de intancia
    ```
4. innerclasses pueden acceder desde un bloque 
a los atributos solo con el nombre

5. (shadowing) las variables de <span style="color: #e9c46a;">inner class</span>  pueden ocultar las variables de <span style="color: #e9c46a;">enclosingClass</span>  
y <span style="color: #e9c46a;">las variables locales</span> puede ocultar las variables de <span style="color: #e9c46a;">inner class </span>  por tanto usar:

    ``` 

    this.outerName  -> para acceder a variables de innerclass  

    EnclosingClass.this.outerName -> acceder a variables de la clase EnclosingClass

    ```

5. <span style="color: #e9c46a;">no puede tener interfaces o enums</span> ya que requieren contexto estatico, 
   no se puede contener metodos static
   (en caso de tener genera error de compilacion)

6. En cambio si pueden contener constantes static es decir variables definidas con static las constantes static pueden ser utilizadas sin requerir de una instancia 
   de outherclass ejemplo:

    ```

    EnclosingClass.InnerMemberClass.staticName

    ```

```java

package inner;

// Enclosing Class
public class EnclosingClass {

    // Add Constructor
    EnclosingClass(String name) {
        this.outerName = name;
    }

    // instance field on enclosing class
    public String outerName = "outer";

    // instance field of the type of the inner class
    public InnerMemberClass innerMemberClass = new InnerMemberClass();

    // instance method on enclosing class
    public void doSomethingOnInstance() {
        System.out.print("doSomethingOnInstance invoked: ");
        // invoke nested class's method via class reference
        System.out.println(new InnerMemberClass().getInstanceName());
    }

    // Begin declaration of inner member class named InnerMemberClass
    public class InnerMemberClass {

        // instance field
        public String instanceName = "InnerMemberClass.instanceName";

        // instance method
        public String getInstanceName() {
            return "getInstanceName() = " + this.instanceName;
        }

        public String getOuterName() {
            // Local variable shadows inner class member which in turn
            // shadows outer class's member.  Here we access all 3
            String outerName = "local_outerName";
            return outerName + " : " +
                    this.outerName + " : " +
                    EnclosingClass.this.outerName;
        }

        // static field
        public static final String staticName = "staticName";

        public String outerName = "outer";

    }  // Ends declaration of the inner member class

}


// This class tests the EnclosingClass and it's inner member class
// using a disassociated class.
class TestEnclosingClass {

    public static void main(String[] args) {
        EnclosingClass e = new EnclosingClass("e's Instance");
        e.doSomethingOnInstance();

        // We can declare a local variable of the inner class
        EnclosingClass.InnerMemberClass i;

        // You must use the instance new operator, much like a method.
        i = e.new InnerMemberClass();

        // Use local variable referencing the inner member class to
        // access method on the inner class
        System.out.println("Invoking i.getOuterName: " + i.getOuterName());

        // Create another instance of the Enclosing Class
        EnclosingClass f = new EnclosingClass("f's Instance");

        // Declare and Assign a variable to the inner member class of
        // new instance in a single statement
        EnclosingClass.InnerMemberClass j = f.new InnerMemberClass();
        System.out.println("Invoking j.getOuterName: " + j.getOuterName());

        e.innerMemberClass.instanceName = "Testing";
        System.out.println(e.innerMemberClass.getInstanceName());
    }
}

```

### <span style="color: #2a9d8f;">Ejemplo de implementacion nested static class

1. nested static class pueden ser accedidas directamete
    ```
    EnclosingClass.NestedStaticClass.staticName
    ```
2. se puede crear una referencia sin necesitar new (pero no se podran acceder a metodos y atributos de intancia)
    ```

    EnclosingClass.NestedStaticClass.NestedInterface n;

    ```
3. se pueden definir metodos de instancia y atributos de instancia, en  
static class

5. Se pueden crear objetos de intancia de una static class para poder acceder
a fields y metodos de intancia (Por defecto los metodos de intancia no pueden ser accedidos a menos que se cuente con 
una instancia)

    ```

    new NestedStaticClass().getInstanceName();

    ```



Ejemplo de implementacion de static nested class

```java

package nest;

// Enclosing Class
public class EnclosingClass {
    // instance field on enclosing class
    public String outerName = "outer";

    // static field on enclosing class
    public static String staticOuterName = "OUTER";

    // static method on enclosing class
    public static void doSomethingStatically() {
        System.out.println("doSomethingStatically invoked.");
        // invoke nested class's method via class reference
        NestedStaticClass.getStaticName();
    }

    // instance method on enclosing class
    public void doSomethingOnInstance() {
        System.out.println("doSomethingOnInstance invoked.");
        // invoke nested class's method via instance reference
        new NestedStaticClass().getInstanceName();
    }

    // Begin declaration of static nested class named NestedStaticClass
    public static class NestedStaticClass {
        // static field
        public static String staticName = "NestedStaticClass.staticName";

        // instance field
        public String instanceName = "NestedStaticClass.instanceName";

        // static method
        public static String getStaticName() {
            return "getStaticName() = " + NestedStaticClass.staticName;
        }

        // instance method
        public String getInstanceName() {
            return "getInstanceName() = " + this.instanceName;
        }

        public enum Colors {
            RED, BLUE, YELLOW
        }

        public interface NestedInterface {

        }
    }  // Ends declaration of the static nested class

}

// This class tests the EnclosingClass and it's nested class
// from disassociated class.
class TestEnclosingClass {

    public static void main(String[] args) {
        // Reference static field on static nested class directly:
        System.out.println(EnclosingClass.NestedStaticClass.staticName);

        // Reference static method on static nested class directly:
        System.out.println(EnclosingClass.NestedStaticClass.getStaticName());

        // Reference enum on static nested class directly:
        System.out.println(EnclosingClass.NestedStaticClass.Colors.BLUE);

        // Local variable declaration using a nested class's interface
        EnclosingClass.NestedStaticClass.NestedInterface n;

        // Reference of instance member of Nested class
        EnclosingClass.NestedStaticClass nestedStaticClass = new EnclosingClass.NestedStaticClass();
        nestedStaticClass.getInstanceName();
        

    }


```

### <span style="color: #2a9d8f;">Comparacion static nested class con inner member class (non static)


| Feature                                 | Static nested class | Inner Member Class (Not static)|
| --------------------------------------- | ------------------- | -------------------- |
| Basically the same as top-level class, just referenced using the enclosing class type | True | False |
| Static member permmited on nested class  | True  | False, with the exception of constants|
| To initialize you have to initialize the outer class| False| True |
| Has direct access to the enclosing object's instance methods and fields | False| True |
| All access modifiers are available (public,private,protected,none(package)| True | True|
| All modifiers are available (abstract, final, strictfp) | True(Final and abstract can not be combined| True(Final and abstract can not be combined|
| Nested classes can contain nested clasess of either type | True | False(inner class can not contain static nested class, enum or interface|


### <span style="color: #2a9d8f;">Local class

A local class no es miembro de un enclosing class pero <span style="color: #e9c46a;">es declarado en un bloque de codigo</span> 
y  tipicamente se encuentra en el cuerpo de un metodo, tambien puede ser en un bloque como un 
loop, switch , if

local class es como una variable local. <span style="color: #e9c46a;">existe cuando el bloque de codigo se ejecuta y 
deja de existir despues de la ejecucion del bloque</span>

Es un mecanismo conveniente cuando se quieren asociar ciertas variables a un comportamiento 
concreto


1. local class pueden acceder a variables locales
2. local class no pueden acceder a el valor de la variable si 
a esta se le asigna una valor nuevo
3. local class pueden acceder a variables de atributos y metodos de 
outer class 
4. local class pueden extender de otras clases 
5. local class no puede tener interface, enum o clases static
6. local class pueden implemtar interfaces



```java

/*
The Learn Programming Academy
Java SE 11 Developer 1Z0-819 OCP Course - Part 2
Section 1: Java Fundamentals 
Topic:  Local Classes
*/

public class LocalClassExample {

    private String defaultName = "Jane";
    private String lastName = "Doe";

    // Creating a non-static method that declares a local class
    private void methodWithLocalClass() {

        String defaultName = "Ralph";

        // LocalClass Only has scope within this method
        class LocalClass {
            // local class can have all the same members as an outer class
            // (with the exception of static members).
            private String name;

            // Constructor on local class
            private LocalClass(String name) {
                if (name == null) {
                    this.name = defaultName;
                } else {
                    this.name = name;
                }
                this.name += " " + LocalClassExample.this.lastName;
            }

            private void performSomeAction() {
                System.out.println("LocalClass.name = " + name);

            }

        }

        // Create multiple instances of local class within
        // the enclosing code's scope..
        LocalClass jeff = new LocalClass("Jeff");
        LocalClass martha = new LocalClass("Martha");
        LocalClass bob = new LocalClass("Bob");

        // Execute methods on instances of the local class
        jeff.performSomeAction();
        martha.performSomeAction();
        bob.performSomeAction();

        // Access fields directly from instance of local class
        System.out.println("Bob's name is: " + bob.name);

        new LocalClass(null).performSomeAction();
    }

    public static void main(String[] args) {
        new LocalClassExample().methodWithLocalClass();
    }
}

```


7. local class pueden extender de otra local class previamente definida
8. local class can be abstract
9. pueden definirse dentro de bloques static y no static 
si declara dentro de un metodo static no puede acceder a intance atributes

```java

/*
The Learn Programming Academy
Java SE 11 Developer 1Z0-819 OCP Course - Part 2
Section 1: Java Fundamentals 
Topic:  Local Class
*/

abstract class AbstractClass {
    public abstract void doSomething();
}

interface Executable {
    void doSomething();
}

public class LocalClassExample2 {
    String name = "Doe";

    public static void main(String[] args) {
        String name = "Smith";

        // Local class can extend any class, including abstract one
        class A extends AbstractClass {
            public void doSomething() {
                System.out.println("name = " + name);
            }
        }

        // Local class can implement interfaces
        class B implements Executable {

            public void doSomething() {
                System.out.println("name = " + name);
            }
        }

        // Declaring a local abstract class
        abstract class C extends AbstractClass {

        }

        // Create a local class extending another local class
        class D extends C {
            public void doSomething() {
                System.out.println("name = " + name);
            }
        }

        // Execute code on instances of local classes
        new A().doSomething();
        new B().doSomething();
        new D().doSomething();
    }
}


```


### <span style="color: #2a9d8f;">Anonymus class

Una clase anonima es muy diferente a nested class debido a que no es escrita 
como una declaracion del todo. Es escrita como una expresion 

Una clase anonima es dependiente de una interface para implentar o una clase a extender, 
ambos tienen que contener la accesibilidad de los metodos, los cuales la clase anonima
va a implementar y ejecutar.

La expresion puede asignarse a una variable local, instancia de una variable o static class variable 

La expresion puede pasarse como parametro de un metodo siempre en cuando sea del mismo tipo 

En una clase anonima se puede declarar :

1. fields(no pueden ser static amenos que sean constantes)
2. Extra methods ( metodos que no estan en la clase o interface que implementa)
La estructura de una clase anonima puede ser una de las siguientes:
3. instance initializer
4. local variable class

En una clase anonima no se puden declarar 

1. constructores
2. static fields (enum, interfaces, static class)
3. static initilizer
4. static methods
5. member interfaces

```java
/*
The Learn Programming Academy
Java SE 11 Developer 1Z0-819 OCP Course - Part 2
Section 1: Java Fundamentals 
Topic:  Anonymous classes
*/

abstract class AnAbstractClass {
    int i;

    protected abstract void doSomething();
}

interface Doable {
    void doSomething();
}

public class AnonymousInnerExample {
    String name = "Default";

    public void testAnonymous() {

        // Declare a variable of type AnAbstractClass and immediately
        // define the class body
        AnAbstractClass a = new AnAbstractClass() {
            // Implementing the abstract method on AnAbstractClass
            public void doSomething() {
                System.out.println("Anonymous AnAbstractClass " +
                        "will doSomething with " + i);
            }
        };  // Declaration occurs in an expression and must end with ';'

        a.doSomething();

        // Anonymous class extending Object
        Object b = new Object() {
            public String toString() {
                return "Anonymous object";
            }
        };

        System.out.println(b);

        // Anonymous class implements interface Doable
        new Doable() {
            public void doSomething() {
                System.out.println("Anonymous Doable " +
                        "will doSomething with " + b);
            }
        }.doSomething();

        // If you want to pass parameters to your anonymous
        // class, you can extend abstract class using a local
        // class
        abstract class ConcreteClass extends AnAbstractClass {
            ConcreteClass(int i) {
                this.i = i;
            }
        }

        // This example shows an anonymous class created with an
        // instance initializer and passing a value to the constructor
        AnAbstractClass c = new ConcreteClass(5) {
            {
                i = 2 * this.i;
            }

            public void doSomething() {
                System.out.println("Anonymous AnAbstractClass " +
                        "will doSomething with " + i);
            }
        };
        c.doSomething();

    }

    public static void main(String[] args) {
        new AnonymousInnerExample().testAnonymous();
    }
}



```


Mas reglas sobre clases anonimas

1. La instancia de una clase anonym solo puede llamar a los metodos de la interface  
que implementa o de la clase que hereda

2. en la declaracion de los metodos no tienen que se publicos, solo tener la
accesiblilidad correcta

3. pueden implementar cualquier clase

4. no requiere asignar a una variable para invocar a un metodo,  
puede realizarse directamente en la instancia new



```java


public class AnonymousSecondExample {

    // You can define a private interface in your class
    private interface Summable {
        double getTotal(float[] data);
    }

    // A pass through method to invoke getTotal method on an object 
    // that implements Summable interface
    public static void invokeSummable(Summable summable,
                                      float[] data) {
        System.out.println("Total = " + String.format("%.2f",
                summable.getTotal(data)));
    }

    public static void main(String[] args) {

        // Note that we are passing an anonymous class expression as a 
        // parameter to the invokeSummable method .
        invokeSummable(
                new Summable() {  // Begin anonymous class expression

                    // implement Summable.getTotal(float[]) method
                    public double getTotal(float[] values) {
                        double total = 0.0;
                        for (float value : values) total += value;
                        return total;
                    } // end of getTotal method

                }  // End of anonymous class expression
                , new float[]{12.4f, 13.4f, 5f, 12.0f, 11f, 7.5f}
        );  // End of method invocation
    }
}


```

Beneficios de usar las nested class

1. Una forma para agrupar clases logicamente y un lugar para facilitar  
su uso

2. aumentar el encapsulamiento(el ocultamiento y la proteccion de la informacion es mejor)

3. El codigo es mas legible ya que la clase se encuentra en la misma  
coleccion de codigo 

4. Codigo es considerablemente mas mantenible, la proximidad de las clases y el  
codigo que las utiliza hace mas facil debugear y mantener


#### Cuando usar nested class


1. <span style="color: #e9c46a;">static nested class</span>: cuando se quiera incrustar campos estaticos  
 y comportamiento usando un estilo anidado. Donde el uso de miembros estaticos  
 en una estructura de herencia es logica.

2. <span style="color: #e9c46a;">Inner class</span>: Es el miembro de una clase y pueden declararse variables de su tipo  
Usar cuando la inner class es dependiente de la clase que la contiene para algo mas complejo.  

Usar cuando solo la clase enclosing la necesita como es el motor a un carro


3. <span style="color: #e9c46a;">local class</span>: Es tipicamente definida en el cuerpo de un metodo y es generalmente una forma  
de mantener el estado y comportamiento en un conjunto de informacion por un corto tiempo

si solo se va a ejecutar un conjunto de comportamiento , una clase anonima es mejor opcion

si se quiere mantener temporalmente el estado esperar la ejecucion del comportamiento o el estado 
esta cambiando la mejor opcion es sera usar local class

4. <span style="color: #e9c46a;">anonymus class</span>:  una forma de customizar el comportamiento de una forma muy rapida sin el desgaste  
de la declaracion de una clase, generalmente se quiere customizar el codigo de un metodo definido en   
una interface o un metodo abstracto

El manejo de eventos en javaFx es un ejemplo del uso continuo de clases anonimas

## Enums

Los enums son la lista de elementos de un conjunto finito, En java el tipo enum prove soporte para definir un conjunto de constantes predefinidas y metodos para accederlas asi como  
referenciarlas por los nombres constantes  

Un ejemplo de enumeracion son el conjunto de dias de la semana  
o los meses de una anio  

1. Enum una clase, implicitamente extiende de la clase abstracta 
java.lang.Enum  

2. No puede ser extendida o heredar explicitamente(usando extends ) de otra clase  en caso de hacerlo genera error de  
copilacion

3. La definicion se realiza: 

```java

enum BinaryValues{}

```

4. Las reglas de los modificadores de acceso son las mismas 
que aplican a las clases

5. si el enum no se encuentra dentro de un enclosed type  solo se puede usar public,package(implicito),protected no se puede usar private  

6. todos los modificadores de acceso son validos si se encuentra dentro de un eclosed type



7. === dentro de una nested class === se puede declarar static pero es redundate ya que es implicitamente static

8. === dentro de una nested class === se puede declar final  
pero es redundate ya que es implicitamente final  a menos 

9. genera error de compilacion generar un enum como abstract  

10. la forma mas basica de declarar el cuerpo de un enum  

```java

enum DaysOfWeek{
    SUNDAY,MONDAY,TUESDAY,WEDNESDAY,THURSDAY,FRIDAY,SATURDAY
}

```
Ejemplo 1

```java

enum DaysOfWeek{
    SUNDAY,MONDAY,TUESDAY,WEDNESDAY,THURSDAY,FRIDAY,SATURDAY
}

```




## Exception Handling and Assertions

## Java Interfaces  

## Generic and Collections  

## Functional interfaces and lambda expresions

## Built-in Functional interfaces
## Java Stream API
## Lambda Operation on Stream

## Migration to modular application

## Service in modular application

## Concurrency

## Parerall Stream

## I/O (Fundamentals and NIO2)

## Secure Coding in Java SE Application

## Localization

## Annotation

## 
