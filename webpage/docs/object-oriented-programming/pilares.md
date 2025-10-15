---
sidebar_position: 2
slug: /poo/pilares
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Pilares da POO

Vamos pegar um dos pontos trazidos por `Allan Kay`, um dos criadores do SmallTalk:

**Tudo são objetos. Um programa é um grupo de objetos enviando mensagens uns aos outros.**

Legal, agora vamos pegar essa definição e estruturar algumas coisas. No paradigma estruturado, temos procedimentos (ou funções) que são aplicados globalmente em nossa aplicação. No caso da orientação a objetos, temos métodos que são aplicados aos dados de cada objeto.

<img src="https://hermes.dio.me/articles/cover/ed2ae1f4-7901-49f7-b1b5-9fa595caadfc.png" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>

Para organizar a forma como esses programas devem ser escritos, os programadores da época, definiram um conjunto de pilares para definir boas práticas para a construção e manutenção dos sistemas.

<img src="https://imdtec.imd.ufrn.br/assets/lib/upload/plugins/source/imagens/76/697/pilares.jpg" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>

Esse vídeo é o um seminário onde Alan Kay apresenta os conceitos de orientação objeto. É extenso e bastante antigo, mas recomendo muito:

<iframe width="560" height="315" src="https://www.youtube.com/embed/QjJaFG63Hlo?si=tyKjZwgp-oTuIZ-S" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}></iframe>

Vamos ver cada um desses pilares.

## 3.1 Abstração

Esse pilar, como o próprio nome diz, visa abstrair algo do mundo real e transforma-lo em um objeto na programação. Identifique quais características e ações do objeto, são relevantes para o cenário (SW/Sistema).

A abstração é o processo de isolar apenas as características essenciais de um objeto do mundo real, ignorando detalhes irrelevantes para o sistema.
Em termos práticos, significa modelar apenas o que é necessário para representar um conceito dentro do contexto da aplicação.

:::tip[Exemplo]

Por exemplo, se você está desenvolvendo um sistema bancário, para representar um Cliente não é necessário incluir informações como a cor da roupa ou o tipo sanguíneo — apenas o que é útil ao domínio, como nome, CPF e saldo.

:::

```java title="Cliente.java"
public class Cliente {
    private String nome;
    private String cpf;
    private double saldo;

    public Cliente(String nome, String cpf, double saldo) {
        this.nome = nome;
        this.cpf = cpf;
        this.saldo = saldo;
    }

    public void exibirInformacoes() {
        System.out.println("Cliente: " + nome + " | CPF: " + cpf);
    }
}

```



## 3.2 Encapsulamento

Encapsulamento é a técnica que faz com que detalhes internos do funcionamento uma classe permaneçam ocultos para os objetos. Por conta dessa técnica, o conhecimento a respeito da implementação interna da classe é desnecessário do ponto de vista do objeto, uma vez que isso passa a ser responsabilidade dos métodos internos da classe.

A ideia é encapsular, isto é, esconder todos os membros de uma classe, além de esconder como funcionam as rotinas (no caso métodos) do nosso sistema.

Encapsular é fundamental para que seu sistema seja suscetível a mudanças: não precisaremos mudar uma regra de negócio em vários lugares, mas sim em apenas um único lugar, já que essa regra está encapsulada.

:::tip[Programando voltado para a interface e não para a implementação]

É sempre recomendado programar pensando na interface da sua classe, como seus usuários (outros programadores e programadoras) estarão utilizando ela, não somente em como ela vai funcionar. A implementação em si, o conteúdo dos métodos, não tem tanta importância para o usuário dessa classe.

:::

Comece a se preocupar em como os outros objetos/classes (usuários) usarão a sua classe. Sempre que vamos acessar um objeto, utilizamos sua interface. Existem diversas analogias fáceis no mundo real:

> Quando você dirige um carro, o que te importa são os pedais e o volante (interface) e não o motor que você está usando (implementação). É claro que um motor diferente pode te dar melhores resultados, mas o que ele faz é o mesmo que um motor menos potente, a diferença está em como ele faz. Para trocar um carro a álcool para um a gasolina você não precisa reaprender a dirigir! (trocar a implementação dos métodos não precisa mudar a interface, fazendo com que as outras classes continuem usando eles da mesma maneira).

:::tip[Getter e Setter]

Para permitir o acesso aos atributos (já que eles são private) de uma maneira controlada, a prática mais comum é criar dois métodos, um que retorna o valor (getter) e outro que muda o valor (setter).

A convenção para esses métodos é de colocar a palavra get ou set antes do nome do atributo. Apenas um exemplo, manteremos o único modo de acesso ao saldo de nossa classe através dos métodos sacar e depositar, onde colocamos lógica para proteger a semântica.

:::

