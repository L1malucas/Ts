# Semana 1: Fundamentos Críticos e Classes  

## Visão Geral da Semana
Nesta primeira semana, nosso objetivo é construir uma base sólida em TypeScript. Vamos focar em dominar o sistema de classes, entender o comportamento do `this` e, crucialmente, começar a jornada para eliminar o `any` do nosso código, substituindo-o por padrões de tipagem seguros e robustos como `unknown` e type guards. Ao final da semana, você terá as ferramentas para construir componentes encapsulados e reutilizáveis.

---

## Dia 1: Classes Fundamentais

### Foco do Dia
Construir e entender a estrutura de classes em TypeScript, incluindo como inicializar, controlar o acesso a propriedades e definir o comportamento através de métodos.

### Leitura e Teoria (Aprofundada)
Classes são um dos pilares da programação orientada a objetos. Elas são "plantas" para criar objetos.

- **Propriedades (Properties)**: São as variáveis de uma classe. Elas definem o estado de um objeto.
- **Métodos (Methods)**: São as funções de uma classe. Eles definem o comportamento de um objeto.
- **Construtor (Constructor)**: Um método especial para criar e inicializar um objeto. Ele é chamado automaticamente quando usamos a palavra-chave `new`.
- **Modificadores de Acesso (Access Modifiers)**:
  - `public`: (padrão) A propriedade ou método pode ser acessado de qualquer lugar.
  - `private`: A propriedade ou método só pode ser acessado de **dentro da própria classe**. Isso é chamado de **encapsulamento** e é crucial para proteger os dados e esconder a complexidade.
  - `protected`: Pode ser acessado de dentro da classe e de classes que a herdam (`extends`). Veremos mais sobre herança na Semana 2.
- **`readonly`**: Uma propriedade marcada como `readonly` só pode receber um valor durante a sua declaração ou dentro do construtor. Garante **imutabilidade** após a criação do objeto.
- **Parameter Properties**: Um atalho do TypeScript para declarar e inicializar propriedades diretamente nos parâmetros do construtor.

