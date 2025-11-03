---
sidebar_position: 3
slug: /design-patterns/estruturais
---

# Padrões Estruturais

Os padrões estruturais explicam como construir objetos e classes em estruturas maiores, mantendo elas flexíveis e eficientes. Alguns dos padrões compreendidos aqui são:

- **Adapter**: permite criar objetos com interfaces incompatíveis possam colaborar;
- **Bridge**: permite criar uma classe grande em classes menores relacionadas com implementações que podem ser desenvolvidas de forma independente uma da outra;
- **Composite**: permite criar arvores de estrutura e trabalhar com elas como se fossem objetos individuais;
- **Decorator**: permite adicionar novos comportamentos a objetos colocando eles em objetos especiais que contem esses comportamentos;
- **Facade**: traz uma interface simplificada para um conjunto complexo de classes;
- **Flyweight**: permite ajustar mais objetos em memória RAM, compartilhando recursos comuns entre eles, mantendo apenas os dados de cada objeto;
- **Proxy**: permite criar um substituto para outro objeto. Um proxy permite criar acesso ao objeto original, permitindo modificar os elementos antes ou depois da requisição ao objeto.

## 0. Problema de Referência

Legal, agora que temos uma definição geral sobre os padrões estruturais, vamos ver como utilizar eles em nossos projetos e como conseguimos resolver diferentes problemas com sua aplicação.

Para nossa aplicação, vamos considerar um mini editor de desenho que renderiza formas e imagens na tela (importante: não serão renderizadas imagens efetivamente, só a critério de demonstração). Vamos verificar como poderia ser esse código:

```java
//Arquivo: MainBad.java
import java.util.*;

//CanvasBad.java
class CanvasBad {
    private List<Object> items = new ArrayList<>(); // Sem tipo claro


    public void add(Object o){ items.add(o); }


    // Renderização toda em if/else - difícil de escalar
    public void render(){
        for(Object o : items){
            if(o instanceof CircleBad){
                CircleBad c = (CircleBad)o;
                System.out.println("Desenhando Círculo com raio="+c.radius+" em modo RASTER fixo");
            } else if(o instanceof RectBad){
                RectBad r = (RectBad)o;
                System.out.println("Desenhando Retângulo w="+r.w+" h="+r.h+" em modo RASTER fixo");
            } else if(o instanceof HeavyImageBad){
                HeavyImageBad img = (HeavyImageBad)o;
                // Sempre carrega pesado, mesmo sem precisar
                img.load();
                System.out.println("Mostrando imagem pesada: "+img.path);
            }
        }
    }
}


//CircleBad.java
class CircleBad { public final int radius; public CircleBad(int r){ this.radius=r; } }


//RectBad.java
class RectBad { public final int w,h; public RectBad(int w,int h){ this.w=w; this.h=h; } }


//HeavyImageBad.java
class HeavyImageBad { 
    public final String path; public HeavyImageBad(String p){ this.path=p; }
    public void load(){ System.out.println("[LOAD] Carregando bitmap enorme de "+path); }
}


//MainBad.java
public class MainBad {
    public static void main(String[] args){
        CanvasBad c = new CanvasBad();
        c.add(new CircleBad(10));
        c.add(new RectBad(20, 15));
        c.add(new HeavyImageBad("/imgs/bg-huge.png"));
        c.render();
    }
}
```

---

## 1. Adapter

Esse padrão também é conhecido como `Adaptador` ou `Wrapper`. Seu objetivo é possibilitar que objetos que não possuem interfaces semelhantes possam colaborar entre si.

O `refactoring.guru` traz um exemplo que eu considero muito interessante aqui para compreendermos o problema que o **adapter** tenta resolver. Vamos pensar na seguinte situação: você tem uma aplicação que recebe dados no formato XML de um provedor (pense em uma aplicação que coleta dados de preços de ações). A aplicação consegue receber esses dados e processar eles para a aplicação. Agora, para expandir a aplicação, precisamos mandar os dados para uma outra aplicação, mas ela recebe apenas os dados no formato JSON. E agora o que fazer?

