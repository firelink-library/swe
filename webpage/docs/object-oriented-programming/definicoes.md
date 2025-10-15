---
sidebar_position: 1
slug: /poo/definicoes
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Definições para POO

Pessoal para iniciarmos nossos estudos sobre a utilização do paradigma de Orientação Objeto, precisamos definir alguns conceitos primeiro. 

## 1. Por que a Programação Orientada a Objetos foi criada

A POO foi criada para resolver os problemas de complexidade, manutenção e reuso de código que surgiam nos sistemas desenvolvidos com paradigmas anteriores, especialmente o paradigma procedural (baseado em funções e procedimentos).

As principais motivações foram:
- **Modelar o mundo real de forma mais natural:** Permitir que o software representasse entidades e comportamentos do mundo real por meio de objetos com atributos e métodos, aproximando o raciocínio humano da lógica computacional.
- **Reutilizar código com segurança:** Através da herança e do polimorfismo, era possível reaproveitar funcionalidades e estender sistemas sem reescrever tudo do zero.
- **Facilitar manutenção e evolução:** O encapsulamento permitiu isolar partes do código, reduzindo o impacto de mudanças e tornando os sistemas mais robustos e modulares.
- **Gerenciar sistemas cada vez maiores:** À medida que os projetos de software cresceram em tamanho e complexidade (principalmente em engenharia, simulações e sistemas administrativos), a POO trouxe organização e modularidade, facilitando o trabalho em equipe e o versionamento.

Legal, temos agora uma motivação clara que: este paradigma foi criado para lidar com a complexidade dos sistemas.

Recomendo muito o vídeo a seguir:

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycvaECDc31w?si=MIxbTL5VyPP6T36e" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}></iframe>
  <br/>

## 2. Algumas definições

Legal, agora que definimos que a POO é um paradigma (um conjunto de convenções para escrever sistemas e códigos). Vamos compreender alguns dos elementos que utilizamos. É importante destacar um ponto aqui, essas definições não são vinculadas apenas a uma linguagem, elas são uma forma de representar os componentes dos sistemas.

Os elementos que compõem o sistema são chamados de ***`Classes`***. As classes são componentes que representam elementos ou partes do sistema. As classes podem ser utilizadas para descrever desde uma pessoa, ou mesmo um sistema de pagamento.

Beleza, mas agora que temos definido que as classes vão definir as partes do sistema, como nós definimos as classes? Essa é uma ótima pergunta! Nossas classes são definidas utilizando ***`Atributos`*** e ***`Métodos`***. Os atributos definem tudo que a classe ***`CONHECE`***, são as variáveis locais da classe. Os métodos são os comportamentos que uma classe ***`PODE FAZER`***, são as funções definidas na classe.

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRNw0pjxZpaIDUEgXvwHVUtLoS6t70Zc915vA&s" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>

> *Murilão calma ae. Gostei, mas agora tem 3 coisas ai para gente entender não? Classes, Atributos e Métodos? Isso mesmo?*

Denovo, uma pergunta excelente! Vamos tentar definir esses elementos com mais detalhes.

### 2.1 Classes

As **classes** são a base da Programação Orientada a Objetos. Elas funcionam como **modelos ou moldes** que descrevem como determinado elemento do sistema deve se comportar e quais características ele deve possuir. Ela é como a **planta de uma casa**: ela não é a casa em si, mas define tudo o que a casa terá (quartos, portas, janelas). No código, isso significa que a classe define **atributos** (características) e **métodos** (ações).  

<Tabs>
<TabItem value="java" label="Java">
  
```java title="Carro.java" showLineNumbers
public class Carro {
    private int quantidadeBancos;
    private float capacidadeTanqueCombustivel;
    private String fabricante;
    private String nome;
}
```
</TabItem> 
<TabItem value="python" label="Python">

```python title="Carro.py" showLineNumbers
class Carro:
    def __init__(self, quantidade_bancos: int, capacidade_tanque_combustivel: float, fabricante: str, nome: str):
        self.__quantidade_bancos = quantidade_bancos
        self.__capacidade_tanque_combustivel = capacidade_tanque_combustivel
        self.__fabricante = fabricante
        self.__nome = nome

```
</TabItem> 
<TabItem value="kotlin" label="Kotlin">

```kotlin title="Carro.kt" showLineNumbers

class Carro(
    private var quantidadeBancos: Int,
    private var capacidadeTanqueCombustivel: Float,
    private var fabricante: String,
    private var nome: String
) {
}

```
</TabItem> 
</Tabs> 

Eita calma ai, aqui apareceram algumas coisas que falamos só em definição: os atributos.

### 2.2 Atributos