### Documentação Essencial
- [Classes (TypeScript Handbook)](https://www.typescriptlang.org/docs/handbook/2/classes.html)
- [Parameter Properties (TypeScript Handbook)](https://www.typescriptlang.org/docs/handbook/2/classes.html#parameter-properties)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Classe Básica**
```typescript
class Player {
  // Propriedade pública, acessível de qualquer lugar
  health: number = 100;

  // Método público
  attack() {
    console.log("O jogador ataca!");
  }
}

const player1 = new Player();
console.log(player1.health); // 100
player1.attack(); // "O jogador ataca!"
```

**Exemplo 2: Encapsulamento com `private` e `readonly`**
```typescript
class BankAccount {
  public readonly accountNumber: string;
  private balance: number;

  constructor(accountNumber: string, initialBalance: number) {
    this.accountNumber = accountNumber;
    this.balance = initialBalance;
  }

  // Método público para acessar um dado privado de forma controlada
  public getBalance(): number {
    // Aqui poderíamos adicionar lógica de permissão, por exemplo
    return this.balance;
  }

  // Método público para modificar um dado privado
  public deposit(amount: number): void {
    if (amount > 0) {
      this.balance += amount;
    }
  }
}

const myAccount = new BankAccount("12345-6", 500);
// myAccount.balance = 10000; // Erro: 'balance' é privado.
// myAccount.accountNumber = "98765-4"; // Erro: 'accountNumber' é readonly.
myAccount.deposit(150);
console.log(myAccount.getBalance()); // 650
```

**Exemplo 3: Atalho com Parameter Properties**
```typescript
class Car {
  // Declara e inicializa as propriedades diretamente no construtor
  constructor(
    public readonly model: string,
    private year: number
  ) {}

  public getCarInfo(): string {
    return `Carro: ${this.model}, Ano: ${this.year}`;
  }
}

const myCar = new Car("Fusca", 1978);
console.log(myCar.model); // "Fusca"
console.log(myCar.getCarInfo()); // "Carro: Fusca, Ano: 1978"
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma classe `Rectangle` com propriedades `width` e `height` (ambas `public` e do tipo `number`). Adicione um método `getArea()` que retorna a área do retângulo (`width * height`).

<details>
<summary>Ver Solução</summary>

```typescript
class Rectangle {
  public width: number;
  public height: number;

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
  }

  public getArea(): number {
    return this.width * this.height;
  }
}

// Teste
const rect = new Rectangle(10, 20);
console.log(rect.getArea()); // 200
```
</details>

**Nível 2: Intermediário**
Crie uma classe `Product` com propriedades `name` (string) e `price` (number), ambas `private`. Use *parameter properties*. Adicione um método público `getPrice()` para ler o preço e um método `setPrice(newPrice: number)` que só permite a alteração se o `newPrice` for maior que zero.

<details>
<summary>Ver Solução</summary>

```typescript
class Product {
  constructor(
    private name: string,
    private price: number
  ) {}

  public getPrice(): number {
    return this.price;
  }

  public setPrice(newPrice: number): void {
    if (newPrice > 0) {
      this.price = newPrice;
      console.log(`O preço de ${this.name} foi atualizado para ${newPrice}.`);
    } else {
      console.log("Preço inválido. O valor deve ser maior que zero.");
    }
  }
}

// Teste
const book = new Product("O Senhor dos Anéis", 50);
console.log(book.getPrice()); // 50
book.setPrice(65);
console.log(book.getPrice()); // 65
book.setPrice(-10); // "Preço inválido..."
```
</details>

**Nível 3: Avançado**
Implemente a classe `DatabaseConnection` que simula o padrão de *constructor overloading*. A classe deve poder ser instanciada de duas formas:
1.  `new DatabaseConnection(url: string)`
2.  `new DatabaseConnection(host: string, port: number, database: string)`
O construtor deve ter uma única implementação que verifica os argumentos recebidos e monta a `connectionString` interna de acordo.

<details>
<summary>Ver Solução</summary>

```typescript
class DatabaseConnection {
  private connectionString: string;

  // Assinatura de sobrecarga 1
  constructor(url: string);
  // Assinatura de sobrecarga 2
  constructor(host: string, port: number, database: string);
  // Implementação real do construtor
  constructor(arg1: string, arg2?: number, arg3?: string) {
    // Verifica se os argumentos correspondem à segunda assinatura
    if (typeof arg2 === 'number' && typeof arg3 === 'string') {
      const host = arg1;
      const port = arg2;
      const database = arg3;
      this.connectionString = `mongodb://${host}:${port}/${database}`;
    } else {
      // Caso contrário, trata como a primeira assinatura
      const url = arg1;
      this.connectionString = url;
    }
  }

  public connect(): void {
    console.log(`Conectando a: ${this.connectionString}`);
  }
}

// Teste
const connFromUrl = new DatabaseConnection('mysql://user:pass@server/db');
connFromUrl.connect(); // "Conectando a: mysql://user:pass@server/db"

const connFromParts = new DatabaseConnection('localhost', 5432, 'postgres');
connFromParts.connect(); // "Conectando a: postgresql://localhost:5432/postgres"
```
</details>

### Checklist do Dia
- [ ] Entendi a diferença entre `public` e `private`.
- [ ] Sei por que `readonly` é útil para imutabilidade.
- [ ] Usei o atalho de *parameter properties*.
- [ ] Implementei o padrão de "overload" de construtor em TypeScript.

---

## Dia 2: Contexto do `this` - Parte 1

### Foco do Dia
Entender como o `this` funciona em JavaScript/TypeScript e como garantir que ele se refira ao contexto correto, especialmente em callbacks e métodos encadeados.

### Leitura e Teoria (Aprofundada)
O `this` é uma das fontes mais comuns de bugs em JavaScript. Seu valor é determinado por **como a função é chamada (call-site)**, não onde ela é definida.

- **Função Regular (`function() {}` ou `metodo() {}`)**: O `this` é dinâmico. Se a função é chamada como `obj.metodo()`, `this` é `obj`. Se a função é simplesmente chamada (`funcao()`), `this` é `undefined` (em 'strict mode', o padrão em módulos e classes) ou o objeto global (`window` no browser).

- **Arrow Function (`() => {}`)**: O `this` é estático (léxico). Ela **não possui seu próprio `this`**. Ela "herda" o `this` do escopo onde foi **definida**. Dentro de um método de classe definido como arrow function, `this` **sempre** se referirá à instância da classe.

### Documentação Essencial
- [O `this` em JavaScript (MDN)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/this) - Essencial para entender a base.
- [This Types (TypeScript Handbook)](https://www.typescriptlang.org/docs/handbook/2/classes.html#this-types)

### Prática Guiada (Passo a Passo)

**Exemplo 1: O Problema - Perdendo o `this`**
```typescript
class Greeter {
  prefix = "Hello, ";

  // Método de classe normal
  greet(name: string) {
    console.log(this.prefix + name);
  }
}

const greeter = new Greeter();
const greetFunction = greeter.greet; // A função é extraída do objeto

// greetFunction("Mundo"); // Crash! `this` é undefined aqui.
```

**Exemplo 2: A Solução - Arrow Function como Método**
```typescript
class SafeGreeter {
  prefix = "Hello, ";

  // O método é uma propriedade que contém uma arrow function
  // A arrow function "lembra" do `this` de onde foi criada
  greet = (name: string) => {
    console.log(this.prefix + name);
  }
}

const safeGreeter = new SafeGreeter();
const safeGreetFunction = safeGreeter.greet;

safeGreetFunction("Mundo"); // Funciona! "Hello, Mundo"
```

**Exemplo 3: Method Chaining com o tipo `this`**
```typescript
class StringBuilder {
  private parts: string[] = [];

  add(part: string): this {
    this.parts.push(part);
    return this; // Retornar `this` permite o encadeamento
  }

  build(): string {
    return this.parts.join("");
  }
}

const builder = new StringBuilder();
const result = builder.add("Hello, ").add("World!").build();
console.log(result); // "Hello, World!"
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma classe `Logger` com uma propriedade `message`. Crie um método `printMessage()`. Chame este método dentro de um `setTimeout` e observe o erro. Em seguida, corrija o problema para que a mensagem seja impressa corretamente após 1 segundo.

<details>
<summary>Ver Solução</summary>

```typescript
class Logger {
  constructor(private message: string = "Operação concluída!") {}

  // A solução é definir o método como uma arrow function
  printMessage = () => {
    console.log(this.message);
  }

  public schedulePrint(): void {
    console.log("Agendando impressão...");
    setTimeout(this.printMessage, 1000);
  }
}

// Teste
const logger = new Logger();
logger.schedulePrint(); // Após 1s: "Operação concluída!"
```
</details>

**Nível 2: Intermediário**
Crie uma classe `Calculator` com uma propriedade `value` (number, private, inicia em 0). Crie os métodos `add(num: number)`, `subtract(num: number)` e `multiply(num: number)`. Cada um desses métodos deve modificar o `value` e retornar `this` para permitir o encadeamento de chamadas.

<details>
<summary>Ver Solução</summary>

```typescript
class Calculator {
  private value: number = 0;

  constructor(initialValue: number = 0) {
    this.value = initialValue;
  }

  add(num: number): this {
    this.value += num;
    return this;
  }

  subtract(num: number): this {
    this.value -= num;
    return this;
  }

  multiply(num: number): this {
    this.value *= num;
    return this;
  }

  getResult(): number {
    return this.value;
  }
}

// Teste
const calc = new Calculator(10);
const result = calc.add(5).subtract(3).multiply(2).getResult(); // (10 + 5 - 3) * 2 = 24
console.log(result); // 24
```
</details>

**Nível 3: Avançado**
Crie uma classe `DOMManager`. Ela deve ter um método `createElement(tag: string, text: string)` que cria um elemento (simulado por um objeto `{tag, text}`) e o armazena em um array `private elements`. Crie um método `render(containerId: string)` que deveria (em um cenário real) adicionar os elementos a um contêiner do DOM. O método `render` deve ser chamado por um objeto externo, simulando um event listener, então você precisa garantir que o `this` dentro de `render` ainda se refira à instância de `DOMManager`.

<details>
<summary>Ver Solução</summary>

```typescript
interface Element {
  tag: string;
  text: string;
}

class DOMManager {
  private elements: Element[] = [];

  public createElement(tag: string, text: string): this {
    this.elements.push({ tag, text });
    return this;
  }

  // Definido como arrow function para garantir o `this` léxico
  public render = (containerId: string) => {
    // Em um app real, faríamos: const container = document.getElementById(containerId);
    console.log(`Renderizando ${this.elements.length} elementos em #${containerId}`);
    for (const el of this.elements) {
      console.log(`  <${el.tag}>${el.text}</${el.tag}>`);
    }
  }
}