Uma abordagem que poderia resolver esse problema é criar um adaptador. Um adaptador é um objeto especial que converte a interface de um objeto para que outro objeto possa compreender ele. O processo consiste em *esconder* a complexidade da conversão acontecendo nos bastidores da aplicação. Desta forma, nenhum dos dois objetos que estão trocando informação tem conhecimento do processo de adequação que está acontecendo.

O seu fluxo de utilização é o seguinte:

1. O adaptador obtém uma interface compatível com os objetos já existentes;
2. Utilizando essa interface, o objeto existente pode chamar os métodos do adaptador;
3. Quando receber uma chama, o adaptador passa o pedido para segundo objeto, mas em um formato e ordem que o segundo objeto espera receber.

* Em alguns casos é possível ou necessário criar um adaptador de duas vias que pode converter as chamadas em ambas as direções (de qualquer um dos objetos para qualquer outro objeto).

A implementação utiliza o princípio da composição de objetos, o adaptador implementa a interface de um objeto e encobre o outro. Desta forma, o código do cliente não é acoplado a interface implementada. Estes adaptadores podem ser expandidos, sem quebrar implementações que já foram realizadas. Uma outra alternativa é realizar sua implementação por herança.

### Quando Utilizar

Utilizar a classe Adaptador quando você quer utilizar uma classe existente, mas sua interface não for compatível com o resto do seu código. Utilize o padrão quando você quer reutilizar diversas subclasses existentes que não possuam alguma funcionalidade comum que não pode ser adicionada à superclasse. Isso evita código duplicado nas subclasses ou nas classes filhas, levando ele para a interface do adaptador.

### Exemplo de Utilização

Vamos alterar nosso exemplo base para adicionar nele o nosso padrão **Adapter**.

```java
import java.util.*;

interface Drawable { 
    void draw(); 
}


class Circle implements Drawable {
    private final int r;
    public Circle(int r){ 
        this.r = r; 
        }
    @Override 
    public void draw(){ 
        System.out.println("[Circle] r="+r); 
        }
}

class Rect implements Drawable { 
    public final int w,h; 
    public Rect(int w,int h){ 
        this.w=w; this.h=h; 
        } 
    @Override
    public void draw(){
        System.out.println("[Rect] w="+ this.w + " h="+ this.h);
    }        
}

class LegacyImage {
    private final String path;
    public LegacyImage(String path){ 
        this.path=path; 
        }
    public void drawBitmap(){ 
        System.out.println("[Legacy] drawBitmap: "+path); 
        }
}



class LegacyImageAdapter implements Drawable {
    private final LegacyImage legacy;
    public LegacyImageAdapter(LegacyImage legacy){ 
        this.legacy = legacy; 
        }
    @Override 
    public void draw(){ 
        legacy.drawBitmap(); 
        }
}


// Cliente que depende apenas da ABSTRAÇÃO (Drawable)
class Canvas {
    private final List<Drawable> items = new ArrayList<>();
    public void add(Drawable d){ 
        items.add(d); 
        }
    public void render(){ 
        items.forEach(Drawable::draw); // <- Polimorfismo aqui
        } 
}


// Demonstração: objetos diferentes, mesma chamada .draw()
class Main {
    public static void main(String[] args){
        Canvas canvas = new Canvas();
        canvas.add(new Circle(12)); // nativo
        canvas.add(new Rect(3,4));
        canvas.add(new LegacyImageAdapter(new LegacyImage("logo.bmp"))); // adaptado
        canvas.render(); // chamadas .draw() são despachadas dinamicamente
    }
}
```

Aqui podemos observar diversas coisas acontecendo ao mesmo tempo. Primeiro temos que localizar a oportunidade de utilizar o padrão. A classe `LegacyImage` tem um formato diferente do restante das classes que podem ser desenhados no sistema. Como o padrão adapter foi utilizado? Um adaptador para a classe foi construído levando em consideração uma interface que todas as outras classes implementavam. Repare em um detalhe de implementação, a classe `LegacyImage` não foi alterada. As modificações aconteceram no adaptador. 

Vale destacar também, como ele trouxe uma oportunidade de melhoria para a construção do restante do código, que ficou mais enxuto e utilizando melhor o polimorfismo para sua representação.

:::danger[Sugestão de **pausa** para melhor compreensão]

