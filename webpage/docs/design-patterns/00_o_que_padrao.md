---
sidebar_position: 1
slug: /design-patterns/definicao
---

# O que é um Design Pattern?

Vamos por partes, primeiro, vamos compreender o que é um padrão de projeto e como ele pode ser utilizado para facilitar nossa vida durante o desenvolvimento. Vamos primeiro observar a definição do "Refactoring Guru": 

> *Design patterns are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.*

Beleza, agora vamos analisar ela com calma. A primeira afirmação dela é que sua premissa está em resolver problemas que ocorrem comumente durante o desenvolvimento de software. Pense em situações que ocorrem repetidas vezes durante o processo de desenvolvimento de aplicações de software. Para tornar mais tangível, esse processo, vamos a origem destas definições. 

Na arquitetura, algumas situações aconteciam repetidas vezes, como o posicionamento de portas e janelas. Vendo que algumas soluções acabavam ficando melhores, ou mais bem desenvolvidas que outras, os arquitetos da época criaram padrões para fazer tais instalações. Esses padrões não eram restrições ou receitas que deveriam ser seguidas a risca, mas sim recomendações de como essa instalação poderia ser realizada para facilitar sua instalação.

<img src="https://numnum.blog/wp-content/uploads/2023/02/2-bg-composite-israel-door-2022-new-large.jpg?w=1024" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>

Ainda do Refactoring Guru:

> *The concept of patterns was first described by Christopher Alexander in A Pattern Language: Towns, Buildings, Construction. The book describes a “language” for designing the urban environment. The units of this language are patterns. They may describe how high windows should be, how many levels a building should have, how large green areas in a neighborhood are supposed to be, and so on.*

Beleza, mas onde o software entra nessa história? Essa é uma ótima pergunta, em 1994, Erich Gamma, John Vlissides, Ralph Johnson, e Richard Helm publicaram o livro [*Design Patterns: Elements of Reusable Object-Oriented Software*](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612), que trazia 23 padrões para resolver problemas de orientação a objeto. Esse livro ficou conhecido como o livro da gangue dos quatro (*GoF - Gang of Four - book*).

Para fechar essa introdução, é importante que um conceito fique absurdamente claro: os padrões não são algoritmos. Por mais que existam semelhanças entre eles, os padrões são mais parecidos com os *blueprints* de um projeto, onde podemos ver o resultado final e suas características, mas não conseguimos ver sua ordem de implementação.

<img src="https://i0.wp.com/circuitcellar.com/wp-content/uploads/2013/12/ArduinoUnoPoster.jpg" style={{ 
    display: 'block',
    marginLeft: 'auto',
    maxHeight: '60vh',
    marginRight: 'auto'
  }}/>
<br/>

## Quando utilizar os padrões

Devemos utilizar os padrões sempre que for possível? Essa é uma pergunta complexa de se responder. A melhor resposta que consigo pensar para ela nesse momento é: devemos utilizar os padrões sempre que eles trouxerem uma quantidade de vantagens suficiente para justificar sua implementação. Parece meio vago e meio genérico, mas também é verdadeiro. 

Sempre que possível lembrar-se do lema: *If all you have is a hammer, everything looks like a nail.* Não é porque você sabe utilizar os padrões de projeto que você deve tentar colocar eles em todas as soluções que você for desenvolver. Sempre verificar se a solução que está sendo desenvolvida tem aderência com algum dos padrões.

## Tipos de padrões

Os padrões podem ser classificados por sua intenção, dentro do contexto de onde eles vão resolver um determinado tipo de problema. Eles são agnósticos a linguagem de programação. Os três principais tipos de padrões de projeto são:

- **Criacionais**: tratam da criação de objetos e encapsulam a lógica de instanciação;

- **Estruturais**: lidam com a composição de classes e objetos para formar estruturas maiores;

- **Comportamentais**: definem como os objetos interagem e comunicam entre si.