---
sidebar_position: 1
slug: /solid/definicoes
---

# Definição e Princípios


Pessoal, vamos estudar os padrões de como desenvolver soluções de software utilizando os princípios do S.O.L.I.D..

## 1. Single Responsiblity Principle (Princípio da responsabilidade única)

O princípio diz que: ***Uma classe deve ter apenas um motivo para mudar***.

Esse princípio declara que uma classe deve ser especializada em um único assunto e possuir apenas uma responsabilidade dentro do software, ou seja, a classe deve ter uma única tarefa ou ação para executar.

> `Murilão ela deve ter um método só?`

Ótima pergunta! Não necessáriamente. O importante é que todos os métodos desenvolvidos da classe façam sentindo quanto ao objetivo que foi estabelecido para a classe. Isso reduz o acoplamento e torna o código mais simples de manter.

Quando estamos aprendendo programação orientada a objetos, sem sabermos, damos a uma classe mais de uma responsabilidade e acabamos criando classes que fazem de tudo — *God Class*. Num primeiro momento isso pode parecer eficiente, mas como as responsabilidades acabam se misturando, quando há necessidade de realizar alterações nessa classe, será difícil modificar uma dessas responsabilidades sem comprometer as outras. Toda alteração acaba sendo introduzida com um certo nível de incerteza em nosso sistema.

A violação do Single Responsibility Principle pode gerar alguns problemas, sendo eles:

- **Falta de coesão** — uma classe não deve assumir responsabilidades que não são suas;
- **Alto acoplamento** — Mais responsabilidades geram um maior nível de dependências, deixando o sistema engessado e frágil para alterações;
- **Dificuldades na implementação de testes automatizados** — É difícil de “mockar” esse tipo de classe;
- **Dificuldades para reaproveitar o código.**

```java
// Violação do SRP
public class Pedido {
    public void calcularTotal() { /* ... */ }
    public void salvarNoBanco() { /* ... */ } // responsabilidade de persistência
    public void enviarEmailConfirmacao() { /* ... */ } // responsabilidade de notificação
}

// Correção aplicando SRP
public class Pedido {
    public void calcularTotal() { /* ... */ }
}

public class PedidoRepository {
    public void salvar(Pedido pedido) { /* ... */ }
}

public class EmailService {
    public void enviarConfirmacao(Pedido pedido) { /* ... */ }
}

```

## 2. Open-Closed Principle (Princípio Aberto-Fechado)

Este princípio tem como definição: ***Objetos ou entidades devem estar abertos para extensão, mas fechados para modificação***. Isso significa que quando novos comportamentos e recursos precisam ser adicionados no software, devemos estender e não alterar o código fonte original.

> `Mas Murilão, como vamos fazer isso? Não é igual modificar e não modificar ao mesmo tempo?`

Denovo, uma pergunta excelente! Perceba que os princípios do S.O.L.I.D. são recomendações e não regras fechadas. Eles vão ajudar a manter o código que for desenvolvido. Mas ainda não respondi a pergunta: como modificar só extendendo um comportamento? Lembram que quando estavamos estudando os pilares da orientação a objeto, vimos que existem alguns mecanismos que permitem confirmar que um comportamento será implementado?

Se você lembrou dos contratos, das interfaces, ou mesmo da herança, você está correto aqui! Importante perceber, que em geral fazemos extensões com o uso de interfaces. O objetivo deste princípio é evitar alterar uma classe já existente para adicionar um novo comportamento, pois corremos um sério risco de introduzir bugs em algo que já estava funcionando.

```java
// Sem OCP — cada novo tipo exige modificar a classe
public class CalculadoraDesconto {
    public double calcular(String tipoCliente, double valor) {
        if (tipoCliente.equals("VIP")) return valor * 0.9;
        if (tipoCliente.equals("NORMAL")) return valor * 0.95;
        return valor;
    }
}

// Aplicando OCP com Strategy
public interface Desconto {
    double aplicar(double valor);
}

public class DescontoVip implements Desconto {
    public double aplicar(double valor) { return valor * 0.9; }
}

public class DescontoNormal implements Desconto {
    public double aplicar(double valor) { return valor * 0.95; }
}

public class CalculadoraDesconto {
    public double calcular(Desconto desconto, double valor) {
        return desconto.aplicar(valor);
    }
}

```

## 3. Liskov Substitution Principle (Princípio da substituição de Liskov)

Princípio da substituição de Liskov — ***Uma classe derivada deve ser substituível por sua classe base***. As subclasses devem manter o comportamento lógico e coerente da superclasse.

Particularmente, acho esse o princípio mais dificil de se dominar por sua sutileza. Ao mesmo tempo, peço que vocês observem ele com calma. Ele está trazendo a coesão para o comportamento polimórfico seguro. Como a classe pai define um comportamento, as classes filhas devem seguir aquele padrão, ou assinatura. Assim, quando qualquer uma das classes filhas for utilizada, em um local em que é esperado a classe pai, o tipo de retorno e o comportamento daquele objeto estará seguindo esse padrão.

**Observação**: Você deve ter reparado que em alguns lugares eu utilizei classe pai/filha, em outros classe base/derivada, superclasse/subclasse e temos também classe mãe/filha. São todos sinônimos no contexto de demonstrar a hirarquia de classes em um projeto orientado a objetos.