Aqui eu sugiro fortemente uma pausa ou um bom tempo verificando como este padrão foi implementado. Isso pois estes padrões não foram construídos para terem seu valor percebido imediatamente. É importante realizar uma reflexão sobre sua utilização no código. Ele teve implicações e alterou a forma como o sistema funcionava. 

Refletir sobre esse processo e verificar se a complexidade adicionada ao sistema faz sentindo por sua implementação.

<img src="https://pa1.aminoapps.com/6748/97d0befbcc0a0b5126abdafe09cf81b6f0c1f842_00.gif" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>

:::

## 2. Bridge

O padrão **Bridge** permite separar grandes classes ou conjuntos de classes, em hierarquias intimamente ligadas, mas que a abstração e a implementação podem ser desenvolvidas de forma independente uma da outra. Pense agora em um conjunto de classes que representam formas geométricas e em um conjunto de classes que representam cores que essas formas podem assumir. O conjunto de combinações possível cresce de forma geométrica.

:::tip[Progressão Geométrica]

Uma progressão geométrica (abreviada como P.G.) é uma sequência numérica na qual cada termo, a partir do segundo, é igual ao produto do termo anterior por uma constante, chamada de razão da progressão geométrica. A razão é indicada geralmente pela letra **q** (inicial da palavra "quociente").

