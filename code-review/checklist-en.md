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
    - Imagine que temos uma função que atribui um valor a uma propriedade e retorna `true` caso ocorra com sucesso. Na aplicação isso estaria dentro de um `if`, porém não seria possível dizer se a propriedade já estava definida com um valor e então estamos atribuindo um novo valor(adjetivo; ou se o valor foi atribuido com sucesso(verbo). Ex: `if (obj.assignNewValue('newValue')) { ... }`. A solução é separar a verificação (query) da atribuição (command). Ex: `if (obj.hasNewValue('newValue')) { obj.assignNewValue('newValue' }`;
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
- [ ] Comportamentos óbvios não estão implementados. Não devemos ser surpreendidos por uma função não executar um comportamento óbvio.
- [ ] Comportamento errado nos edge cases. Não confie na sua intuição. Teste todos os edge cases.
- [ ] Overridden safeties
- [ ] Encontre e remova duplicações de código ou lógica;
- [ ] Separação vertical (PBF ao invés de PBL)
- [ ] Inconsistência. Se algo é feito de uma forma, faça todas as outras da mesma forma. Principle of least surprise;
- [ ] Adote Structure over Convention;
- [ ] Garanta decisões de design através de estrutura ao invés de convenção. Convenções de nomenclatura são boas, mas são inferiores a estruturas, que forçam conformidade.
  - Ex: `switch cases` com `enums` são inferiores a classes base com métodos abstratos, pois não somos forçados a implementar um novo fluxo em um `switch case` da mesma forma sempre; mas classes base com métodos abstratos nos forçam a implementar os métodos nas classes filhas;
- [ ] Encapsule condicionais;
- [ ] Não seja arbitrário nas decisões, tenha um motivo para justificar a forma como você estrutur aseu código, e faça com que esse motivo seja comunicado pela estrutura do código;
### Names
- [ ] Escolha nomes de acordo com o nível de abstração apropriado. Não escolha nomes que expressam implementação, escolha nomes que refletem o nível de abstração da classe ou função;
- [ ] Use nomes longos para escopos longos de código;
- [ ] Nomes devem deixar de forma explícita. Ex: `getFormattedValueOrEmpty`
### Tests
- [ ] Use alguma ferramenta de code coverage
- [ ] Não deixe de implementar testes triviais
- [ ] Um teste ignorado é um ponto de dúvida
- [ ] Teste edge cases
- [ ] Quando encontrar um bug em alguma função, teste-a de forma exaustiva. Bugs tendem a se aglomerar. Provavelmente você irá encontrar algum outro bug;
</details>

---

## Avaliando o design de código e projeto de classes

### Consistência e objetos

* Garanta a integridade do estado dos objetos para que não ocorram surpresas durante seu funcionamento.
    * Se uma classe possui atributos nos quais não se pode funcionar corretamente sem, peça-os no construtor.
    * Force as classes a nascerem já em um estado válido. Caso a construção seja complexa e o construtor não atenda, parta para Creational Design Patterns como Builder ou Factory.
    * Garanta que uma classe permaneça sempre em um estado válido, e isso é atingido através de encapsulamento de métodos setters, para que nenhum client da classe deixe-a em um estado inválido.
    * O mesmo vale para atributos, se não faz sentido pedir os atributos no construtor, forneça valores padrão.
    * Como alguns frameworks utilizam um construtor padrão para inicializar uma classe, como o hibernate, crie um construtor com valores default e marque-o como deprecated.
* Trate a coerência dos valores passados, por clients, para atributos das classes (letras onde se espera string, números onde se espera int, etc)
    * Validação para o tipo de dado fornecido pelo client. Ex: campo idade está recebendo um int, campo email está recebendo um email de fato.
        * Validação no controller. Não devemos permitir dados sujos chegando no domínio.
    * Validação de negócio. Ex: O imposto precisa ser maior do que 1%, ou que pessoas precisam de um cpf.
        * Pode acontecer no construtor, que lança uma exception (mas polui o código)
        * Pode acontecer através de um método de verificação se é válido, mas garante que o objeto seja criado em um estado inválido (Não usar esse approach)
        * Pode acontecer através de um builder (approach intermediário), que garante que a entidade nasce com um estado válido, mas não coloca regras de validação na classe de domínio.
            * Dependendo do caso podemos ter uma classe responsável apenas pela validação. A vantagem disso é poder criar diferentes tipos de validação para uma mesma entidade, e essas validações poderão ser usadas em diferentes momentos do sistema. Usaria-se o decorator ou chain of responsibility. Essas classes poderão devolver erros a serem devolvidos para o client, ou lançar exceptions.
            * A escolha da abordagem deve partir do cenário, se forem validações complexas vale a pena deixar o design mais flexível, caso sejam simples, vale deixa-las no controller.
    * Teorema do bom vizinho: Não serão passados dados inválidos para o client. Não passar nulo onde não é esperado.
    * Tiny types são classes que representam conceitos pequenos como CPF, Endereço, Telefone. E isso evita que passemos atributos errados onde não deveríamos. Ex: uma classe recebe um conjunto de String. Cada tiny type pode fazer sua própria validação.
        * O hibernate fará queries maiores.
        * Pode complicar um cenário simples.
    * Faça uso de DTOs para desacomplar os dados exibidos/retornados do domínio da aplicação. O problema não é ter DTOs, mas sim SÓ ter DTOs.
    * Classes imutáveis não sofrem problemas de serem alteradas por diversos lugares do sistema. Isso facilita inclusive o paralelismo.
        * Para uma classe imutável basta evitar os setters.
        * Caso precisemos de uma alteração nessa classe imutável, o método que altera o estado da classe devolve uma nova instância com o novo valor.
        * Classes candidatas a serem imutáveis são classes que não terão seu estado alterado nunca. Ex: Um endereço sempre será o mesmo endereço. Uma data sempre existirá e será sempre a mesma. Já o pedido de um cliente será alterado.
    * Códigos com validação sempre serão feios, ou Factories… Pois terão várias verificações. Mas garante que esses códigos estejam sempre nas pontas da aplicação, fazendo conversão de tipos, fazendo ponte entre dois mundos (legado, infra).

---

### Encapsulamento e propagação de mudanças

* Se uma regra de negócio não está bem encapsulada, teremos essa regra espalhada por lugares diferentes.
* Imagine um  quebra cabeça, onde o formato das peças são interfaces e os desenhos são implementações.
    * Isso é programar OO: é pensar no formato das peças; às vezes, pensar até mais do que no próprio desenho.
* Tell, Don't Ask. Não faça verificações para executar uma operação. A verificação deverá acontecer dentro da classe que disponibiliza a operação, de forma encapsulada.
* A lei de demeter diz que nao devemos usar em uma classe A, a classe C atraves de B. Pois mudancas em C irao propagar mudancas em B (seu client direto) e em A (client indireto).
    * Para contornar este problema, devemos encapsular em B o metodo de C, caso A precise utiliza-lo.
    * Quebre esta regra quando fizer sentido. Quebra-la para o uso de getter causa menos danos do que para setters.
* Antes de criar setters, pense se eles não gerarão problemas de encapsulamento no futuro. É preferível você fornecer comportamentos que alterem o valor do atributo. No exemplo da conta, poderíamos ter métodos como saca() ou deposita(), muito mais interessantes do que um setter.
* Getters nos dao a oportunidade de modificar valores do objeto. Ex: fatura.getPagamentos().add(pagamento); Sendo assim é mais interessante fazer que seus getters sempre devolvam cópias dos objetos originais, ou mesmo bloqueiem alterações de alguma forma. Usar uma lista imutavel seria um exemplo. E a unica forma de alterar um valor seria atraves de um comportamento, bem encapsulado, na classe a ser alterada.
* Como que eu sei que as minhas classes e métodos estão encapsulados? Basta olhar para ela e tentar responder as duas perguntas: O quê? E como? O “o quê?” você tem que ser capaz de responder, porque o nome do método tem que lhe dizer isso. O “como?” você não tem que conseguir responder. Se conseguir responder, não está encapsulado.
* Evite modelos anemicos (Que possuem apenas atributos ou apenas metodos)

---

### SRP - Coesão

* Uma classe coesa possui uma única responsabilidade, um único motivo para mudar. Abrange apenas um conceito.
    * Uma classe não coesa cresce infinitamente
* Um comportamento não encapsulado propaga efeitos de sua alteração para o resto do sistema
* Métodos privados ajudam a melhorar a legibilidade dos métodos públicos
* Separar comportamentos em métodos não temos reuso
* Se a classe tem algum contato com infraestrutura, você não escreve regras de negócio alguma nelas; se a classe tem regras de negócio, ela não deve conhecer nenhuma infraestrutura
* Se uma classe chama muito uma outra classe pra executar uma série de procedimentos, este comportamento deve estar encapsulado dentro da outra classe, e ser invocado através de um método
* Dois comportamentos pertencem ao mesmo conceito/responsabilidade se ambos mudam juntos
* Como encontrar classes que não são coesas? Procure por classes que possuem muitos métodos diferentes; por classes que são modificadas com frequência; por classes que não param nunca de crescer.
* Na arquitetura hexagonal separamos as portas (classes do domínio) de adaptadores (classes que fazem a ponte entre mundos diferentes, como web e domínio, banco de dados e domínio etc.).

---

### OCP - Classes abertas

* O código deve estar sempre pronto pra evoluir. A evolução não deve forçar grandes mudanças no sistema, ou CTRL F para encontrar onde código precisa ser alterado. Ou descobrir onde mais a lógica precisa ser alterada.
* Deve-se receber dependências no construtor ao invés de instancia-las na classe
    * Podemos receber dependências através de um construtor ou por setters. Mas é preferível por construtor
* A classe que está aberta para extensão altera seu comportamento baseado nas abstrações que recebe. Então novos comportamentos podem ser adicionados sem que ela precise ser alterada.
* Isso é programar orientado a objetos. É lidar com acoplamento, coesão, pensando em abstrações para nossos problemas. Quando se tem uma boa abstração, é fácil evoluir o sistema. Seu sistema deve evoluir por meio de novas implementações dessas abstrações, previamente pensadas, e não por meio de diversos ifs espalhados por todo o código
* "se está difícil de testar, é porque seu código pode ser melhorado"
* um bom projeto de classes é aquele que deixa claro qual o caminho a seguir
* Classes abertas são aquelas que deixam explícitas as suas dependências, e mudam seu comportamento através delas.

---

### LSP - Herança e Composição

* Já dizia Joshua Bloch: "Crie suas classes pensando em herança, ou então proíba-a"
* Para usar herança de maneira adequada, o desenvolvedor deve pensar o tempo todos nas pré e pós-condições que a classe pai definiu.
    * Por precondições, entenda os dados que chegam nela. Quais são as restrições iniciais para que aquele método funcione corretamente?
    * As pós-condições são o outro lado da moeda. O que aquele comportamento devolve? 
    * Podemos sim mudar as pré e pós-condições, mas com regras.
      * Pense no caso em que a classe pai tem um método que recebe inteiros de 1 a 100. A classe filho pode sobrescrever esse método e permitir o método a receber inteiros de 1 a 200. Veja que, dessa forma, todo o código que já fazia uso da classe pai continua funcionando.
        * Ao contrário, a pós-condição só pode ser apertada; ela nunca pode afrouxar. Pense em um método que devolve um inteiro, de 1 a 100. As classes que a usam entendem isso. A classe filho sobrescreve o método e devolve números só entre 1 a 50. Os clientes continuarão a funcionar, afinal eles já entendiam saídas entre 1 e 50. Ou seja, não podemos nunca apertar
    * Ou seja, não podemos nunca apertar uma precondição, e nem afrouxar uma pós-condição.
* Modele hierarquias de forma que as classes filhas precisem conhecer pouco ou nada da classe pai. Pois caso a classe pai tenha algum coportamento inesperado (exception por chamar um metodo ou ter que chamar `super.abc()`), e que so pode ser descoberto caso olhemos a classe pai. Para atingir este objetivo as classes filhas devem usar seus proprios comportamentos, e nao contar com as implementacoes feitas na classe pai. Devemos usar comportamentos da classe pai quando precisarmos exatamente da regra implementada no comportamento da classe pai, ou ela + alguma regra. E nao apenas para reaproveitar codigo.
    * Se nao queremos que a classe filha altere diretamente o estado da classe pai, devemos definir os atributos na classe pai com visibilidade private e realizar as alteracoes atraves de metodos (que apenas as classes filhas manipulem. metodos protected).
    *   Se uma classe pai possui um atributo que sera alterado pelas classes filhas e nao ha nenhuma regra de negocio envolvida nesta alteracao, podemos deixar que as classes filhas o alterem, caso contrario seria mais interessante disponibilizar esta alteracao via comportamento
* Use composicao ao inves  de heranca.
* Herança deve ser usada quando existe realmente a relação de X é um Y. Por exemplo, Gerente é um Funcionário, ou ICMS é um Imposto. Não use herança caso a relação seja de composição, ou seja, X tem um Y, ou X faz uso de Y.
* Evite ao máximo que a classe filho conheça detalhes da implementação do pai, e não violam as restrições de pré e pós-condições na hora de sobrescrever um determinado comportamento. Nada também o impede de usar herança e composição. Use herança para reaproveitar código que realmente faz sentido, e composição para trechos de código que precisam de mais flexibilidade.
* Se você gerenciar bem todos os conceitos discutidos aqui (acoplamento, coesão e restrições entre classes pai e filhos), a herança será bem-vinda.
* Pacotes são a maneira que temos para agrupar classes que são parecidas. Lembre-se que o que está no mesmo pacote deve ser focado e suas classes sempre reutilizadas juntas. Também não se deve haver ciclos entre pacotes, ou seja, se o pacote A depende de B, então o pacote B não pode depender de A.
* Não descarte herança, apenas favoreça a composição

---

### ISP - Interfaces

* Em determinadas situacoes passamos uma entidade inteira para um metodo que usa apenas um dos atributos. Imagine que um objeto NotaFiscal possui um List<Item> e passamos o objeto NotaFiscal para um metodo calcular, que por sua vez percorre os itens e calcula o valor dos impostos baseado no valor total da soma. Passar a NotaFiscal inteira é impor um acoplamento entre o método calcular e uma classe instável (pois é uma implementação concreta). Para resolver esse problema não bastaria passar o valor total (um double) para o método? Isso não representaria muito bem o que o método 'calcular' recebe como entrada, já que este pode ser qualquer double. E o que queremos na verdade é o valor total da soma de Itens. Para tal situação é mais interessante criar uma interface 'Tributável', por exemplo, que teria um método itensASeremTributados(). Esta interface seria implementada pela NotaFiscal, e recebida como argumento pelo método calcular(itensTributaveis: Tributavel)
    * Essa abordagem nos provê semântica no parâmetro do método calcular
* Temos 2 formas de obter instancias de classes. Atraves de um CDI ou Factories.

### DIP - Acoplamento

* Quando uma classe possui muitas dependencias, todas elas propagam seus efeitos(impactos) para a classe principal, tornando-se fragil.
* Depender de classes, interfaces, módulos, que sejam estáveis e tendam a mudar pouco é mais seguro do que depender de classes concretas.
* Alguns padrões de projeto ajudam você a desacoplar seus projetos de classe, como o caso do Observer, Visitor e Factory.
* Antes de usar a solução, tenha certeza de que o problema existe. Evite overengineering
* A ideia é: sempre que uma classe for depender de outra, ela deve depender sempre de outro módulo mais estável do que ela mesma.
    * Se A depende de B, a ideia é que B seja mais estável que A. Mas B depende de C. Logo, a ideia é que C seja mais estável que B. Ou seja, suas classes devem sempre andar em direção à estabilidade, depender de módulos mais estáveis que ela própria.
* módulos estáveis (não queremos mexer nunca, pois eles são importantes e muitas outras classes dependem deles), e módulos mais instáveis (que dependem dos estáveis, mas que vez ou outra precisam sofrer alterações). Essa é outra balança para você pensar: módulos estáveis e módulos instáveis
* Procure trechos no código onde duas ou mais dependências são usadas para algo específico, e somente para aquilo, e abstraia estas dependências agrupando-as. Assim reduzimos o acomplamento e aumentamos a coesão da nossa classe (muitas regras), teremos mais métodos usando os atributos da própria classe)
* Observe o acomplamento lógico. O impacto de alterações em um componente que afeta outro. Isso pode indicar um mal projeto de classes, ou que um código não está bem encapsulado.