// Teste
const manager = new DOMManager();
manager.createElement("h1", "Título Principal").createElement("p", "Este é um parágrafo.");

// Simulando um sistema de eventos que pega a função e a chama depois
const eventSystem = {
  listeners: {} as Record<string, () => void>,
  addEventListener: function(eventName: string, callback: () => void) {
    this.listeners[eventName] = callback;
  },
  trigger: function(eventName: string) {
    this.listeners[eventName]();
  }
};

// Passamos o método `render` como callback. Graças à arrow function, o `this` funciona.
eventSystem.addEventListener("renderPage", () => manager.render("app-root"));
eventSystem.trigger("renderPage");
```
</details>

### Checklist do Dia
- [ ] Sei explicar a diferença de `this` entre `function()` e `() => {}`.
- [ ] Consigo resolver um problema de `this` perdido em um callback.
- [ ] Entendi como `return this;` permite o encadeamento de métodos.
- [ ] Implementei uma classe com uma API fluente.

---

## Dia 3: Eliminando `any` - Parte 1

### Foco do Dia
Abandonar o `any` e adotar `unknown` como a alternativa segura para tipos desconhecidos, forçando a verificação de tipos antes do uso.

### Leitura e Teoria (Aprofundada)
- **`any`**: É a "válvula de escape" do TypeScript. Uma variável do tipo `any` **desliga completamente a verificação de tipos**. Você pode chamar qualquer método, acessar qualquer propriedade, e o compilador não vai reclamar. Isso é perigoso e anula o propósito de usar TypeScript.

- **`unknown`**: É a alternativa segura. Uma variável `unknown` também pode receber qualquer valor, mas você **não pode fazer nada** com ela sem antes **provar** ao TypeScript qual é o seu tipo. Esse processo de prova é chamado de *narrowing* (estreitamento).

| Característica | `any` | `unknown` |
| :--- | :--- | :--- |
| **Atribuição** | Pode receber qualquer valor | Pode receber qualquer valor |
| **Operações** | Permite qualquer operação | **Não permite nenhuma operação** |
| **Segurança** | Baixa | Alta |
| **Necessidade** | Nenhuma verificação | **Requer verificação de tipo** |

### Documentação Essencial
- [The `unknown` Type (Handbook)](https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown)
- [The `any` Type (Handbook)](https://www.typescriptlang.org/docs/handbook/2/functions.html#any)

### Prática Guiada (Passo a Passo)

**Exemplo 1: O Perigo do `any`**
```typescript
let value: any = "isto é uma string";
// Nenhuma verificação do compilador!
value.toFixed(2); // Crash em tempo de execução: value.toFixed is not a function
```

**Exemplo 2: A Segurança do `unknown`**
```typescript
let safeValue: unknown = "isto é uma string";
// Erro de compilação! O TS nos protege.
// safeValue.toFixed(2); // Object is of type 'unknown'.
```

**Exemplo 3: Usando `unknown` Corretamente com Verificação**
```typescript
let anotherSafeValue: unknown = 123.456;