É uma má prática criar uma classe e, logo em seguida, criar getters e setters para todos seus atributos. Você só deve criar um getter ou setter se tiver a real necessidade. Repare que nesse exemplo setSaldo não deveria ter sido criado, já que queremos que todos usem depositar() e sacar().

Outro detalhe importante, um método getX não necessariamente retorna o valor de um atributo que chama X do objeto em questão. Isso é interessante para o encapsulamento. Imagine a situação: queremos que o banco sempre mostre como saldo o valor do limite somado ao saldo (uma prática comum dos bancos que costuma iludir seus clientes). 

O código nem possibilita a chamada do método getLimite(), ele não existe. E nem deve existir enquanto não houver essa necessidade.  O método getSaldo() não devolve simplesmente o saldo... e sim o que queremos que seja mostrado como se fosse o saldo. 

Utilizar getters e setters não só ajuda você a proteger seus atributos, como também possibilita ter de mudar algo em um só lugar... chamamos isso de encapsulamento, pois esconde a maneira como os objetos guardam seus dados. É uma prática muito importante.

:::tip[Construtores]

Construtores são métodos (CUIDADO) especiais chamados pelo sistema no momento da criação de um objeto. Eles não possuem valor de retorno, porque você não pode chamar um construtor para um objeto, você só usa o construtor no momento da inicialização do objeto. Quando usamos a palavra chave new, estamos construindo um objeto. Sempre quando o new é chamado, ele executa o construtor da classe.

Construtor é como se fosse um método que possui o mesmo nome da classe.

Pode variar de acordo com a linguagem utilizada, mas o conceito é o mesmo.

:::

O encapsulamento protege os dados internos de uma classe, impedindo que sejam acessados ou modificados diretamente por outras partes do sistema.
Ele garante que as mudanças internas não quebrem o funcionamento do resto do programa, pois o acesso ocorre apenas através de métodos controlados. Pensa na ideia de uma caixa-preta: você pode usar os botões (métodos públicos), mas não precisa (nem deve) ver o que acontece lá dentro.

```java title="ContaBancaria.java"
public class ContaBancaria {
    private double saldo;

    public ContaBancaria(double saldoInicial) {
        this.saldo = saldoInicial;
    }

    public void depositar(double valor) {
        saldo += valor;
    }

    public boolean sacar(double valor) {
        if (valor <= saldo) {
            saldo -= valor;
            return true;
        } else {
            System.out.println("Saldo insuficiente!");
            return false;
        }
    }

    public double getSaldo() {
        return saldo;
    }
}

```

## 3.3 Herança

Herança é um mecanismo que permite que características comuns a diversas classes sejam fatoradas em uma classe base, ou superclasse. A partir de uma classe base, outras classes podem ser especificadas.

Cada classe derivada ou subclasse apresenta as características (estrutura e métodos) da superclasse e acrescenta a elas o que for definido de particularidade para ela.

A herança permite criar novas classes reutilizando código de classes já existentes. A classe original é chamada de superclasse (ou classe base), e as classes derivadas são chamadas de subclasses. Isso permite reaproveitar atributos e métodos e adicionar ou especializar comportamentos quando necessário. É como uma “árvore genealógica” de classes: uma subclasse herda características da superclasse, mas também pode ter suas próprias particularidades.

Algumas formas de herança:

- **Extensão:** subclasse estende a superclasse, acrescentando novos membros (atributos e/ou métodos). A superclasse permanece inalterada, motivo pelo qual este tipo de relacionamento é normalmente referenciado como herança estrita.
- **Especificação:** a superclasse específica o que uma subclasse deve oferecer, mas não implementa nenhuma funcionalidade. 
- **Combinação de extensão e especificação:** a subclasse herda a interface e uma implementação padrão de (pelo menos alguns de) métodos da superclasse. 

```java title="Funcionario.java"
public class Funcionario {
    protected String nome;
    protected double salario;

    public Funcionario(String nome, double salario) {
        this.nome = nome;
        this.salario = salario;
    }

    public double calcularBonus() {
        return salario * 0.05;
    }
}

```

```java title="Gerente.java"
public class Gerente extends Funcionario {
    private double bonusExtra;

    public Gerente(String nome, double salario, double bonusExtra) {
        super(nome, salario);
        this.bonusExtra = bonusExtra;
    }

    @Override
    public double calcularBonus() {
        return super.calcularBonus() + bonusExtra;
    }
}

```

### 3.3.1 Herança vs Interfaces

A herança e a implementação de interfaces são duas formas de reutilizar e organizar código em sistemas orientados a objetos — mas elas resolvem problemas diferentes.