---

## Avaliando smells arquiteturais da aplicação

* Rigidez
    * É a tendência de um software de resistir à mudanças. Quando mudamos algo, temos que mexer em muitos outros módulos.
    * Esse smell nos faz não saber quando uma mudança terá fim, e acabamos por ouvir mais "foi mais complexo do que imaginávamos".
    * Mudanças não críticas no sistema acabam sendo esquecidas para evitar esse tipo de problema.
* Fragilidade
    * Mudanças na aplicação quebram muitas partes que não são relacionadas conceitualmente com a parte alterada. Isso torna o processo de manunteção muito custoso e complexo.
* Imobilidade
    * Partes do software que poderiam ser reutilizadas em outras partes da aplicação, ou extraídas para uso em outra aplicação, estão tão acopladas ao código que o esforço de reescrever é menor do que separar extrair o módulo.
* Viscosidade
    * É mais fácil implementar a forma errada do que a forma certa. É difícil preservar o projeto de classes.
* Complexidade desnecessária
    * Uma complexidade trazida por BDUF ao invés de uma abordagem iterativa e incremental (arquitetura emergente), quando o projeto de classes é flexível ou sofisticado demais.
    * Uma nova pessoa no time não deve gastar muito tempo para aprender o projeto de classes para conseguir estende-lo ou mante-lo de forma confortável.