if (typeof anotherSafeValue === 'number') {
  // Dentro deste bloco, o TS sabe que anotherSafeValue é um número
  console.log(anotherSafeValue.toFixed(2)); // "123.46"
}
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma função `logString(value: unknown)` que verifica se o valor recebido é uma `string`. Se for, ela o imprime no console. Se não for, ela imprime a mensagem "Valor não é uma string".

<details>
<summary>Ver Solução</summary>

```typescript
function logString(value: unknown): void {
  if (typeof value === 'string') {
    console.log(value);
  } else {
    console.log("Valor não é uma string.");
  }
}

// Teste
logString("Olá, TypeScript!"); // "Olá, TypeScript!"
logString(123); // "Valor não é uma string."
logString({}); // "Valor não é uma string."
```
</details>

**Nível 2: Intermediário**
Crie uma função `getDouble(value: unknown): number | null` que verifica se o valor é um `number`. Se for, a função deve retornar o dobro do valor. Caso contrário, deve retornar `null`.

<details>
<summary>Ver Solução</summary>

```typescript
function getDouble(value: unknown): number | null {
  if (typeof value === 'number') {
    return value * 2;
  }
  return null;
}

// Teste
console.log(getDouble(10)); // 20
console.log(getDouble("20")); // null
console.log(getDouble(null)); // null
```
</details>

**Nível 3: Avançado**
Crie uma função `safeJsonParse(json: string): unknown | Error`. Esta função deve receber uma string JSON, tentar fazer o parse com `JSON.parse`. Se for bem-sucedido, deve retornar o objeto parseado (como `unknown`). Se ocorrer um erro no parse, ela deve capturar a exceção e retornar um objeto `Error`.

<details>
<summary>Ver Solução</summary>

```typescript
function safeJsonParse(json: string): unknown | Error {
  try {
    // JSON.parse por padrão retorna `any`, mas nós o atribuímos a `unknown`
    // para forçar quem chama a função a verificar o tipo do resultado.
    const parsed: unknown = JSON.parse(json);
    return parsed;
  } catch (e) {
    if (e instanceof Error) {
      return e;
    }
    return new Error("Falha desconhecida ao analisar JSON.");
  }
}

// Teste
const validJson = '{"name": "Lucas", "id": 1}';
const invalidJson = '{'name': "Lucas"}'; // JSON malformado

const result1 = safeJsonParse(validJson);
const result2 = safeJsonParse(invalidJson);

if (result1 instanceof Error) {
  console.error("Erro no JSON válido:", result1.message);
} else {
  console.log("Sucesso no JSON válido:", result1);
}

if (result2 instanceof Error) {
  console.error("Erro no JSON inválido:", result2.message);
} else {
  console.log("Sucesso no JSON inválido:", result2);
}
```
</details>

### Checklist do Dia
- [ ] Entendi por que `any` é perigoso e deve ser evitado.
- [ ] Sei a diferença fundamental entre `any` e `unknown`.
- [ ] Usei `typeof` para fazer *narrowing* de um tipo `unknown`.
- [ ] Implementei uma função que lida com dados de tipo desconhecido de forma segura.

---

## Dia 4: Type Guards e Narrowing

### Foco do Dia
Aprender as técnicas de *narrowing* (estreitamento de tipo) para que o TypeScript possa inferir um tipo mais específico dentro de um bloco de código, e criar seus próprios `type guards` customizados.

### Leitura e Teoria (Aprofundada)
*Narrowing* é o processo pelo qual o TypeScript remove tipos de uma união. Se você tem `string | number`, e o TS prova que é `string`, ele "estreita" o tipo para apenas `string` naquele escopo.

**Técnicas de Narrowing:**
1.  **`typeof`**: Para tipos primitivos (`string`, `number`, `boolean`, etc.).
2.  **`instanceof`**: Para verificar se um objeto é uma instância de uma classe.
3.  **Truthiness**: Verificar se um valor não é `null`, `undefined`, `false`, `0`, `""`.
4.  **Equality (`===`)**: Verificar igualdade com um valor literal.
5.  **`in` operator**: Verificar se um objeto possui uma propriedade com um certo nome.
6.  **Discriminated Unions**: Um padrão poderoso onde você usa uma propriedade literal comum (`kind`, `type`, `status`) em vários tipos para ajudar o TypeScript a descobrir qual tipo é.
7.  **Custom Type Guards (Type Predicates)**: Funções que retornam um booleano especial: `parametro is Tipo`. Se a função retornar `true`, o TypeScript "confia" que o parâmetro é daquele `Tipo` no resto do escopo.

