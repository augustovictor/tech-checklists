# Code review checklist

## Analyzing the business value
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

## Assumptions:
- A premature decision is a decision made with suboptimal knowledge
- To write clean code, you must first write dirty code and then clean it

---

## Analyzing code quality and classes design
<details>
  <summary>Meaningful names [Nomes significativos]</summary>
  
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
  - [ ] For constructor overload use factory method
  - [ ] Methods should have verbs
  - [ ] One word per concept. Don’t mix fetch, get, retrieve. Or add, create, increase. For the same concept.
  - [ ] Use computer science terms, algorithm names, pattern names, etc. Programmers will be reading the code
  - [ ] Add meaningful context. A variable named state does not give any context when received as a parameter. However if its name is addrState you’d know it it’s part of an address.
</details>

---

<details>
  <summary>Functions</summary>
  
  - [ ] Should be small
  - [ ] Should have one level of abstraction only so readers may tell whether a particular expression is an essential concept or a detail.
  - [ ] `If` statements should always be positive
  - [ ] Reading Code from Top to Bottom: The Stepdown Rule
  - [ ] We want the code to read like a top-down narrative. We want every function to be followed by those at the next level of abstraction so that we can read the program, descending one level of abstraction at a time as we read down the list of functions.
  - [ ] To say this differently, we want to be able to read the program as though it were a set of TO paragraphs, each of which is describing the current level of abstraction and referencing subsequent TO paragraphs at the next level down.
  - [ ] To include the setups and teardowns, we include setups, then we include the test page content, and then we include the teardowns.
  - [ ] To include the setups, we include the suite setup if this is a suite, then we include the regular setup.
  - [ ] To include the suite setup, we search the parent hierarchy for the “SuiteSetUp” page and add an include statement with the path of that page.
  - [ ] To search the parent. . .
  - [ ] Number of arguments:
  - [ ] zero (niladic)
  - [ ] one (monadic)
  - [ ] two (dyadic)
  - [ ] Three arguments (triadic) should be avoided where possible
  - [ ] More than three (polyadic) requires very special justification—and then shouldn’t be used anyway.
  - [ ] Don’t use output parameters (parameters that suffer changes within the function they are passed in)
  - [ ] No flag arguments
  - [ ] Functions should have no side effect
  - [ ] Watch out for temporal coupling (When there is a right moment to call a function, otherwise it would not work)
  - [ ] If a function must change the state of something, have it change the state of its own object
  - [ ] We should not have to check the function signature to understand what it does.
  - [ ] Command Query Separation: Functions should either do something or answer something. Not both.
  - [ ] Say we have a function to set a value to an attribute and return true if it succeeds. In our program it would be wrapped in an if condition. However we would not tell whether we’re checking the attribute was previously set then we do something (work as an adjective). Or if we can assign a value successfully (work as a verb). A solution to that is to separate the verification(Query) from the assignment(Command). And wrap the command inside an if statement verifying the Query in our program.
  - [ ] It's better to throw exceptions than returning error codes. By returning error codes we obligate the caller to handle the error immediately. Also it leads to many ‘if’ statements inside the command body.
  - [ ] By using exceptions we can, in our program, wrap the statement with a try catch block and handle error separately.
  - [ ] Extract the content of try catch blocks to functions.
</details>

---

<details>
  <summary>Formatting</summary>

  - [ ] Place closely related concepts.
  - [ ] Use empty lines to separate context
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

  - [ ] Use unchecked exceptions. The main difference between checked and unchecked exception is that the checked exceptions are checked at compile-time while unchecked exceptions are checked at runtime.
  - [ ] Provide context with exceptions so we can track down where it came from
  - [ ] SPECIAL CASE PATTERN [Fowler].
  - [ ] Don’t Return Null, return special case types
</details>

---

<details>
  <summary>Boundaries</summary>

  - [ ] Learning tests: Tests to learn a new package/lib/api
  - [ ] Using code that does not exist yet:
  - [ ] Create an interface to represent that code, then create an implementation adapter to use the actual implementation, and also a fake implementation to use in tests.
  - [ ] See more about seams in [WELC]
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