* Repetição desnecessária
    * Quando há código repetido é sinal de que alguma abstração não foi capturada de forma correta no projeto de classes.
    * Acarreta mudanças em diferentes pontos do código quando há alguma alteração. E isso demanda o entendimento se há alguma peculiaridade nas diferentes ocorrências do código duplicado.
* Opacidade
    * É um módulo difícil de ser entendido.
    * A tendência de qualquer código é se tornar opaco com o tempo, e para isso deve haver um constante trabalho se colocar no lugar do leitor e refatorar o código de forma a deixa-lo claro.

---

## Métricas de código

* A questão é: como fazer para detectar possíveis problemas em nosso design antes que eles se tornem problemas de verdade?
* Complexidade ciclomática
* Tamanho dos métodos
* Coesão e a LCOM (Lack of Cohesion of Methods)
    * Quando parte dos métodos de uma classe utilizam parte dos atributos.
    * Em alto nível, ela conta o número desses diferentes conjuntos de responsabilidades dentro de uma classe. Como o próprio nome diz, ela mede a falta de coesão de uma classe, ou seja, quanto maior esse número, menos coesa a classe é.
    * Existem várias versões dessa métrica. Todas elas tentando contornar possíveis problemas, e a mais aceita hoje pela comunidade é a LCOM HS