Os **atributos** são as **características** ou **propriedades** de uma classe. Eles representam o que a classe **conhece** — ou seja, quais informações ela guarda. Pense assim: se a classe for um “molde”, os atributos são os **dados que personalizam cada instância** (objeto) criada a partir dela.

Exemplo:

<Tabs>
<TabItem value="java" label="Java">
  
```java title="Carro.java" showLineNumbers
public class Carro {
    private int quantidadeBancos;
    private float capacidadeTanqueCombustivel;
    private String fabricante;
    private String nome;
}
```
</TabItem> 
<TabItem value="python" label="Python">

```python title="Carro.py" showLineNumbers
class Carro:
    def __init__(self, quantidade_bancos: int, capacidade_tanque_combustivel: float, fabricante: str, nome: str):
        self.__quantidade_bancos = quantidade_bancos
        self.__capacidade_tanque_combustivel = capacidade_tanque_combustivel
        self.__fabricante = fabricante
        self.__nome = nome

```
</TabItem> 
<TabItem value="kotlin" label="Kotlin">

```kotlin title="Carro.kt" showLineNumbers

class Carro(
    private var quantidadeBancos: Int,
    private var capacidadeTanqueCombustivel: Float,
    private var fabricante: String,
    private var nome: String
) {
}

```
</TabItem> 
</Tabs> 

Neste caso:

* `quantidadeBancos`, `capacidadeTanqueCombustivel`, `fabricante` e `nome` são atributos do carro.
* Cada carro que criamos (cada objeto) terá valores próprios para esses atributos.

E quanto os comportamentos?

### 2.3 Métodos

Os **métodos** são as **ações** que um objeto pode realizar — os **comportamentos** da classe. Eles definem o que o objeto **pode fazer**. Métodos são como **funções associadas a um tipo específico de dado** (a classe). Por exemplo, se temos uma classe `Carro`, ela pode ter métodos como `acelerar()`, `frear()` ou `ligarMotor()`.

<Tabs>
<TabItem value="java" label="Java">
  
```java title="Carro.java" showLineNumbers
public class Carro {
    private int quantidadeBancos;
    private float capacidadeTanqueCombustivel;
    private String fabricante;
    private String nome;
    private boolean motorLigado = false;
    private float velocidade = 0f;

    public void ligarMotor() {
        motorLigado = true;
        System.out.println("Motor ligado.");
    }

    public void acelerar(float delta) {
        if (!motorLigado) {
            System.out.println("Não é possível acelerar com o motor desligado.");
            return;
        }
        velocidade += delta;
        System.out.println("Acelerou para " + velocidade + " km/h");
    }

    public void frear(float delta) {
        velocidade = Math.max(0f, velocidade - delta);
        System.out.println("Freou para " + velocidade + " km/h");
    }
}
```
</TabItem> 
<TabItem value="python" label="Python">

```python title="Carro.py" showLineNumbers
class Carro:
    def __init__(self, quantidade_bancos: int, capacidade_tanque_combustivel: float, fabricante: str, nome: str):
        self.__quantidade_bancos = quantidade_bancos
        self.__capacidade_tanque_combustivel = capacidade_tanque_combustivel
        self.__fabricante = fabricante
        self.__nome = nome
        self.__motor_ligado: bool = False
        self.__velocidade: float = 0.0

    def ligar_motor(self) -> None:
        self.__motor_ligado = True
        print("Motor ligado.")

    def acelerar(self, delta: float) -> None:
        if not self.__motor_ligado:
            print("Não é possível acelerar com o motor desligado.")
            return
        self.__velocidade += delta
        print(f"Acelerou para {self.__velocidade} km/h")

    def frear(self, delta: float) -> None:
        self.__velocidade = max(0.0, self.__velocidade - delta)
        print(f"Freou para {self.__velocidade} km/h")
```

</TabItem> 
<TabItem value="kotlin" label="Kotlin">

```kotlin title="Carro.kt" showLineNumbers
class Carro(
    private var quantidadeBancos: Int,
    private var capacidadeTanqueCombustivel: Float,
    private var fabricante: String,
    private var nome: String
) {
    private var motorLigado: Boolean = false
    private var velocidade: Double = 0.0

    fun ligarMotor() {
        motorLigado = true
        println("Motor ligado.")
    }

    fun acelerar(delta: Double) {
        if (!motorLigado) {
            println("Não é possível acelerar com o motor desligado.")
            return
        }
        velocidade += delta
        println("Acelerou para $velocidade km/h")
    }

    fun frear(delta: Double) {
        velocidade = (velocidade - delta).coerceAtLeast(0.0)
        println("Freou para $velocidade km/h")
    }
}
```
</TabItem> 
</Tabs>