### Documentação Essencial
- [Narrowing (Handbook)](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [Using Type Predicates (Custom Type Guards)](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates)

### Prática Guiada (Passo a Passo)

**Exemplo 1: `typeof`**
```typescript
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}
```

**Exemplo 2: `instanceof`**
```typescript
class Cat { meow() { console.log("Meow!"); } }
class Dog { bark() { console.log("Woof!"); } }
type Pet = Cat | Dog;

function makeSound(pet: Pet) {
  if (pet instanceof Cat) {
    pet.meow(); // O TS sabe que `pet` é um Cat aqui
  }
}
```

**Exemplo 3: `in` operator**
```typescript
interface Movie { title: string; duration: number; }
interface TVShow { title: string; seasons: number; }
type Media = Movie | TVShow;

function getMediaTitle(media: Media) {
  if ('duration' in media) {
    return `Filme: ${media.title}`;
  }
  // O TS sabe que se não tem 'duration', deve ter 'seasons'
  return `Série: ${media.title}`;
}
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma função `formatInput(input: string | string[])`. Se o `input` for uma `string`, retorne a própria string. Se for um array de strings, retorne as strings unidas por um espaço. Use o type guard `Array.isArray()`.

<details>
<summary>Ver Solução</summary>

```typescript
function formatInput(input: string | string[]): string {
  if (Array.isArray(input)) {
    // O TS sabe que `input` é string[] aqui
    return input.join(' ');
  }
  // O TS sabe que `input` é string aqui
  return input;
}

// Teste
console.log(formatInput("hello")); // "hello"
console.log(formatInput(["hello", "world"])); // "hello world"
```
</details>

**Nível 2: Intermediário**
Defina duas interfaces: `Fish` com um método `swim()` e `Bird` com um método `fly()`. Crie uma função `makePetMove(pet: Fish | Bird)`. Dentro dela, use um **custom type guard** `isFish(pet): pet is Fish` para verificar o tipo do animal e chamar o método correto (`swim` ou `fly`).

<details>
<summary>Ver Solução</summary>

```typescript
interface Fish {
  swim: () => void;
}

interface Bird {
  fly: () => void;
}

type Animal = Fish | Bird;

// Custom type guard
function isFish(pet: Animal): pet is Fish {
  // Se o pet tem a propriedade 'swim', então ele é um Fish.
  // A asserção `(pet as Fish)` é necessária para checar a propriedade
  // que pode não existir no tipo Animal.
  return (pet as Fish).swim !== undefined;
}

function makePetMove(pet: Animal) {
  if (isFish(pet)) {
    pet.swim();
  } else {
    pet.fly();
  }
}

// Teste
const nemo: Fish = { swim: () => console.log("Nemo está nadando.") };
const zazu: Bird = { fly: () => console.log("Zazu está voando.") };

makePetMove(nemo);
makePetMove(zazu);
```
</details>

**Nível 3: Avançado**
Implemente o padrão *Discriminated Union*. Crie duas interfaces, `SuccessResponse` e `ErrorResponse`. Ambas devem ter uma propriedade `status`, mas com valores literais diferentes (`'success'` e `'error'`). Crie uma função `handleApiResponse(response: ApiResponse)` que usa a propriedade `status` para identificar o tipo da resposta e logar os dados ou a mensagem de erro apropriada.

<details>
<summary>Ver Solução</summary>

```typescript
interface SuccessResponse {
  status: 'success'; // Propriedade discriminante
  data: { id: number; name: string };
}

interface ErrorResponse {
  status: 'error'; // Propriedade discriminante
  error: { code: number; message: string };
}

type ApiResponse = SuccessResponse | ErrorResponse;

function handleApiResponse(response: ApiResponse) {
  // Usando um switch na propriedade discriminante
  switch (response.status) {
    case 'success':
      // O TS sabe que `response` é SuccessResponse aqui
      console.log("Dados recebidos:", response.data.name);
      break;
    case 'error':
      // O TS sabe que `response` é ErrorResponse aqui
      console.error("Ocorreu um erro:", response.error.message);
      break;
  }
}

// Teste
const success: ApiResponse = { status: 'success', data: { id: 1, name: 'Produto A' } };
const failure: ApiResponse = { status: 'error', error: { code: 404, message: 'Produto não encontrado' } };

handleApiResponse(success);
handleApiResponse(failure);
```
</details>

### Checklist do Dia
- [ ] Sei usar `typeof`, `instanceof` e `in` para narrowing.
- [ ] Entendi o padrão de *Discriminated Unions*.
- [ ] Criei e usei um *custom type guard* (`is Type`).
- [ ] Apliquei narrowing para acessar propriedades de tipos em uma união de forma segura.

---

## Dia 5: Utility Types Nativos - Record e Básicos

### Foco do Dia
Utilizar os `Utility Types` nativos do TypeScript para transformar e criar novos tipos a partir de tipos existentes, com foco especial em `Record<K,V>`.

### Leitura e Teoria (Aprofundada)
Utility Types são ferramentas que ajudam a manipular tipos sem ter que reescrevê-los.

- **`Record<Keys, Type>`**: Constrói um tipo de objeto cujas chaves são `Keys` e os valores são `Type`. Perfeito para dicionários ou mapeamentos onde as chaves são de um conjunto conhecido.
- **`Partial<Type>`**: Constrói um tipo com todas as propriedades de `Type` definidas como **opcionais**. Útil para payloads de atualização (`update`).
- **`Required<Type>`**: O oposto de `Partial`. Torna todas as propriedades **obrigatórias**.
- **`Pick<Type, Keys>`**: Constrói um tipo "pegando" um conjunto de propriedades `Keys` de `Type`.
- **`Omit<Type, Keys>`**: O oposto de `Pick`. Constrói um tipo com todas as propriedades de `Type`, **exceto** as `Keys`.
- **`Readonly<Type>`**: Torna todas as propriedades de `Type` somente leitura.

### Documentação Essencial
- [Utility Types (Handbook)](https://www.typescriptlang.org/docs/handbook/utility-types.html)

### Prática Guiada (Passo a Passo)

**Exemplo 1: `Partial` para atualizações**
```typescript
interface User { name: string; age: number; email: string; }
function updateUser(user: User, fieldsToUpdate: Partial<User>) {
  return { ...user, ...fieldsToUpdate };
}
const user1 = { name: 'Lucas', age: 30, email: 'lucas@test.com' };
const updatedUser = updateUser(user1, { age: 31 });
```

**Exemplo 2: `Pick` para dados públicos**
```typescript
interface UserWithPassword { id: number; name: string; passwordHash: string; }
type PublicUser = Pick<UserWithPassword, 'id' | 'name'>;
const publicProfile: PublicUser = { id: 1, name: 'Ana' };
```

**Exemplo 3: `Omit` para remover dados sensíveis**
```typescript
type UserWithoutPassword = Omit<UserWithPassword, 'passwordHash'>;
const userToSend: UserWithoutPassword = { id: 2, name: 'Beto' };
```

**Exemplo 4: `Record` para um dicionário de configurações**
```typescript
type Theme = 'light' | 'dark';
interface ThemeSettings { color: string; background: string; }
const themes: Record<Theme, ThemeSettings> = {
  light: { color: '#000', background: '#fff' },
  dark: { color: '#fff', background: '#000' },
};
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Dada a interface `Book { id: number; title: string; author: string; pages: number; }`, crie um tipo `BookPreview` que contenha apenas as propriedades `title` e `author` usando um Utility Type.

<details>
<summary>Ver Solução</summary>

```typescript
interface Book {
  id: number;
  title: string;
  author: string;
  pages: number;
}

type BookPreview = Pick<Book, 'title' | 'author'>;

// Teste
const preview: BookPreview = {
  title: "Duna",
  author: "Frank Herbert"
};
```
</details>

**Nível 2: Intermediário**
Crie uma interface `AppSettings` com todas as propriedades opcionais: `theme: string`, `fontSize: number`, `language: string`. Em seguida, crie um tipo `RequiredAppSettings` que tenha todas essas mesmas propriedades, mas como obrigatórias. Por fim, crie uma função `applySettings(settings: RequiredAppSettings)`.

<details>
<summary>Ver Solução</summary>

```typescript
interface AppSettings {
  theme?: string;
  fontSize?: number;
  language?: string;
}

type RequiredAppSettings = Required<AppSettings>;

function applySettings(settings: RequiredAppSettings) {
  console.log("Aplicando configurações:", settings);
}

// Teste
const mySettings: RequiredAppSettings = {
  theme: 'dark',
  fontSize: 14,
  language: 'pt-BR'
};

applySettings(mySettings);

// const invalidSettings: RequiredAppSettings = { theme: 'light' }; // Erro: fontSize e language estão faltando
```
</details>

**Nível 3: Avançado**
Crie um `enum` chamado `UserRole` com os valores `ADMIN`, `EDITOR`, e `VIEWER`. Crie um tipo `Permissions` que define o que cada papel pode fazer (ex: `{ canWrite: boolean; canRead: boolean; }`). Use o Utility Type `Record` para criar um objeto `rolePermissions` que mapeia cada `UserRole` para seu respectivo objeto `Permissions` de forma type-safe.

<details>
<summary>Ver Solução</summary>

```typescript
enum UserRole {
  ADMIN = 'admin',
  EDITOR = 'editor',
  VIEWER = 'viewer'
}

interface Permissions {
  canWrite: boolean;
  canRead: boolean;
  canDelete: boolean;
}

const rolePermissions: Record<UserRole, Permissions> = {
  [UserRole.ADMIN]: { canWrite: true, canRead: true, canDelete: true },
  [UserRole.EDITOR]: { canWrite: true, canRead: true, canDelete: false },
  [UserRole.VIEWER]: { canWrite: false, canRead: true, canDelete: false },
};

// Teste
function checkPermissions(role: UserRole) {
  const permissions = rolePermissions[role];
  console.log(`Permissões para ${role.toUpperCase()}:`, permissions);
}

checkPermissions(UserRole.EDITOR);
// checkPermissions('guest'); // Erro: Argument of type '"guest"' is not assignable to parameter of type 'UserRole'.
```
</details>

### Checklist do Dia
- [ ] Entendi o propósito do `Record<K, V>`.
- [ ] Usei `Record` para criar um dicionário type-safe.
- [ ] Sei a diferença entre `Pick` e `Omit`.
- [ ] Combinei `Partial` e `Required` para manipular tipos.

---

## Dia 6: Template Literal Types

### Foco do Dia
Criar tipos de string altamente específicos e dinâmicos usando Template Literals, permitindo validação de formatos de string em tempo de compilação.

### Leitura e Teoria (Aprofundada)
Template Literal Types usam a mesma sintaxe de template strings do JavaScript (`${}`), mas no nível dos tipos. Eles permitem construir novos tipos de string concatenando outros tipos, especialmente uniões.

**Com Unions (o superpoder):**
```typescript
type HttpMethod = 'GET' | 'POST' | 'PUT' | 'DELETE';
type ApiEndpoint = 'users' | 'products';

// Gera uma união de todas as 8 combinações possíveis:
// "GET /api/users" | "POST /api/users" | ...
type ApiRequestSignature = `${HttpMethod} /api/${ApiEndpoint}`;
```

**Com Helpers de Manipulação de String:**
TypeScript inclui tipos utilitários para manipular strings dentro dos tipos: `Uppercase<S>`, `Lowercase<S>`, `Capitalize<S>`, `Uncapitalize<S>`.

### Documentação Essencial
- [Template Literal Types (Handbook)](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Concatenação Simples**
```typescript
type World = "World";
type Greeting = `Hello ${World}`;
const g: Greeting = "Hello World"; // Válido
```

**Exemplo 2: União de Tipos**
```typescript
type MarginSide = 'top' | 'right' | 'bottom' | 'left';
type MarginProperty = `margin-${MarginSide}`;
const m: MarginProperty = "margin-left"; // Válido
```

**Exemplo 3: `Capitalize` para Nomes de Eventos**
```typescript
type EventName = 'click' | 'scroll' | 'focus';
type HandlerName = `on${Capitalize<EventName>}`;
const handler: HandlerName = "onClick"; // Válido
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie um tipo `Size` (`'small' | 'medium' | 'large'`) e um tipo `Color` (`'red' | 'blue' | 'green'`). Crie um terceiro tipo, `TShirtSKU`, que combine os dois para formar um identificador de produto, como por exemplo `'small-red'`.

<details>
<summary>Ver Solução</summary>

```typescript
type Size = 'small' | 'medium' | 'large';
type Color = 'red' | 'blue' | 'green';

type TShirtSKU = `${Size}-${Color}`;

// Teste
const myShirt: TShirtSKU = "medium-blue";
const anotherShirt: TShirtSKU = "large-red";
// const invalidShirt: TShirtSKU = "small-yellow"; // Erro: 'yellow' não é uma cor válida
```
</details>

**Nível 2: Intermediário**
Crie um tipo `IconName` que represente nomes de ícones no formato `icon-[nome]`, onde `[nome]` pode ser `user`, `home`, ou `settings`. Em seguida, crie um tipo `IconSize` que pode ser `16`, `24`, ou `32`. Finalmente, crie um tipo `IconId` que combine os dois, no formato `icon-[nome]-[tamanho]px`.

<details>
<summary>Ver Solução</summary>

```typescript
type IconName = `icon-${'user' | 'home' | 'settings'}`;
type IconSize = 16 | 24 | 32;

type IconId = `${IconName}-${IconSize}px`;

// Teste
const userIcon: IconId = "icon-user-24px";
const homeIcon: IconId = "icon-home-32px";
// const invalidIcon: IconId = "icon-settings-20px"; // Erro: 20 não é um tamanho válido
```
</details>

**Nível 3: Avançado**
Crie um tipo `ApiRoute` que valide os seguintes formatos de rota para uma API RESTful:
1.  Listar todos os recursos: `/api/[recurso]` (ex: `/api/users`)
2.  Obter um recurso específico: `/api/[recurso]/[id]` (ex: `/api/posts/123`)
Onde `[recurso]` pode ser `users`, `posts`, ou `products`, e `[id]` pode ser `string` ou `number`.

<details>
<summary>Ver Solução</summary>

```typescript
type Resource = 'users' | 'posts' | 'products';
type ResourceId = string | number;

type ApiRoute = `/api/${Resource}` | `/api/${Resource}/${ResourceId}`;

// Teste
const listUsers: ApiRoute = '/api/users';
const getPost: ApiRoute = '/api/posts/post-id-abc';
const getProduct: ApiRoute = '/api/products/12345';

// const invalidRoute1: ApiRoute = '/api/orders'; // Erro: 'orders' não é um Resource
// const invalidRoute2: ApiRoute = '/api/users/1/comments'; // Erro: formato inválido
```
</details>

### Checklist do Dia
- [ ] Criei um tipo de string simples com template literals.
- [ ] Usei uniões para gerar múltiplas strings possíveis a partir de um template.
- [ ] Usei `Capitalize` e outras helpers para transformar strings em tipos.
- [ ] Construí um tipo prático para validar um formato de string (rotas de API).

---

## Dia 7: Projeto Mini #1 - Consolidando Conceitos

### Foco do Dia
Consolidar todos os conceitos da semana (Classes, `this`, `unknown`, Type Guards, Utility Types) em um único projeto prático: um `BaseRepository` genérico para operações CRUD em memória.

### Leitura e Teoria (Revisão)
- **Classes e Generics (`<T>`)**: Para criar uma "planta" reutilizável.
- **Constraints (`extends`)**: Para garantir que o tipo `T` tenha as propriedades que precisamos (como `id`).
- **`Record<K, V>`**: Para criar um armazenamento em memória type-safe.
- **`Partial` e `Omit`**: Para criar um payload de atualização seguro.
- **Type Guards**: Para verificar se um item existe antes de realizar uma operação.

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma classe `StringStore` **não genérica**. Ela deve ter um array privado para armazenar strings. Implemente dois métodos: `add(item: string)` e `getAll(): string[]`.

<details>
<summary>Ver Solução</summary>

```typescript
class StringStore {
  private data: string[] = [];

  public add(item: string): void {
    this.data.push(item);
  }

  public getAll(): string[] {
    return this.data;
  }
}

// Teste
const store = new StringStore();
store.add("maçã");
store.add("banana");
console.log(store.getAll()); // ["maçã", "banana"]
```
</details>

**Nível 2: Intermediário**
Crie uma classe `KeyValueStore<V>` que seja genérica para o **valor** (`V`), mas a chave seja sempre `string`. Use um `Record<string, V>` para o armazenamento privado. Implemente os métodos `set(key: string, value: V)` e `get(key: string): V | undefined`.

<details>
<summary>Ver Solução</summary>

```typescript
class KeyValueStore<V> {
  private data: Record<string, V> = {};

  public set(key: string, value: V): void {
    this.data[key] = value;
  }

  public get(key: string): V | undefined {
    return this.data[key];
  }
}

// Teste com números
const userAges = new KeyValueStore<number>();
userAges.set("Lucas", 30);
userAges.set("Ana", 25);
console.log(userAges.get("Lucas")); // 30

// Teste com objetos
interface User { id: number; name: string; }
const users = new KeyValueStore<User>();
users.set("user-1", { id: 1, name: "Lucas" });
console.log(users.get("user-1")); // { id: 1, name: "Lucas" }
```
</details>

**Nível 3: Avançado**
Implemente a classe `BaseRepository<T>`. Esta classe deve ser genérica e funcionar para qualquer tipo `T` que satisfaça a constraint `BaseEntity` (que possui uma propriedade `id` do tipo `string | number`).
**Requisitos:**
1.  Crie a interface `BaseEntity { id: string | number; }`.
2.  A classe `BaseRepository<T extends BaseEntity>` deve usar um `Record` para armazenamento privado.
3.  Implemente os métodos: `create(item: T)`, `findById(id: T['id'])`, `update(id: T['id'], payload: Partial<Omit<T, 'id'>>)`, e `delete(id: T['id'])`.

<details>
<summary>Ver Solução</summary>

```typescript
// A constraint que todas as entidades devem seguir
interface BaseEntity {
  id: string | number;
}

class BaseRepository<T extends BaseEntity> {
  private data: Record<T['id'], T> = {} as Record<T['id'], T>;

  public create(item: T): T {
    this.data[item.id] = item;
    return item;
  }

  public findById(id: T['id']): T | undefined {
    return this.data[id];
  }

  public update(id: T['id'], payload: Partial<Omit<T, 'id'>>): T | undefined {
    const currentItem = this.findById(id);
    if (!currentItem) {
      return undefined;
    }

    const updatedItem = { ...currentItem, ...payload } as T;
    this.data[id] = updatedItem;
    return updatedItem;
  }

  public delete(id: T['id']): boolean {
    if (!this.findById(id)) {
      return false;
    }
    delete this.data[id];
    return true;
  }

  public findAll(): T[] {
    return Object.values(this.data);
  }
}

// Teste
interface Post extends BaseEntity {
  id: string;
  title: string;
  content: string;
}

const postRepository = new BaseRepository<Post>();
postRepository.create({ id: 'post-1', title: "Primeiro Post", content: "..." });
postRepository.create({ id: 'post-2', title: "Segundo Post", content: "..." });

postRepository.update('post-1', { title: "Título Atualizado" });
postRepository.delete('post-2');

console.log(postRepository.findAll());
// [ { id: 'post-1', title: 'Título Atualizado', content: '...' } ]
```
</details>

### Checklist do Dia
- [ ] Criei uma classe genérica com constraints (`extends`).
- [ ] Usei `Record` como um armazenamento em memória type-safe.
- [ ] Implementei métodos CRUD (Create, Read, Update, Delete).
- [ ] Usei `Partial` e `Omit` para criar um payload de atualização seguro.
- [ ] Sinto-me mais confiante para aplicar os conceitos da Semana 1 em um projeto real.