* Acoplamento aferente e eferente
    * Quando uma classe depende de diversas outras classes, dizemos que esse é o acoplamento eferente. Quanto maior o acoplamento eferente de uma classe, mais ela depende de outras classes e, por consequência, mais frágil ela é. Conte o número de acoplamentos referentes. Se for muito alto, há algo errado na modelagem
    * O outro lado do acoplamento é o que chamamos de acoplamento aferente. Ele mede quantas classes dependem da classe principal. É esse o acoplamento com que nos preocupamos quando queremos ver se a classe é estável ou não. Afinal, se ela é importante para o resto do sistema, provavelmente você não a alterará com frequência; ela é estável.
        * Ex: A classe List do java tem acoplamento eferente 0, pois não depende de ninguém. Mas o acoplamento aferente é alto, já que várias classes dependem dela.
    * Encontrar uma classe cujo acoplamento aferente é alto, assim como sua complexidade ciclomática, nos dá indício de uma classe muito reutilizada ao longo do código, mas que é complicada. Talvez ela mereça ser refatorada.
* Má nomenclatura
* Como avaliar os números encontrados? Qual o limite para dizermos que a classe tem problemas de acoplamento ou de complexidade?
    * Você pode usar algum número mágico. Apesar da minha crítica a eles, é uma possível maneira. Na literatura, você encontra números ideais para essas métricas. Basta você compará-los com os seus.