### 2.4 Objetos (Eita, esse é novo!)

Os **objetos** são as **instâncias concretas** das classes. Se a classe é o molde, o objeto é o produto final moldado. Enquanto a classe define o “tipo” e o “comportamento”, o objeto é **uma representação real em execução**, com valores próprios nos seus atributos.

Exemplo:

<Tabs groupId="lang" defaultValue="java" values={[
  {label: 'Java', value: 'java'},
  {label: 'Python', value: 'python'},
  {label: 'Kotlin', value: 'kotlin'},
]}>
<TabItem value="java">

```java title="Carro.java" showLineNumbers
public class Carro {
    public String modelo;
    public float velocidade;

    public void acelerar() {
        velocidade += 10f;
        System.out.println("Acelerou para " + velocidade + " km/h");
    }
}
```

```java title="Main.java"
public class Main {
    public static void main(String[] args) {
        Carro carro1 = new Carro();
        carro1.modelo = "Fusca";
        carro1.velocidade = 0;
        carro1.acelerar();
    }
}
```
</TabItem> 
<TabItem value="python">

```python title="Carro.py"
class Carro:
    def __init__(self):
        self.modelo: str = ""
        self.velocidade: float = 0.0

    def acelerar(self) -> None:
        self.velocidade += 10.0
        print(f"Acelerou para {self.velocidade} km/h")

if __name__ == "__main__":
    carro1 = Carro()
    carro1.modelo = "Fusca"
    carro1.velocidade = 0.0
    carro1.acelerar()
```
</TabItem> 
<TabItem value="kotlin">

```kotlin title="Carro.kt"
class Carro {
    var modelo: String = ""
    var velocidade: Double = 0.0

    fun acelerar() {
        velocidade += 10.0
        println("Acelerou para $velocidade km/h")
    }
}
```
```kotlin title="Main.kt"
fun main() {
    val carro1 = Carro()
    carro1.modelo = "Fusca"
    carro1.velocidade = 0.0
    carro1.acelerar()
}
```
</TabItem> 
</Tabs> 

Neste caso:

* `carro1` é um **objeto** (ou instância) da classe `Carro`;
* Ele tem valores próprios para `modelo` e `velocidade`;
* Pode executar métodos definidos na classe, como `acelerar()`.

💡 **Resumindo:**

| Conceito     | O que representa   | Exemplo                             |
| ------------ | ------------------ | ----------------------------------- |
| **Classe**   | Molde ou modelo    | `Carro`                             |
| **Atributo** | Características    | `cor`, `velocidade`                 |
| **Método**   | Ações              | `acelerar()`, `frear()`             |
| **Objeto**   | Instância concreta | `carro1` criado a partir de `Carro` |

---

:::tip[Sugestão]

> Pense que quando você programa em POO, está **ensinando o computador a entender o mundo como um conjunto de coisas (objetos)** que têm **características (atributos)** e **fazem coisas (métodos)**. É isso que torna a POO tão poderosa para modelar sistemas complexos!

:::

:::danger[Atenção]

Pessoal a partir daqui eu vou ficar com os exemplos só em Java por enquanto, depois adiciono os demais.

<img src="https://rockmeon.wordpress.com/wp-content/uploads/2012/04/java1.jpg" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>

:::

## 3. Sugestão de Exercícios

Aqui vou deixar alguns exercícios para fixar alguns dos conceitos que colocamos aqui.

### 3.1 Criando sua primeira classe

Crie uma classe chamada Pessoa que possua os seguintes atributos:
- nome (String)
- idade (int)
- altura (float)

E um método chamado apresentar() que imprima uma mensagem como:

> "Olá, meu nome é João, tenho 25 anos e 1.75m de altura."

Depois, crie um objeto da classe e chame o método apresentar().

### 3.2 Adicionando comportamento

Crie uma classe ContaBancaria com os atributos:
- titular (String)
- saldo (float)

Implemente os métodos:
- depositar(float valor) — adiciona o valor ao saldo.
- sacar(float valor) — subtrai o valor se houver saldo suficiente.
- mostrarSaldo() — exibe o saldo atual do titular.

Crie um objeto, faça um depósito e um saque e mostre o saldo final.

### 3.3 Desafio prático

Implemente um pequeno sistema de gerenciamento de Funcionários.
Cada funcionário deve ter:
- nome, cargo, salario
- um método aumentarSalario(percentual)
- um método descricao() que exiba suas informações.

Crie 3 objetos diferentes (por exemplo, gerente, técnico, estagiário) e chame os métodos de aumento e descrição para cada um.