Alguns exemplos de progressão geométrica:
- 1,2,4,8,16,32,64,128,256,512,1024,2048, em que **q = 2** e **a1=1**;
- -3,9,-27,81,-243,729,-2187, em que **q=−3** e **a1=−3**;
- 7,7,7,7,7,7,7,7,7,7, em que **q=1** e **a1=7**;
- 3,0,0,0,0,0,0,0,0,0, em que **q=0** e **a1=3**.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4b/Geometric_progression_convergence_diagram.svg/350px-Geometric_progression_convergence_diagram.svg.png" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>
Diagrama mostrando uma série geométrica 1 + 1/2 + 1/4 + 1/8 + ... que converge para 2 - retirado de [link](https://pt.wikipedia.org/wiki/Progressão_geométrica).
:::

Seguindo esse exemplo, como podemos implementar nosso código? Essa é uma ótima pergunta! Podemos fazer essa implementação trazendo a relação entre as classes mais significativa. No exemplo acima, cada forma geométrica pode ser filhas da classe `Forma`, enquanto que a classe cor pode ter seus filhos como a class `Cor`.

Cada uma das partes recebe uma tarefa específica que deve ser realizada. Enquanto a abstração fornece a lógica de controle de alto nível para a o projeto, é o objeto de implementação que faz o trabalho por baixo dos panos.

### Quando Utilizar

Uma boa pedida para utilizar o padrão Bridge quando é necessário dividir e organizar uma classe monolítica que tem diversas formas de implementar uma mesma funcionalidade. Lembrem-se: quanto maior a classe, mais difícil é de compreender como ela funciona e mais complexo ainda é o tempo para fazer mudanças para ela.

Considere que uma hierarquia de classe existe quando uma dimensão da classe demanda um comportamento para ela. Isso significa que, quando um comportamento da classe pode ser construído com um outro conjunto de classes. Isso facilita muito as modificações e desacoplamento do projeto.

**ATENÇÃO:** O padrão Bridge é geralmente definido com antecedência, permitindo que as partes do sistema possam ser desenvolvidas de forma independente umas das outras. Já o Adapter é utilizado, em geral, quando as aplicações já existem.

### Exemplo de Utilização

Exemplo de utilização:

```java
interface Renderer { 
    void drawCircle(int radius); void drawRect(int w,int h); 
    }


class VectorRenderer implements Renderer {
    public void drawCircle(int radius){ 
        System.out.println("[Vector] Circle r="+radius); 
        }
    public void drawRect(int w,int h){ 
        System.out.println("[Vector] Rect "+w+"x"+h); 
        }
}


class RasterRenderer implements Renderer {
    public void drawCircle(int radius){ 
        System.out.println("[Raster] Circle r="+radius); 
        }
    public void drawRect(int w,int h){ 
        System.out.println("[Raster] Rect "+w+"x"+h); 
        }
}


abstract class Shape {
    protected final Renderer renderer;
    protected Shape(Renderer r){ this.renderer=r; }
    public abstract void draw();
    }


class Circle extends Shape {
    private final int radius;
    public Circle(Renderer r,int radius){ 
        super(r); 
        this.radius=radius; 
        }
    public void draw(){ 
        renderer.drawCircle(radius); 
        }
}


class Rect extends Shape {
    private final int w,h;
    public Rect(Renderer r,int w,int h){ 
        super(r); this.w=w; this.h=h; 
        }
    public void draw(){ 
        renderer.drawRect(w,h); 
        }
}



class Main {
    public static void main(String[] args){
        Renderer vec = new VectorRenderer();
        Renderer ras = new RasterRenderer();
        new Circle(vec, 10).draw();
        new Rect(ras, 20, 15).draw();
    }
}
```

## 3. Composite

O Composite permite compor objetos em estruturas em árvore (parte–todo) e tratar objetos individuais e composições de maneira uniforme.

### Quando usar

Você precisa tratar indivíduos e grupos de forma uniforme (em Python, pense numa lista com elementos que também são listas recursivamente). Use quando precisar representar hierarquias (camadas, grupos, cenas) e quer que o cliente chame o mesmo método (draw(), execute(), etc.) tanto para elementos simples quanto para grupos.

### Exemplo de Utilização

```java
import java.util.*;

interface Drawable { void draw(); }

// Folhas
class Circle implements Drawable { 
    private final int r; 
    public Circle(int r){this.r=r;} 
    public void draw(){ System.out.println("Circle r="+r); } 
}


class Rect implements Drawable { 
    private final int w,h; 
    public Rect(int w,int h){this.w=w;this.h=h;} 
    public void draw(){ 
        System.out.println("Rect "+w+"x"+h); 
        } 
}


// Composto
class Group implements Drawable {
    private final List<Drawable> children = new ArrayList<>();
    public Group add(Drawable d){ 
        children.add(d); 
        return this; 
        }
    public void draw(){ 
        children.forEach(Drawable::draw); 
        }
}

class Main {
    public static void main(String[] args){
        Group root = new Group()
        .add(new Circle(10))
        .add(new Rect(20,15));
        Group layer = new Group().add(new Circle(5)).add(new Circle(7));
        root.add(layer);
        root.draw(); // desenha tudo
    }
}
```

## 4. Decorator

O Decorator adiciona comportamentos dinamicamente sem herança, “embrulhando” um objeto com outro que implementa a mesma interface.

### Quando Utilizar

Quando quer “plugar” funcionalidades (borda, sombra, logging) em qualquer Drawable de forma combinável. Quando quiser combinar recursos opcionais (ex.: borda, sombra, cor, logging) sem explodir subclasses e mantendo abertura para composição em tempo de execução.

### Exemplo de Uso

```java
interface Drawable { void draw(); }


// Núcleo (component)
class Circle implements Drawable { 
    private final int r; 
    public Circle(int r){
        this.r=r;
    } 
    public void draw(){ 
        System.out.println("Circle r="+r); 
    } 
    
}

class Rect implements Drawable {
    private final int w,h;
    public Rect(int w, int h){
        this.w = w;
        this.h = h;
    }

    public void draw(){
        System.out.println("Rect w="+this.w+" h="+ this.h);
    }
}

// Decorator base
abstract class DrawableDecorator implements Drawable {
    protected final Drawable inner;
    protected DrawableDecorator(Drawable inner){ 
        this.inner = inner; 
    }
}


// Concretos
class BorderDecorator extends DrawableDecorator {
    public BorderDecorator(Drawable d){ 
        super(d); 
    }
    public void draw(){ 
        System.out.println("+ Borda 1px"); 
        inner.draw(); 
    }
}


class ShadowDecorator extends DrawableDecorator {
    public ShadowDecorator(Drawable d){ 
        super(d); 
    }
    public void draw(){ 
        System.out.println("+ Sombra suave"); 
        inner.draw(); 
    }
}


public class Main {
    public static void main(String[] args){
        Drawable base = new Circle(12);
        Drawable fancy = new BorderDecorator(new ShadowDecorator(base));
        fancy.draw();

        base = new Rect(3,4);
        fancy = new ShadowDecorator(base);
        fancy.draw();
    }
}
```

## 5. Facade

O Facade fornece uma interface simplificada para um subsistema complexo, escondendo detalhes e passos internos.

### Quando Utilizar

Você tem vários passos/objetos internos e quer expor uma API simples para o usuário (em Python, pense num módulo que encapsula detalhes feios). Quando o cliente não precisa conhecer várias classes/ordens de chamada internas e você quer entregar uma API coesa e fácil.

### Exemplo de Uso

```java
// Subsistema (oculto ao cliente)
class VectorEngine { 
    void line(){ 
        System.out.println("[Vector] line()"); 
    } 
}

class RasterEngine { 
    void blit(){ 
        System.out.println("[Raster] blit()"); 
    } 
}


class AssetLoader { 
    void load(String p){ 
        System.out.println("[Asset] load "+p); 
    } 
}


// Facade
class GraphicsFacade {
    private final VectorEngine vector = new VectorEngine();
    private final RasterEngine raster = new RasterEngine();
    private final AssetLoader assets = new AssetLoader();

    public void drawCircle(int r){ 
        vector.line(); 
        System.out.println("draw circle r="+r); 
    }
    public void drawImage(String path){ 
        assets.load(path); 
        raster.blit(); 
        System.out.println("image: "+path); 
    }
}


// Cliente
class Main {
    public static void main(String[] args){
        GraphicsFacade g = new GraphicsFacade();
        g.drawCircle(10);
        g.drawImage("/imgs/logo.png");
    }
}
```

## 6. Flyweight

### Quando Utilizar

Muitos objetos semelhantes consomem memória; parte do estado pode ser compartilhada (ex.: estilo/pen/cor). Em Python, lembre de interns de strings.

### Exemplo de Uso

```java
// Estado intrínseco compartilhado
class ShapeStyle {
    public final String stroke; 
    public final String fill;
    private ShapeStyle(String stroke,String fill){ 
        this.stroke=stroke; 
        this.fill=fill; 
    }
    // Factory com cache
    private static final java.util.Map<String,ShapeStyle> CACHE = new java.util.HashMap<>();
    public static ShapeStyle of(String stroke,String fill){
        String key = stroke+"|"+fill;
        return CACHE.computeIfAbsent(key, k -> new ShapeStyle(stroke, fill));
    }
}


// Objetos leves referenciam o estilo
class Circle { 
    private final int r; 
    private final ShapeStyle style;
    public Circle(int r, ShapeStyle s){ 
        this.r=r; 
        this.style=s; 
    }
    public void draw(){ 
        System.out.println("Circle r="+r+" style=("+style.stroke+","+style.fill+")"); 
    }
}


class Main {
    public static void main(String[] args){
        var red = ShapeStyle.of("#f00","none");
        var red2 = ShapeStyle.of("#f00","none");
        System.out.println("Mesma instância? "+(red==red2)); // true
        new Circle(5, red).draw();
        new Circle(10, red2).draw();
    }
}
```

## 7. Proxy

### Quando Utilizar

Você quer lazy-load, caching, segurança ou acesso remoto sem mudar o cliente.

### Exemplo de Uso

```java
// Sujeito real pesado
class HeavyImage implements Image {
    private final String path; 
    private boolean loaded=false;
    HeavyImage(String p){ 
        this.path=p; 
    }
    private void load(){ 
        if(!loaded){ 
            System.out.println("[LOAD] "+path); 
            loaded=true; 
        } 
    }
    public void show(){ 
        load(); 
        System.out.println("show "+path); 
    }
}


// Interface comum
interface Image { void show(); }


// Proxy virtual: carrega sob demanda e faz cache
class ImageProxy implements Image {
    private final String path; 
    private HeavyImage real;
    public ImageProxy(String p){ 
        this.path=p; 
    }
    public void show(){
        if(real==null){ 
            real = new HeavyImage(path); 
        }
        real.show();
    }
}


// Cliente
class Main {
    public static void main(String[] args){
        Image img = new ImageProxy("/imgs/bg-huge.png");
        System.out.println("— primeiro show —"); img.show();
        System.out.println("— segundo show —"); img.show(); // Sem recarregar
    }
}
```

## 8. Sugestões de Exercícios
