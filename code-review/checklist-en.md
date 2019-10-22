# Code review checklist

## Analizando o valor de negócio
Verificar primeiro se o valor de negócio está sendo entregue, ou seja, se o [teste funcional](http://softwaretestingfundamentals.com/functional-testing/) é aprovado.

O ponto aqui é que se temos a funcionalidade entregando a feature esperada, podemos avaliar se temos tempo para refactoring. Caso contrário podemos assumir o débito técnico e cadastrar uma nova task no gerenciador de tarefas.

Don’t perform manual tests, ensure functional tests instead.
Pros:
* No time is spent creating a script for how to setup the environment for the test
* No time is spent performing the manual test
* No repetition of the whole manual process if more than one people tests the feature
Cons:
* The reviewer should have a good technical background and a complete understanding of what is required from business so they analyze whether the test covers the proper scenarios.
* No room for exploratory tests (Input change, or request method change, etc)

What to consider:
* Verify if the functional test covers the happy path
* Mocks should specify attributes under test instead of use default values
* Assert attribute values under test
* Verify if the functional test does not cause side effects (Changes what should not be changed)
* Verify if edge cases are covered by unit tests
* Verify if critical operations are being performed with specific values. Eg: Audit event, database events, integration calls, etc.

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
  - [ ] É melhor lançar exceptions do que retornar códigos de erro. Ao retornar um código de erro nós obrigamos o client a tratar o erro imediatamente (programação defensiva);
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

  - [ ] Objects hide their data behind abstractions and expose functions that operate on that data. Whereas DataStructures expose their data and have no meaningful functions.
  - [ ] Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions. Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.
  - [ ] (See how to solve)Law of Demeter: a module should not know about the innards of the objects it manipulates. In other words, talk to friends, not to strangers.
  - [ ] Avoid train wracks: final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
  - [ ] It’s better to split in variables
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

  - [ ] First Law You may not write production code until you have written a failing unit test
  - [ ] Second Law You may not write more of a unit test than is sufficient to fail, and not compiling is failing
  - [ ] Third Law You may not write more production code than is sufficient to pass the currently failing test
  - [ ] Single concept per test
  - [ ] Minimize the number of asserts per concept
  - [ ] Tests rules
  - [ ] Fast: Tests should be fast
  - [ ] Independent: Tests should not depend on each other
  - [ ] Repeatable: Tests should be repeatable in any environment
  - [ ] Self-validating: The tests should have a boolean output. Either they pass or fail.
  - [ ] Timely: The tests need to be written in a timely fashion. Unit tests should be written just before the production code that makes them pass. If you write tests after the production code, then you may find the production code to be hard to test
</details>

---

<details>
  <summary>Classes</summary>

  ### Structure
  - [ ] Variables
  - [ ] Static constants
  - [ ] Private static variables
  - [ ] Private instance variables
  - [ ] Private utilities called by a public function right after the public function itself

  ### Encapsulation
  - [ ] Hide as much as possible


  ### Design
  - [ ] Class size (Count responsibilities)
  - [ ] The name of a class should describe what responsibilities it fulfills
  - [ ] If we cannot derive a concise name for a class, then it’s likely too large
  - [ ] Organize your code for change
  - [ ] SRP
  - [ ] We should also be able to write a brief description of the class in about 25 words, without using the words “if,” “and,” “or,” or “but.”
  - [ ] Class or module should have one, and only one, reason to change
  - [ ] Do you want your tools organized into toolboxes with many small drawers each containing well-defined and well-labeled components? Or do you want a few drawers that you just toss everything into?
  - [ ] The goal is to organize it so that a developer knows where to look to find things and need only understand the directly affected complexity at any given time.
  - [ ] Cohesion
  - [ ] Classes should have few instance variables. Each of the methods of a class should manipulate one or more of those variables. In general the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable is used by each method is maximally cohesive.
  - [ ] DIP
  - [ ] Depend on abstractions, not on details

</details>

---

<details>
  <summary>Separate where an object is created from where it is used</summary>

  - [ ] ‘Main’ module defines where it is created. It has the Factory Implementation of an abstraction in the Application module
  - [ ] ‘Application’ module defines where it is used and depends on an abstraction
</details>

---

<details>
  <summary>4 rules of Simple Design. a design is “simple” if it follows these rules</summary>

  - [ ] Runs all the tests
  - [ ] Contains no duplication
  - [ ] Expresses the intent of the programmer
  - [ ] Minimizes the number of classes and methods (lowest priority)
</details>

---

<details>
  <summary>Smells</summary>

  ### Environment
    - [ ] Build requires more than one step (single entry point)
    - [ ] Tests require more than one step
  
  ### Functions
    - [ ] Too many args
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