| Aspecto                    | **Herança**                                                | **Interfaces**                                                               |
| -------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **Propósito**              | Reutilizar código e comportamentos de uma classe base.     | Definir um contrato (métodos obrigatórios) que outras classes devem cumprir. |
| **Tipo de relação**        | “É um” (ex: `Gerente` **é um** `Funcionario`).             | “Sabe fazer” (ex: `Funcionario` **sabe** executar uma tarefa).               |
| **Reutilização de código** | Permite reutilizar **atributos e métodos** da superclasse. | Não possui implementação (até Java 8), apenas a **assinatura dos métodos**.  |
| **Flexibilidade**          | Classes só podem **herdar de uma única superclasse**.      | Uma classe pode **implementar múltiplas interfaces**.                        |
| **Acoplamento**            | Cria forte dependência entre as classes.                   | Favorece baixo acoplamento e maior flexibilidade.                            |

```java
// Superclasse
public class Funcionario {
    protected String nome;
    protected double salario;

    public Funcionario(String nome, double salario) {
        this.nome = nome;
        this.salario = salario;
    }

    public double calcularBonus() {
        return salario * 0.1;
    }
}

// Interface — define um contrato
interface Trabalhavel {
    void executarTarefa();
}

// Subclasses com herança e interface
public class Gerente extends Funcionario implements Trabalhavel {
    public Gerente(String nome, double salario) {
        super(nome, salario);
    }

    @Override
    public void executarTarefa() {
        System.out.println(nome + " está gerenciando a equipe.");
    }
}

public class Desenvolvedor extends Funcionario implements Trabalhavel {
    public Desenvolvedor(String nome, double salario) {
        super(nome, salario);
    }

    @Override
    public void executarTarefa() {
        System.out.println(nome + " está programando novas funcionalidades.");
    }
}

// Exemplo de uso
public class Main {
    public static void main(String[] args) {
        Trabalhavel t1 = new Gerente("Ana", 8000);
        Trabalhavel t2 = new Desenvolvedor("Carlos", 5000);

        t1.executarTarefa();
        t2.executarTarefa();
    }
}

```

## 3.4 Polimorfismo

O polimorfismo (do grego “muitas formas”) é a capacidade de um mesmo método ter comportamentos diferentes dependendo do tipo do objeto que o invoca.
Ele se manifesta de duas formas principais:

- **Sobrescrita (Override):** quando uma subclasse redefine um método herdado da superclasse.
- **Sobrecarga (Overload):** quando uma mesma classe tem métodos com o mesmo nome, mas assinaturas diferentes (parâmetros distintos).

Em termos simples: diferentes objetos podem reagir de maneira própria à mesma ação.

```java title="Animal.java"
public class Animal {
    public void emitirSom() {
        System.out.println("Som genérico de animal");
    }
}

```

```java title="Cachorro.java"
public class Cachorro extends Animal {
    @Override
    public void emitirSom() {
        System.out.println("Au Au!");
    }
}

```

```java title="Main.java"
public class Main {
    public static void main(String[] args) {
        Animal a1 = new Animal();
        Animal a2 = new Cachorro();
        a1.emitirSom(); // Som genérico
        a2.emitirSom(); // Au Au!
    }
}
```

## 3.5 Exercícios

### 3.5.1 - Abstração

Crie uma classe chamada Livro que possua apenas as informações essenciais para um sistema de biblioteca digital.
Explique, em um comentário no código, quais informações você optou por abstrair (ou seja, não incluir). Faça uma implementação que utilize a classe criada.

### 3.5.2 - Encapsulamento

Implemente uma classe CaixaEletronico que utilize a classe ContaBancaria.
Simule uma tentativa de saque direto no atributo saldo e explique por que isso não é possível (nem desejável) quando o encapsulamento é aplicado corretamente.

### 3.5.3 - Herança

Crie uma classe Estagiario que herde de Funcionario, mas receba metade do bônus padrão.
Depois, instancie um Gerente e um Estagiario e mostre a diferença entre os bônus calculados.

### 3.5.4 - Polimorfismo

Crie uma classe Gato que também herde de Animal e sobrescreva o método emitirSom() para imprimir “Miau!”.
Depois, adicione os três objetos (Animal, Cachorro, Gato) em um mesmo array e percorra-os chamando emitirSom() — observe o resultado.

### 3.5.5 - Integração

Implemente um pequeno sistema de locadora de veículos usando todos os pilares:

- Abstração: Crie uma classe base Veiculo com apenas os atributos essenciais.
- Encapsulamento: Torne esses atributos privados e crie métodos controlados para acesso.
- Herança: Crie subclasses como Carro, Moto e Caminhao.
- Polimorfismo: Faça com que cada uma tenha uma implementação diferente de calcularValorDiaria().

No final, crie uma lista de Veiculo e imprima o valor da diária de cada tipo, mostrando o comportamento polimórfico em ação.