* Ferramentas
    * Lembre-se que a ferramenta não importa, mas sim como você analisará os dados gerados por ela

---

## Avaliando o uso do Kotlin

### Definitions

* A method is a function associated with a class
    * A function can be:
        * At top-level (top-level functions)
        * In a class (member functions)
        * In a function (local functions)
* Member is an element defined in a class
* Extensions are like fake added members to an existing class: they are elements defined outside of a class, but they are called like members
    * Both members and extensions are associated with a class and so they are both methods
* The parameter is a variable defined in a function declaration. The argument is the actual value of this variable that gets passed to the function
* secondary constructor - constructor that calls the primary constructor using 'this' keyword

---

### Safety

* Item 1: Limit mutability
    * A class with many mutating points that depend on each other is often really hard to understand and to modify.
    * It requires proper synchronization in multithreaded programs

* Item 2: Minimize the scope of variables
* Item 3: Eliminate platform types as soon as possible. These are types provided by another programming language
* Item 4: Do not expose inferred types. If we assign an inferred type to a generic class, it will have the exact instance type.
* Item 5: [Specify your expectations on arguments and state](https://gist.github.com/augustovictor/5d2befc9a9782ff5253620bb3eb63e73)
* Item 6: Prefer standard errors to custom ones
* Item 7: Prefer null or Failure result when the lack of result is possible
* Item 8: Handle nulls properly
    * ‘also' and ‘let’ are a better choice for additional operations or handling nullable values
* Item 9: Close resources with use
* Item 10: Write unit tests
* Item 11: Design for readability (Reduce cognitive load)
* Item 12: Operator meaning should be consistent with its function name. Use infix, a function with explicit name, or a top level  function
* Item 13: Avoid returning or operating on `Unit?` The only case the author identified was to use Elvis operator or safe call.
* Item 14: Specify the variable type when it is not clear
* Item 15: Consider referencing receivers explicitly
* Item 16: Properties should represent state, not behavior
* Kotlin properties != java fields
* Item 17: Consider naming arguments

### Code design

### Reusability

Item 19: Do not repeat knowledge
* Logic knowledge - How a program behaves and what it looks like
* Common algorithms - Implementation of algorithms to achieve expected behavior
* Only allow repetition when things do not belong to the same context. Ex: Two similar config files for different projects in the same workspace. When wondering whether to keep code separate or abstracted ask yourself: Are they more likely going to change together or separately? If they change together, join them. If not, keep them separate.
Item 20: Do not repeat common algorithms
* Know the stdlib
* DRY
Item 21: Use property delegation to extract common property patterns
Item 22: Reuse between different platforms by extracting common modules
Chapter 4: Abstraction design

Item 23: Each function should be written in terms of a single level of abstraction
Item 24: Use abstraction to protect code against changes
* The best way to abstract a concept is to create a class to represent it. This way we decouple from type, and we can hold state. A function is good but a change in its signature affects its usage in every place.
* Always use interfaces to decouple usage from declaration
* Common ways to introduce abstractions:
    * Extracting constant
    * Wrapping behavior into a function
    * Wrapping function into a class
    * Hiding a class behind an interface
    * Wrapping universal objects into specialist
    * Using generic type parameters
    * Extracting inner classes
    * Restricting creation, for instance by forcing object creation via factory method
* Too many abstractions brings too much complexity. One may use the abstraction expecting certain behavior, when it was actually changed.
* Factors to consider when introducing abstractions:
    * Team size
    * Team experience
    * Project size
    * Feature set
    * Domain knowledge
* Suggestion: In bigger projects with more developers, it is much harder to change object creation and usage later, so we prefer more abstract solutions.

Item 25: Specify API stability
* Use SemVer
* Use Experimental meta-annotation
* Use Deprecated annotation
* Use ReplaceWith annotation to suggest a newer version of a method
* Changes in an API should follow a long process of deprecating

Item 26: Consider wrapping external APIs
* Not afraid of API contract changing since we only have a single place to change
* Adjust the API to our project style and logic
* Replaceable

Item 27: Minimize elements visibility
* Keep an API as lean as possible
    * Easier to learn and maintain
    * When making changes it is easier to expose something than to hide it
    * A class cannot be responsible for its own state when some properties can be changed from outside
* Protecting internal object state is important when we have variable depending on each other
* Elements visibility should be as restrictive as possible. There is no need to apply this to DTO.

Item 28: Define contract with documentation
* When we define an element, especially parts of external API, we should define a contract. We do that through documentation, comments, and examples.

Item 29: Respect abstraction contracts
* If you want your programs to be stable, respect contracts.
* If you are forced to break them, document this fact well
Object creation

Item 30: Consider factory functions instead of constructors
* They solve the confusion of having multiple constructors with different parameters. Ex: ArrayList(3) vs ArrayList.withSize(3)
* Can return any type of their subtypes
* We can include a caching mechanism and avoid creating a new object every time (Singleton). Or return null when an object could not be created (Transient errors) with a method named Connection.createOrNull()
* They can be inlined and their type parameters can be reified
* They CANNOT be used in subclass construction since we need to call the superclass constructor
* They are not a competitor of the primary constructor, they are a competitor of the secondary constructor instead.
* Ways to create a factory function:
    * Companion Object Factory Function
        * Ex: Date.from("2019-01-20"). Receives a single parameter and returns a corresponding interface of the same type
        * val enum: Set<String> = EnumSet.of("1", "2"). Takes multiple parameters and returns a type of the provided arguments
        * val prime: BigInteger = BigInteger.valueOf(Integer.MAX_VALUE) is an alternative to 'from' and 'of'
        * val luke: StackWalker = StackWalker.getInstance(options). Used in singleton. Returns always the same instance when the scenario is the same
        * val newArray = Array.newInstance(classObject, arrayLen). Each call returns a new instance
        * val fs: FileStore = Files.getFileStore(path). Like getInstance, but used if the factory function is in a different class
        * val br: BufferedReader = Files.newBufferedReader(path). Like newInstance, but used if the factory function is in a different class
    * Extension factory functions
        * This lets us extend external libraries with our own factory methods. But the class/interface we're extending must have at least an empty companion object
    * Top level functions
        * Good call for small and commonly created objects. Having a method lisfOf(1,2,3) is simpler and more readable than List.of(1,2,3). So we need a companion object with a function. But be careful since they are available everywhere. Choose names wisely so the top level functions do not get confused with class methods
    * Fake constructors (top level factory functions that act like a constructor)
        * Ex: List, MutableList
        * Use this to have a constructor for an Interface
        * To have reified type arguments
* Factory classes have the ability to hold state, and can speed up object creation by caching or duplicating previous objects.

Item 31: Consider a primary constructor with named optional arguments
* No need of telescoping constructor pattern (constructor overload) or builder pattern since we have named arguments and default values
* Use a primary constructor with default arguments or an expressive DSL for objects creation

Item 32: Consider defining a DSL for complex object creation
* DSLs are also often used to define data or configurations (grade)
* Function type is (a type that represents) an object that can be used as a function. Ex: () -> Unit. (Int) -> Unit.
    * valplus1:(Int,Int)->Int={a,b->a+b}
    * valplus1={a:Int,b:Int->a+b}
    * valmyPlus:Int.(Int)->Int={this+it}
* Function type with a receiver is the type of an extension function
    * Similar to a normal function type, but it additionally specifies the receiver type before its arguments and they are separated using a dot
    * You should consider using DSL when you see repeatable boilerplate code48 and there are no simpler Kotlin features that can help

Item 33: Prefer composition over inheritance
* inheritance is a great tool to represent the hierarchy of objects, but not necessarily to just reuse some common parts. For such cases, the composition is better because we can choose what behavior do we need.
* When we need both inheritance (in an ‘is-a’ relationship) but we also want a composition with the same interface we use the Delegation Pattern. The delegation pattern is when our class implements an interface, composes an object that implements the same interface, and forwards methods defined in the interface to this composed object. Such methods are called forwarding methods
* Composition is more secure - We do not depend on how a class is implemented, but only on its externally observable behavior
* Composition is more flexible - We can only extend a single class, while we can compose many
* Composition is more explicit  - Inheritance hides the origin of methods and attributes beside its hierarchy tree.
* Composition is more demanding - We need to use composed object explicitly. When we add some functionalities to a superclass we often do not need to modify subclasses. When we use composition we more often need to adjust usages.
*  we should use inheritance when there is a definite “is a” relationship. Not only linguistically, but meaning that every class that inherits from a superclass needs to “be” its superclass.

Item 34: Use the data modifier to represent a bundle of data
* Do not use Pair or Triple

Item 35: Use function types instead of interfaces to pass operations and actions
* Interfaces with a single method are called SAM (Single Abstraction Method)

Item 36: Prefer class hierarchies to tagged classes (review)
* Classes with a constant “mode” that specifies how the class should behave. We call such classes tagged as they contain a tag that specifies their mode of operation (this should be replaced by a sealed class so each behavior is bounded within a limited context)

Item 37: Respect the contract of equals
* Types of equality: Structural (==) and Referential (===)
* Data class#copy does not replicate atributes not declared in the primary constructor
* Equals implementation requirements
    * Reflexive: for any non-null value x, x.equals(x) should return true.
    * Symmetric: for any non-null values x and y, x.equals(y) should return true if and only if y.equals(x) returns true.
    * Transitive: for any non-null values x, y, and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should  return true.
    * Consistent:for any non-null values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.
    * Never equal to null: for any non-null value x, x.equals(null) should return false.

Item 38: Respect the contract of hashCode
* Read about hashtables
* Dot not use mutability on data structures based on hashes or any other DS that organizes elements based on their properties.
* When you do not have a custom equals method, do not define a custom hashCode
* Overritten hashCode should always be consistent with equals

Item 39: Respect the contract of compareTo
* Properties:
    * Antisymmetric, meaning that if a >= b and b >= a then a == b. Therefore there is a relation between comparison and equality and they need to be consistent with each other.
    * Transitive, meaning that if a >= b and b >= c then a >= c. Similarly when a > b and b > c then a > c. This property is important because without it, sorting of elements might take literally forever in some sorting algorithms.
    * Connex, meaning that there must be a relationship between every two ele- ments. So either a >= b, or b >= a. In Kotlin, it is guaranteed by typing system for compareTo because it returns Int, and every Int is either positive, negative or zero. This property is important because if there is no relationship between two elements, we cannot use classic sorting algorithms like quicksort or insertion sort. Instead, we need to use one of the special algorithms for partial orders, like topological sorting.
* Comparisons should return:
* 0 if the receiver and other are equal
* a positive number if the receiver is greater than other
* a negative number if the receiver is smaller than other

Item 40: Consider extracting non-essential parts of your API into extensions
* Extensions need to be imported
* Extensions are not virtual
* Member has a higher priority
* Extensions are on a type, not on a class
* Extensions are not listed in the class reference
* Extensions give us more freedom and flexibility. They are more noncommittal. Although they do not support inheritance, annotation  processing, and it might be confusing that they are not present in the class. Essential parts of our API should rather stay as members

Item 41: Avoid member extensions
* Especially, do not define extension as members just to restrict visibility.
* You should restrict the extension visibility using a visibility modifier and not by placing it locally.
* When we expect an extension to modify or reference a receiver, it is not clear if we modify the extension or dispatch receiver (the class in which the extension is defined)

Part 3: Efficiency

Item 42: Avoid unnecessary object creation
* `Nothing` is a subtype of every type
* mutable objects should not be cached
* Soft vs Weak reference
    * Weak references do not prevent Garbage Collector from cleaning-up the value. So once no other reference (variable) is using it, the value will be cleaned.
    * Soft references are not guaranteeing that the value won’t be cleaned up by the GC either, but in most JVM implementations, this value won’t be cleaned unless memory is needed. Soft references are perfect when we implement a cache.
* Caching is always a tradeoff: performance for memory.
* Wrap primitive types when a nullable type or a generic is needed

Item 43: Use inline modifier for functions with parameters of functional types
* Advantages of 'inline' modifier:
    * A type argument can be reified
    * Functions with functional parameters are faster when they are inline
    * Non-local return is allowed
* Functions with functional parameters are faster when they are inlined
* in most cases we don’t know how functions with parameters of functional types will be used, when we define a utility function with such parameters, for instance for collection processing, it is good practice to make it inline
* Inline functions cannot be recursive
* Inline functions cannot use elements with more restrictive visibility. We cannotuse private or internal functions or properties in a public inline function
* There are cases when we want to inline a function, but for some reason, we cannot inline all function type arguments. In such cases we can use the following modifiers:
    * crossinline - it means that the function should be inlined but non-local return is not allowed. We use it when this function is used in another scope where nonlocal return is not allowed, for instance in another lambda that is not inlined.
    * noinline - it means that this argument should not be inlined at all. It is used mainly when we use this function as an argument to another function that is not inlined.
* The main cases where we use inline functions are:
    * Very often used functions, like print.
    * Functions that need to have a reified type passed as a type argument, like filterIsInstance.
    * When we define top-level functions with parameters of functional types. Especially helper functions, like collection processing functions (like map, filter, flatMap, joinToString), scope functions (like also, apply, let), or top-level utility functions (like repeat, run, with).

Item 44: Consider using inline classes
* Objects holding a single value can be replaced with this value
* We can use inline classes to make a wrapper around some type
* Two especially popular uses of inline classes are:
    * To indicate a unit of measure
    * To use types to protect user from misuse
* When we present inline classes through an interface, such classes are not inlined
* Typealias
    *  
    * typealias lets us create another name for a type
    * it is popular practice to name repeatable function types
    * typealiases do not protect us in any way from type misuse. They are just adding a new name for a type
    * If you use a type with unclear meaning, especially a type that might have different units of measure, consider wrapping it with inline classes.

Item 45: Eliminate obsolete object references
* Forgetting about memory management altogether leads to memory leaks - unnecessary memory consumption - and in some cases to OutOfMemoryError
* The single most important rule is that we should not keep a reference to an object that is not useful anymore. Especially if such an object is big in terms of memory or if there might be a lot of instances of such objects.
* Manage dependencies properly instead of storing them statically
* When we hold state, we should have memory management in our minds.
*  Generally, readable code will also be doing fairly well in terms of performance or memory. Unreadable code is more likely to hide memory leaks or wasted CPU power. Though sometimes those two values stand in opposition and then, in most cases, readability is more important. When we develop a library, performance and memory are often more important.
* heap profiler for memory leaks analysis
* The most important way to avoid cluttering our memory is having variables defined in a local scope and not storing possibly heavy data in top-level properties or object declarations (including companion objects)

Chapter 8: Efficient collection processing

Item 46: Prefer Sequence for big collections with more than one processing step
* Sequences are lazy, so intermediate functions for Sequence processing don’t do any calculations. Instead, they return a new Sequence that decorates the previous one with the new operation. All these computations are evaluated during a terminal operation like toList or count. Iterable processing, on the other hand, returns a collection like List on every step
* Sequence processing functions are not invoked until the terminal operation (operation that returns something else but Sequence). For instance, for Sequence, filter is an intermediate operation, so it doesn’t do any calculations, but instead, it decorates the sequence with the new processing step. Calculations are done in a terminal operation like toList
* There are a few important advantages of the fact that sequences are lazy in Kotlin:
    * They keep the natural order of operations
        * In sequence processing, we take the first element and apply all the operations, then we take the next element, and so on. We will call it element-by-element or lazy order. In iterable processing, we take the first operation and we apply it to the whole collection, then when move to the next operation, etc.. We will call it step-by-step or eager order
    * They do a minimal number of operations
        * when we have some intermediate processing steps and our terminal operation does not necessarily need to iterate over all elements, using a sequence will most likely be better for the performance of your processing. Examples of such operations are first, find, take, any, all, none or indexOf
    * They can be infinite
        * we generally either limit the number of elements by take, or we ask for just the first element using first
    * They do not need to create collections at every step
        *  prefer to use Sequence for big collections with more than one processing step
        * in a typical collection processing with more than one step, for at least a couple of thousands of elements, we can expect around a 20-40% performance improvement
* There are some operations where we don’t profit from this sequence usage because we have to operate on the whole collection either way. sorted is an example
* Use Java streams rarely, only for computationally heavy processing where you can profit from the parallel mode. Otherwise, use Kotlin stdlib functions to have a homogeneous and clean code that can be used in different platforms or on common modules
* Plugin to debug sequences:
    * Kotlin Sequence Debugger
    * Java Stream Debugger

Item 47: Limit the number of operations
* For standard collection processing, it is most often another iteration over elements and additional collection created under the hood. For sequence processing, it is another object wrapping the whole sequence, and another object to keep operation

Item 48: Consider Arrays with primitives for performance-critical processing
*  Primitives are lighter, as every object adds additional weight
* Faster, as accessing the value through accessors is an additional cost
* For a 1M int values, a IntArray allocates 400 000 016 bytes, while a List<Int> allocates s 2 000 006 944 bytes. It is 5 times more.
    * When calculating an average, the processing over primitives is around 25% faster.

Item 49: Consider using mutable collections
* The biggest advantage of using mutable collections instead of immutable is that they are faster in terms of performance
* Adding all elements from a previous collection is a costly process when we deal with bigger collections. This is why using mutable collections, especially if we need to add elements, is a performance optimization.
* Although notice that those arguments rarely apply to local variables where synchronisation or encapsulation is rarely needed. This is why for local processing, it generally makes more sense to use mutable collections

---

## Avaliando os testes

### Estrutura dos testes

* Se um método tem testes demais, é um sinal que ele possui muitos comportamentos e tendem a alterar muitos atributos da classe.
* Se uma classe possui testes demais, é um sinal que ela possui muitas responsabilidades. Classes que possuem muitos métodos públicos tendem a ter muitas responsabilidades.
* A necessidade de criar cenários de testes muito grandes para uma classe nos leva a entender que essa classe lidam com muitos objetos
* Querer testar métodos privados é um indício de que o método poderia estar em outra classe
* Um teste de classe que tem muitos mocks indica que a classe testada é instável pois depende de muitas classes
* Para 'get' de informações podemos ignorar a lei de demeter, mas para usar commands é bom encapsular.

Testes de integração
* Faça uso de mocks quando usar a instância real for complexo ou trabalhoso. Caso não seja, faça uso do objeto real

O TDD faz sentido quando queremos testar classes complexas ou algorítmos. Quando estamos falando de código de infraestrutura, normalmente é um comportamento padrão, em que a forma de escrever a classe não irá mudar dependendo do TDD.

Ao praticar o TDD, criamos interfaces que representam a interação com o sistema, e não workarounds para resolver um problema. Pratique o TDD quando houver a necessidade de feedback constante.


### Regras de negócio (happy path X edge cases)
