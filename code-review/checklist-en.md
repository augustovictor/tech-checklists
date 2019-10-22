# Code review checklist

## Analizando o valor de negócio
Verificar primeiro se o valor de negócio está sendo entregue, ou seja, se o [teste funcional](http://softwaretestingfundamentals.com/functional-testing/) é aprovado.

O ponto aqui é que se temos a funcionalidade entregando a feature esperada, podemos avaliar se temos tempo para refactoring. Caso contrário podemos assumir o débito técnico e cadastrar uma nova task no gerenciador de tarefas.

Não execute testes manuais, garanta testes automatizados.
Pros:
* No time is spent creating a script for how to setup the environment for the test
* No time is spent performing the manual test
* No repetition of the whole manual process if more than one people tests the feature
Cons:
* The reviewer should have a good technical background and a complete understanding of what is required from business so they analyze whether the test covers the proper scenarios.
* No room for exploratory tests (Input change, or request method change, etc)

O que considerar:
* Verifique se o teste de integração cobre o `happy path`;
* Garanta que não há mais de um teste de integração cobrindo o mesmo cenário;
* Caso o teste faça uso de `Mocks`, especifique os valores para os atributos que farão parte do teste, ao invés de assumir valores default;
* Faça assert dos valores dos atributos que estão sendo considerados no teste;
* Verifique se o teste não causa side effects, ou seja, se ele não altera valores que de fato não deveriam ser alterados;
* Verfique se edge cases estão sendo cobertos por unit tests;
* Verifique se ações cruciais estão sendo executadas, e com os valores esperados, ao invés de usar o `any()`. Ex: Eventos de auditoria, eventos de banco de dados, chamadas para sistemas de terceiros, etc;

---

## Consideraçes iniciais:
- Uma decisão prematura é feita com um conhecimento limitado e sub-ótimo;
- Para escrever clean code, devemos primeiro escrever código ruim, e então refatorar;

---

## Analizando a qualidade do código e o design de classes
<details>
  <summary>Meaningful names</summary>
  
  - [ ] Os nomes escolhidos para variáveis, métodos, classes, etc; expressam realmente seu significado?
    - Exemplos de nomes de variáveis: `elapsedTimeInDays`, `daysSinceCreation`, `daysSinceModification`, `fileAgeInDays`;
  - [ ] Não faça append do tipo da variável ao seu nome. Ex: `shoppingCartItemsList`;
  - [ ] Consistência ao escolher nomes que tem conceitos similares;
  - [ ] Consistência ao, escolher nomes que tem conceitos similares
  - [ ] O tamanho do nome da variável deve corresponder ao seu escopo. Nome curto -> Escopo curto;
  - [ ] Consistência ao, escolher nomes que tem conceitos similares
  - [ ] Existem valores mágicos pelo código? Extraia constantes para tais situações;
  - [ ] Utilize nomes pronunciáveis e que sejam possíveis de serem procurados. Nada de `xbhqService`;
  - [ ] Utilize unencoded interfaces. `IShapeFactory` -> `ShapeFactory`, e então nas classes concrertas: `ShapeFactoryImp`;
  - [ ] Classes devem ser nomeadas com substantivos ou frases. Ex: `Customer`, `WikiPage`, `Account`, `AddressParser`. Evite nomes como `Manager`, `Processor`, or `Info`. O nome de uma classe não deve ser um verbo.
  - [ ] Para `constructor overload` use um `factory method`;
  - [ ] Métodos devem ser nomeados como verbos;
  - [ ] Escolha uma palavra por conceito. Não misture `fetch`, `get`, `retrieve`; ou `add`, `create`, `increase`;
  - [ ] Faça uso dos termos da computação, nomes de algorítmos, nomes de patterns, etc. São pessoas familiarizadas com desenvolvimento de software que lerão o código. Não se preocupe :);
  - [ ] Adicione contexto ao escolher nomes. A variável `state` não provê contexto algum quando recebida sozinha como parâmetro, mas se o noem dela é `addrState`e conseguimos deduzir que ela é parte de um endereço.
</details>

---

