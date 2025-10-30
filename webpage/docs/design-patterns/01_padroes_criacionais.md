---
sidebar_position: 2
slug: /design-patterns/criacionais
---

# Padrões Criacionais 

Os padrões criacionais (*Creational Patterns*) são utilizados para criação de objetos e encapsulam a lógica de instanciação. Isso significa que eles tem como principal dor resolver como os elementos são criados para atender a solução.

Alguns métodos que podem ser observados aqui (retiradas do [Refactoring Guru](https://refactoring.guru/design-patterns/creational-patterns)):

- **Factory Method**: traz uma interface para criar objetos em uma superclasse, mas permite que as subclasses possam alterar os tipos dos objetos que são criados;
- **Abstract Factory**: cria uma família de objetos sem precisar especificar suas classes concretas;
- **Builder**: cria objetos complexos um passo por vez. Este padrão permite criar diferentes representações de um objeto utilizando a mesma construção;
- **Prototype**: permite criar cópias de um objeto sem criar mais acoplamento entre as classes;
- **Singleton**: garante que uma classe possui apenas uma instância, com acesso global a está instância.

Agora vamos analisar alguns destes padrões.

## Padrão Factory Method

Este padrão fornece uma forma de criar objetos utilizando uma superclasse. O padrão Factory Method sugere que você substitua chamadas diretas de construção de objetos (usando o operador new ou o equivalente em uma outra linguagem) por chamadas para um método fábrica especial. 

Os objetos ainda são criados através do operador new (ou equivalente), mas esse está sendo chamado de dentro do método fábrica. Objetos retornados por um método fábrica geralmente são chamados de produtos.

> `Mas Murilo, por que isso?`

Ótima pergunta! Utilizamos esse método para que possamos modificar nosso código e adicionar mais elementos deste que a fabrica consegue criar, sem causar modificações no restante do comportamento do código que está implementado. Desta maneira, nosso código pode implementar outras funcionalidades sem ter que fazer mudanças muito grandes no restante de sua implementação.


### Exemplo de implementação em Java

**Ideia:** a superclasse define um método fábrica (`createTransport`) que as subclasses implementam para decidir **qual** produto concreto criar.

**Estrutura (papéis):**

* `Creator` (Logistics): declara `createTransport()` e um método de alto nível que usa o produto.
* `ConcreteCreator` (RoadLogistics, SeaLogistics): implementa `createTransport()`.
* `Product` (Transport): interface comum.
* `ConcreteProduct` (Truck, Ship): implementações do produto.

```java
// Product
interface Transport {
    void deliver(String cargo);
}

// Concrete Products
class Truck implements Transport {
    @Override
    public void deliver(String cargo) {
        System.out.println("Entregando por estrada: " + cargo);
    }
}

class Ship implements Transport {
    @Override
    public void deliver(String cargo) {
        System.out.println("Entregando por mar: " + cargo);
    }
}

// Creator
abstract class Logistics {
    public void planDelivery(String cargo) {
        Transport t = createTransport();
        // ... outras lógicas antes/depois ...
        t.deliver(cargo);
    }
    protected abstract Transport createTransport(); // Factory Method
}

// Concrete Creators
class RoadLogistics extends Logistics {
    @Override
    protected Transport createTransport() {
        return new Truck();
    }
}

class SeaLogistics extends Logistics {
    @Override
    protected Transport createTransport() {
        return new Ship();
    }
}

// Uso
public class FactoryMethodDemo {
    public static void main(String[] args) {
        Logistics l1 = new RoadLogistics();
        l1.planDelivery("Caixas");

        Logistics l2 = new SeaLogistics();
        l2.planDelivery("Contêineres");
    }
}
```

**Quando usar:** quando você quer **desacoplar** o código cliente do **tipo concreto** de objeto criado e permitir que subclasses mudem a família de produtos sem alterar o algoritmo.

**Prós:** reduz acoplamento, facilita extensão.
**Contras:** pode aumentar o número de classes.

:::info[Simple Factory]

Existe uma variação ao Factory Method, chamado de Simple Factory. Ele concentra todos os construtores em uma classe principal.

```java
// Product
interface Transport { void deliver(String cargo); }

// Concrete Products
class Truck implements Transport {
    public void deliver(String cargo) { System.out.println("Estrada: " + cargo); }
}
class Ship implements Transport {
    public void deliver(String cargo) { System.out.println("Mar: " + cargo); }
}

// Simple Factory
class TransportFactory {
    static Transport create(String mode) {
        return switch (mode) {
            case "road" -> new Truck();
            case "sea"  -> new Ship();
            default -> throw new IllegalArgumentException("Modo inválido: " + mode);
        };
    }
}

// Cliente
public class SimpleFactoryDemo {
    public static void main(String[] args) {
        Transport t = TransportFactory.create("road"); // construtor indireto
        t.deliver("Caixas");
    }
}

```

Embora não um padrão do GoF, é bastante utilizado. Quando utilizar cada um:

- **Factory Method (herança):** quando você quer permitir que subclasses decidam o produto (polimorfismo) e, muitas vezes, varie o algoritmo de alto nível dentro do creator. Facilita extensão por novas variantes.

- **Simple Factory:** quando você só quer centralizar a criação em um ponto e manter o cliente limpo. É simples e direto.

:::

---

## Padrão Abstract Factory

Cria **famílias** de objetos relacionados (produtos) sem expor suas classes concretas. É útil quando você precisa garantir que produtos **compatíveis** sejam usados juntos.

**Estrutura (papéis):**

* `AbstractFactory` (GUIFactory): cria famílias (ex.: `createButton`, `createCheckbox`).
* `ConcreteFactory` (WinFactory, MacFactory): cria variantes compatíveis.
* `AbstractProduct` (Button, Checkbox): interfaces dos produtos.
* `ConcreteProduct` (WinButton/MacButton, WinCheckbox/MacCheckbox): implementações por plataforma.
* Cliente usa apenas interfaces abstratas.

```java
// Abstract Products
interface Button { void render(); }
interface Checkbox { void check(); }

// Concrete Products
class WinButton implements Button {
    public void render() { System.out.println("Renderizando botão Windows"); }
}
class MacButton implements Button {
    public void render() { System.out.println("Renderizando botão Mac"); }
}
class WinCheckbox implements Checkbox {
    public void check() { System.out.println("Checkbox Windows marcada"); }
}
class MacCheckbox implements Checkbox {
    public void check() { System.out.println("Checkbox Mac marcada"); }
}

// Abstract Factory
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete Factories
class WinFactory implements GUIFactory {
    public Button createButton() { return new WinButton(); }
    public Checkbox createCheckbox() { return new WinCheckbox(); }
}
class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}

// Client
class Application {
    private final Button button;
    private final Checkbox checkbox;

    Application(GUIFactory factory) {
        this.button = factory.createButton();
        this.checkbox = factory.createCheckbox();
    }

    void renderUI() {
        button.render();
        checkbox.check();
    }
}

// Uso
public class AbstractFactoryDemo {
    public static void main(String[] args) {
        GUIFactory factory = System.getProperty("os.name").toLowerCase().contains("mac")
                ? new MacFactory()
                : new WinFactory();

        Application app = new Application(factory);
        app.renderUI();
    }
}
```

**Quando usar:** quando o sistema deve ser **independente** de como seus produtos são criados e quando você quer **impor compatibilidade** entre produtos.
**Prós:** consistência entre produtos; troca de famílias é simples.
**Contras:** adicionar um **novo tipo de produto** (ex.: `Slider`) exige mudar **todas** as fábricas.

---

## Padrão Builder

Constrói objetos complexos passo a passo. O mesmo processo (o “diretor”) pode criar representações diferentes (builders diferentes).

**Estrutura (papéis):**

* `Builder` (CarBuilder): define passos de construção.
* `ConcreteBuilder` (CityCarBuilder, SportCarBuilder): implementa passos.
* `Director` (opcional): orquestra a ordem dos passos.
* `Product` (Car): objeto complexo final.

```java
// Product
class Car {
    String engine;
    int seats;
    boolean gps;

    @Override
    public String toString() {
        return "Car{engine='" + engine + "', seats=" + seats + ", gps=" + gps + "}";
    }
}

// Builder
interface CarBuilder {
    CarBuilder engine(String type);
    CarBuilder seats(int qty);
    CarBuilder gps(boolean hasGps);
    Car build();
}

// Concrete Builders (fluent)
class CityCarBuilder implements CarBuilder {
    private final Car car = new Car();
    public CarBuilder engine(String type) { car.engine = type; return this; }
    public CarBuilder seats(int qty) { car.seats = qty; return this; }
    public CarBuilder gps(boolean hasGps) { car.gps = hasGps; return this; }
    public Car build() { return car; }
}

class SportCarBuilder implements CarBuilder {
    private final Car car = new Car();
    public CarBuilder engine(String type) { car.engine = type; return this; }
    public CarBuilder seats(int qty) { car.seats = qty; return this; }
    public CarBuilder gps(boolean hasGps) { car.gps = hasGps; return this; }
    public Car build() { return car; }
}

// Director (opcional)
class CarDirector {
    public Car makeCityCar(CarBuilder b) {
        return b.engine("1.0 Flex").seats(5).gps(true).build();
    }
    public Car makeSportCar(CarBuilder b) {
        return b.engine("V8").seats(2).gps(false).build();
    }
}

// Uso
public class BuilderDemo {
    public static void main(String[] args) {
        CarDirector director = new CarDirector();
        Car city = director.makeCityCar(new CityCarBuilder());
        Car sport = director.makeSportCar(new SportCarBuilder());
        System.out.println(city);
        System.out.println(sport);

        // Sem diretor, direto no builder (também válido):
        Car custom = new CityCarBuilder().engine("1.6").seats(4).gps(true).build();
        System.out.println(custom);
    }
}
```

**Quando usar:** quando a criação tem **muitos passos/opções** ou quando diferentes **representações** do mesmo produto são necessárias.
**Prós:** separa construção de representação; API fluente legível.
**Contras:** mais classes; pode ser demais para objetos simples.

---

## Padrão Prototype

Cria novos objetos copiando (clonando) instâncias existentes (protótipos), em vez de instanciar diretamente. Útil quando criar do zero é **caro** ou quando queremos **duplicar estados complexos**.

**Estrutura (papéis):**

* `Prototype` (Cloneable/clone customizado): define a operação de cópia.
* `ConcretePrototype` (Circle, Rectangle): implementa a clonagem.
* Cliente chama `clone()` no protótipo.

```java
// Prototype base
abstract class Shape {
    int x, y;
    String color;

    Shape() {}

    // "Construtor de cópia"
    Shape(Shape target) {
        if (target != null) {
            this.x = target.x;
            this.y = target.y;
            this.color = target.color;
        }
    }

    public abstract Shape clone();
}

class Circle extends Shape {
    int radius;

    Circle() {}
    Circle(Circle target) {
        super(target);
        if (target != null) this.radius = target.radius;
    }

    @Override
    public Shape clone() {
        return new Circle(this);
    }

    @Override
    public String toString() {
        return "Circle{x=" + x + ", y=" + y + ", color=" + color + ", r=" + radius + "}";
    }
}

class Rectangle extends Shape {
    int width, height;

    Rectangle() {}
    Rectangle(Rectangle target) {
        super(target);
        if (target != null) {
            this.width = target.width;
            this.height = target.height;
        }
    }

    @Override
    public Shape clone() {
        return new Rectangle(this);
    }

    @Override
    public String toString() {
        return "Rectangle{x=" + x + ", y=" + y + ", color=" + color +
               ", w=" + width + ", h=" + height + "}";
    }
}

// Uso
public class PrototypeDemo {
    public static void main(String[] args) {
        Circle c1 = new Circle();
        c1.x = 10; c1.y = 20; c1.radius = 15; c1.color = "red";

        Circle c2 = (Circle) c1.clone(); // cópia
        c2.x = 30;

        System.out.println(c1);
        System.out.println(c2);
    }
}
```

**Quando usar:** quando a criação é **custosa** (ex.: carregamento, cálculos) ou quando queremos **duplicar** uma configuração inicial complexa.
**Prós:** evita `new` repetitivo; copia estados facilmente.
**Contras:** cópias **profundas** podem ser complexas; atenção a referências mutáveis.

---

## Padrão Singleton

Garante **uma única instância** de uma classe e **ponto de acesso global**. Em Java moderno, as formas mais seguras e simples são **`enum`** ou o **idioma do Holder**.

### Opção 1: `enum` (thread-safe e à prova de serialização/reflexão)

```java
public enum AppConfig {
    INSTANCE;

    // Estado global controlado
    private String baseUrl = "https://api.exemplo.com";
    public String getBaseUrl() { return baseUrl; }
    public void setBaseUrl(String url) { this.baseUrl = url; }

    // Métodos de negócio
    public void log(String msg) {
        System.out.println("[LOG] " + msg);
    }
}

// Uso
class SingletonEnumDemo {
    public static void main(String[] args) {
        AppConfig.INSTANCE.log("Iniciando aplicação");
        System.out.println(AppConfig.INSTANCE.getBaseUrl());
    }
}
```

### Opção 2: Holder Idiom (lazy, thread-safe, sem sincronização explícita)

```java
public class ConnectionPool {
    private ConnectionPool() { /* inicializações caras */ }

    private static class Holder {
        private static final ConnectionPool INSTANCE = new ConnectionPool();
    }

    public static ConnectionPool getInstance() {
        return Holder.INSTANCE;
    }

    public void acquire() {
        System.out.println("Pegando conexão...");
    }
}

// Uso
class SingletonHolderDemo {
    public static void main(String[] args) {
        ConnectionPool pool = ConnectionPool.getInstance();
        pool.acquire();
    }
}
```

> Evite o Singleton “apressado” com `synchronized` e **double-checked locking** se não souber exatamente o que está fazendo — é fácil errar e desnecessário hoje.

**Quando usar:** quando **de fato** há um único recurso compartilhado (config, registrador, pool).
**Prós:** controle centralizado de acesso; única instância garantida.
**Contras:** pode virar **estado global** difícil de testar; risco de **acoplamento** e **efeito colateral** em larga escala.

---

## Dicas rápidas para ensino/avaliação

* **Factory Method x Abstract Factory:** FM decide **um tipo**; AF decide **uma família** de tipos compatíveis.
* **Builder x Factory:** Builder foca em **passos** e **configuração complexa**; Factory foca **qual classe** instanciar.
* **Prototype:** pense em **duplicar estado** (cópia), não em “construir do zero”.
* **Singleton:** use **com moderação**; prefira **injeção de dependência** quando possível.