Exemplos de violação do LSP:

- Sobrescrever/implementar um método que não faz nada;
- Lançar uma exceção inesperada;
- Retornar valores de tipos diferentes da classe base.

```java
// Exemplo clássico de violação do LSP
class Retangulo {
    protected int largura;
    protected int altura;

    public void setLargura(int largura) { this.largura = largura; }
    public void setAltura(int altura) { this.altura = altura; }

    public int getArea() { return largura * altura; }
}

class Quadrado extends Retangulo {
    @Override
    public void setLargura(int largura) {
        this.largura = this.altura = largura;
    }

    @Override
    public void setAltura(int altura) {
        this.largura = this.altura = altura;
    }
}

// Problema: ao usar um Quadrado como Retângulo, a área esperada pode ser incorreta.

```

## 4. Interface Segregation Principle (Princípio da Segregação da Interface)

O que nos diz o princípio: ***Uma classe não deve ser forçada a implementar interfaces e métodos que não irão utilizar.*** Esse princípio basicamente diz que é melhor criar interfaces mais específicas ao invés de termos uma única interface genérica.

Desta forma, devemos preferir desenvolver mais itnerfaces menores e específicas, em detrimento de interfaces grandes e com muitas funcionalidades.

```java
// Violação do ISP
public interface Ave {
    void voar();
    void nadar();
}

public class Pinguim implements Ave {
    public void voar() { /* Pinguim não voa */ }
    public void nadar() { System.out.println("Nadando!"); }
}

// Correção aplicando ISP
public interface AveQueVoa {
    void voar();
}

public interface AveQueNada {
    void nadar();
}

public class Pinguim implements AveQueNada {
    public void nadar() { System.out.println("Nadando!"); }
}

```

## 5. Dependency Inversion Principle (Princípio da inversão da dependência)

O princípio nos diz: ***Dependa de abstrações e não de implementações***. As classes de alto nível não devem depender diretamente de classes de baixo nível, mas sim de interfaces ou classes abstratas. 

Por exemplo, em vez de uma classe Pedido depender diretamente de MySQLRepositorio, ela depende de uma interface RepositorioPedidos. Assim, você pode trocar o banco sem alterar o código principal.

```java
// Sem DIP
public class PedidoService {
    private MySQLRepositorio repo = new MySQLRepositorio();
    public void salvar(Pedido pedido) {
        repo.salvar(pedido);
    }
}

// Aplicando DIP
public interface RepositorioPedidos {
    void salvar(Pedido pedido);
}

public class MySQLRepositorio implements RepositorioPedidos {
    public void salvar(Pedido pedido) { /* ... */ }
}

public class PedidoService {
    private final RepositorioPedidos repo;

    public PedidoService(RepositorioPedidos repo) {
        this.repo = repo;
    }

    public void salvar(Pedido pedido) {
        repo.salvar(pedido);
    }
}

// Uso
RepositorioPedidos repo = new MySQLRepositorio();
PedidoService service = new PedidoService(repo);

```

## 6. Recomendação Extra

Nada melhor que ouvir de quem cunhou e popularizou os termos:

<iframe width="560" height="315" src="https://www.youtube.com/embed/zHiWqnTWsn4?si=TVc7IN8t_FmLRNra" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## 7. Exercícios Sugeridos

### Exercício 1 — SRP + DIP: “Pedidos e Persistência”

**Objetivo:** aplicar Single Responsibility Principle (SRP) e Dependency Inversion Principle (DIP) separando regras de negócio de infraestrutura e dependendo de abstrações.


Implemente um pequeno módulo de pedidos:

- Regras de negócio: calcular total do pedido a partir de itens (preço unitário × quantidade), aplicar cupom opcional (percentual).

- Persistência: salvar o pedido em algum repositório (pode ser “em memória”).

- Notificação: enviar confirmação (pode ser “logar” no console).

Restrições & Requisitos:

- A classe Pedido não deve salvar nem enviar e-mail/log; apenas regras de negócio (SRP).

- PedidoService deve depender de interfaces PedidoRepository e Notifier (DIP). As implementações concretas serão injetadas.

- Permitir facilmente trocar InMemoryPedidoRepository por FilePedidoRepository sem alterar PedidoService.

Critérios de Aceitação

- Cálculo do total correto com/sem cupom.

- Salvar pedido via PedidoRepository injetado.

- Notificar via Notifier injetado após salvar.


### Exercício 2 — OCP + Strategy: “Mecanismo de Descontos”

**Objetivo:** aplicar Open-Closed Principle (OCP) com Strategy para permitir novos descontos sem modificar a calculadora.



Implemente uma calculadora de preço final que aplique um desconto configurável:

- Desconto VIP (10%).

- Desconto Sazonal (5%).

- Sem desconto.

Sem if/else por tipo na calculadora. Deve apenas delegar à estratégia.

Restrições & Requisitos

- Criar interface Desconto com método aplicar(double valor).

- A classe CalculadoraPreco recebe uma Desconto no construtor (ou método).

- Adicionar um novo desconto (por exemplo, “Lealdade 7%”) sem editar CalculadoraPreco.

Critérios de Aceitação

- Cálculo correto para cada estratégia.

- Inclusão de nova estratégia sem alterar a calculadora (OCP).