<details>
  <summary>Functions</summary>
  
  - [ ] Devem ser pequenas
  - [ ] Devem ter apenas um nível de abstração, de forma que possa-se entender se ela é um conceito essencial ou apenas um detalhe;
  - [ ] `If` statements devem sempre estar na forma positiva;
  - [ ] O código deve ser lido de cima para baixo: Stepdown Rule
  - [ ] O código deve ser lido como uma narrativa, onde cada função é sucedida por uma com seu próximo nível de abstração;
    - To say this differently, we want to be able to read the program as though it were a set of TO paragraphs, each of which is describing the current level of abstraction and referencing subsequent TO paragraphs at the next level down.
    - To include the setups and teardowns, we include setups, then we include the test page content, and then we include the teardowns.
    - To include the setups, we include the suite setup if this is a suite, then we include the regular setup.
    - To include the suite setup, we search the parent hierarchy for the “SuiteSetUp” page and add an include statement with the path of that page.
    - To search the parent. . .
  
  ### Number of arguments:
  
  - [ ] Zero (niladic);
  - [ ] Um (monadic;
  - [ ] Dois (dyadic);
  - [ ] Três argumentos (triadic) deve ser evitado sempre que possível;
  - [ ] Mais do que 3 (polyadic) - Requer uma boa justificativa, pois não deve ser usado;
  - [ ] Não use parâmetros `output` (Parâmetros que sofrem modificações dentro do método no qual é passado);
  - [ ] Não utilize parâmetros como flag
  - [ ] Funções não devem ter efeitos colaterais;
  - [ ] Observe acoplamento temporal (Quando existe um momento certo para chamar uma função, caso contrário ela não irá funcionar);
  - [ ] Se uma função precisa alterar o estado de algo, que seja do próprio objeto que está a invocando;
  - [ ] Não devemos ter que ler a assinatura de uma função para entender o que ela faz. O próprio nome deve deixar isso descrito;
  - [ ] Command Query Separation: Funções devem executar algo OU retornar algo. Não os dois;
    - Say we have a function to set a value to an attribute and return true if it succeeds. In our program it would be wrapped in an if condition. However we would not tell whether we’re checking the attribute was previously set then we do something (work as an adjective). Or if we can assign a value successfully (work as a verb). A solution to that is to separate the verification(Query) from the assignment(Command). And wrap the command inside an if statement verifying the Query in our program.
  - [ ] É melhor lançar `exceptions` do que retornar códigos de erro. Ao retornar um código de erro nós obrigamos o client a tratar o erro imediatamente (programação defensiva);
    - Ao fazer uso de exceptions podemos fazer um wrap na classe client da nossa função, e separar a execução do cenário esperado do momento de tratamento de erro;
  - [ ] Extraia para funções o conteúdo de tryCatch blocks;
</details>

---

<details>
  <summary>Formatting</summary>

  - [ ] Posicione de forma próxima conceitos relacionados;
  - [ ] Utilize linhas em branco para separar contexto;
</details>

---

<details>
  <summary>Objects and data structures</summary>

  - Objetos escodem seus atributos atrás de abstrações e expõe funções que operam sobre esses dados. Enquanto que estruturas de dados expõe seus atributos mas não contém funções significativas;
  - Código procedural (código usando estrutuas de dados) tornam fácil o processo de adicionar novas funções sem alterar as estruturas de dados existentes. Mas torna difícil adicionar novas estruturas de dados, porque todas as funções precisam ser alteradas.
  - Código OO, ao contrário, torna fácil adicionar novas classes sem alterar as funções já existentes. Mas torna difícil adicionar novas funções, pois todas as classes precisam ser alteradas;
    - (See how to solve)Lei de Demeter: Um módulo não deve saber sobre os detalhes internos do objeto que ele manipula. Talk to friends, not to strangers
  - [ ] Evite `train wracks`: `final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();`
    - É melhor fazer o split em várias variáveis;
</details>

---

<details>
  <summary>Error Handling</summary>

  - [ ] Use `unchecked exceptions`. A principal diferença é que as `unchecked exceptions` são verificadas em tempo de execução, e não em tempo de compilação como as `checked exceptions`;
  - [ ] Forneça contexto nas exceptions para que seja possível saber exatamente de onde saiu;
  - [ ] Não retorne `null`, faça uso do SPECIAL CASE PATTERN [Fowler];
</details>

---

<details>
  <summary>Boundaries</summary>

  - [ ] Quando estiver estudando algum package, lib, ou api, escreva testes. Dessa forma você não só aprende como é o funcionamento mas também deixa a documentação para outras pessoas;
  - [ ] Quando precisar fazer uso de código que ainda não existe, crie uma interface para representar o código, e então crie um adapter para usar a real implementação posteriormente, e também poder criar uma implementação fake para os testes;
    - See more about seams in [WELC]
</details>

---

<details>
  <summary>Unit tests - Laws of TDD</summary>

  - [ ] A primeira lei é não escrever código de produção até termos um teste falhando para o cenário;
  - [ ] A segunda lei é que não se deve escrever mais código de teste do que o suficiente para que os testes falhem;
  - [ ] A terceira lei é não escrever mais código de produção do que o suficiente para os testes passarem;
  - [ ] Aborde apenas UM conceito por teste;
  - [ ] Minimize o número de asserts por teste;
  - Tests rules
  - [ ] **F**ast: Testes devem ser rápidos;
  - [ ] **I**ndependent: Testes não devem depender um do outro;
  - [ ] **R**epeatable: Testes devem poder ser repetidos em qualquer ambiente;
  - [ ] **S**elf-validating: Testes devem ter um output booleano. Ou passar ou falhar;
  - [ ] **T**imely: Testes devem ser escritos antes do código de produção, caso contrário pode ser difícil criar os testes;
    - O TDD faz sentido quando queremos testar classes complexas ou algorítmos. Quando estamos falando de código de infraestrutura, normalmente é um comportamento padrão, em que a forma de escrever a classe não irá mudar dependendo do TDD.
</details>

---

<details>
  <summary>Classes</summary>

  ### A ordem dos elementos em uma classe deve ser:
  - [ ] Variáveis;
  - [ ] Constantes estáticas;
  - [ ] Variáveis estáticas privadas;
  - [ ] Variáveis de instância privadas;
  - [ ] Private utilities called by a public function right after the public function itself

  ### Encapsulation
  - [ ] Esconda o máximo possível
    - É mais simples deixar um método público caso haja a necessidade do que tornar um método privado, pois seu uso está espalhado pela aplicação;


  ### Design
  - [ ] Tamanho da classe (Contar quantidade de responsabilidades);
  - [ ] O nome da classe deve descrever que responsabilidades ela tem;
  - [ ] Se não é possível criar um nome consiso para uma classe, é provável que ela esteja com muitas responsabilidades;
  - [ ] Escreva um código que esteja preparado para mudanças;
  
  #### SRP
  
  - [ ] Devemos conseguir dar uma breve descrição de uma classe em mais ou menos 25 palavras, sem usar as palavras "se", "e" ou "ou";
  - [ ] Classes e módulos devem ter apenas UMA razão para mudar. Devem compreender apenas um conceito;
    - Pense na seguinte analogia: É melhor ter ferramentas separadas em pequenas caixas com labels ou uma grande caixa com tudo dentro?
  - [ ] O objetivo é organizar classes e módulos de forma que seja fácil encontrar o que se procura, e ter que lidar com a complexidade apenas do que se está tratando no momento;
  - Cohesion
  - [ ] Classes devem ter apenas algumas variáveis de instância. Cada um dos seus métodos deve manipular uma ou mais de suas variáveis. Quanto mais variáveis um método manipula, mais coeso ele é em relação à classe;
  
  #### DIP
  - [ ] Dependa de abstrações, não de implementações;

</details>

---

<details>
  <summary>Separe o momento de criação dos objetos do momento do seu uso</summary>

  - [ ] O módulo ‘Main’ define onde o objeto é criado. Ele tem a implementação da abstração da `Factory` que esta´no módulo `Application`;
  - [ ] O módulo ‘Application’ define onde o objeto é usado e depende de uma abstração;
</details>

---

<details>
  <summary>4 rules of Simple Design. a design is “simple” if it follows these rules</summary>

  - [ ] Execute todos os testes
  - [ ] O código não contém duplicações (DRY)
  - [ ] O design expressa as intenções do desenvolvedor
  - [ ] Minimize a quantidade de classes e métodos (lowest priority)
</details>

---

<details>
  <summary>Smells</summary>

  ### Smells de Environment
    - [ ] O build requer mais de um step (no single entry point)
    - [ ] Os testes requerem mais de um step
  
  ### Smells em Functions
    - [ ] Muitos argumentos
    - [ ] Output args
    - [ ] Flag args
</details>

---

<details>
  <summary>Tips</summary>

  ### General
- [ ] Obvious Behavior Is Unimplemented. We should not be surprised by the function for not doing expected operations
- [ ] Incorrect behavior on edge cases. Don’t rely on your intuition. Look for every boundary condition and write a test for it
- [ ] Overridden safeties
- [ ] Duplication. Find and eliminate duplication wherever you can
- [ ] Vertical separation
- [ ] Inconsistency. If you do something a certain way, do all similar things in the same way. This goes back to the principle of least surprise.
- [ ] Structure over Convention
- [ ] Enforce design decisions with structure over convention. Naming conventions are good, but they are inferior to structures that force compliance. For example, switch/cases with nicely named enumerations are inferior to base classes with abstract methods. No one is forced to implement the switch/case statement the same way each time; but the base classes do enforce that concrete classes have all abstract methods implemented.
- [ ] Encapsulate conditionals
- [ ] Don’t Be Arbitrary. Have a reason for the way you structure your code, and make sure that reason is communicated by the structure of the code
### Names
- [ ] Choose Names at the Appropriate Level of Abstraction. Don’t pick names that communicate implementation; choose names the reflect the level of abstraction of the class or function you are working in.
- [ ] Use Long Names for Long Scopes
- [ ] Names Should Describe Side-Effects. Ex: getFormattedValueOrEmpty
### Tests
- [ ] Use a Coverage Tool!
- [ ] Don’t Skip Trivial Tests
- [ ] An Ignored Test Is a Question about an Ambiguity
- [ ] Test Boundary Conditions
- [ ] Exhaustively Test Near Bugs
- [ ] Bugs tend to congregate. When you find a bug in a function, it is wise to do an exhaustive test of that function. You’ll probably find that the bug was not alone
</details>

---

## Avaliando o design de código e projeto de classes

### Consistência e objetos

---

### Encapsulamento e propagação de mudanças

---

### SRP - Coesão

---

### OCP - Acoplamento

---

### LSP - Herança e Composição

---

### ISP - Interfaces

---

## Avaliando smells arquiteturais da aplicação

---

## Avaliando o uso do Kotlin

---

## Avaliando os testes

### Regras de negócio (happy path X edge cases)
