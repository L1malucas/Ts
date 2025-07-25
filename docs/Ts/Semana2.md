# Semana 2: Classes Avançadas e Generics  

## Visão Geral da Semana
Com os fundamentos sólidos, a segunda semana mergulha em conceitos mais poderosos e abstratos. Vamos explorar herança com `abstract classes`, o poder da reutilização de código com `generics`, o uso avançado do `this`, e introduzir `decorators` para metaprogramação. Continuaremos nossa missão de eliminar o `any` com tipos mais sofisticados como `never` e `branded types`, e finalizaremos com a introdução aos `conditional types`, uma das ferramentas mais avançadas do TypeScript.

---

## Dia 8: Herança e Abstract Classes

### Foco do Dia
Entender como criar hierarquias de classes usando herança (`extends`) e como definir "contratos" de classes com `abstract classes`.

### Leitura e Teoria (Aprofundada)
- **Herança (`extends`)**: Permite que uma classe (subclasse ou classe filha) herde propriedades e métodos de outra classe (superclasse ou classe pai). Isso promove a reutilização de código.
- **`super()`**: Dentro do `constructor` de uma subclasse, `super()` é usado para chamar o `constructor` da classe pai. Isso é obrigatório antes de usar a palavra-chave `this` na subclasse.
- **Sobrescrita de Métodos (Method Overriding)**: Uma subclasse pode fornecer sua própria implementação de um método que já existe na classe pai.
- **Classes Abstratas (`abstract class`)**: São classes que não podem ser instanciadas diretamente. Elas servem como um modelo base para outras classes. Podem conter `métodos abstratos`, que são métodos sem implementação que **devem** ser implementados pelas subclasses.

### Documentação Essencial
- [Herança de Classes (MDN)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Classes/extends)
- [Abstract Classes and Members (Handbook)](https://www.typescriptlang.org/docs/handbook/2/classes.html#abstract-classes-and-members)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Herança Simples**
```typescript
class Animal {
  constructor(public name: string) {}

  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moveu ${distanceInMeters}m.`);
  }
}

class Dog extends Animal {
  // O construtor da subclasse
  constructor(name: string) {
    super(name); // Chama o construtor da classe Animal
  }

  // Sobrescrevendo o método move
  move(distanceInMeters: number = 5) {
    console.log("Correndo...");
    super.move(distanceInMeters);
  }
}

const myDog = new Dog("Rex");
myDog.move(10);
```

**Exemplo 2: Classes Abstratas**
```typescript
abstract class Shape {
  // Método abstrato: sem implementação, deve ser definido na subclasse
  abstract getArea(): number;

  // Método concreto: já tem implementação e é herdado
  printInfo() {
    console.log(`Esta é uma forma com área de ${this.getArea()}`);
  }
}

class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  // Implementação obrigatória do método abstrato
  getArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

// const shape = new Shape(); // Erro: Não se pode criar instância de classe abstrata.
const myCircle = new Circle(10);
myCircle.printInfo();
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma classe base `Vehicle` com uma propriedade `brand` (marca) e um método `startEngine()` que imprime "Motor ligado". Crie uma subclasse `Car` que herda de `Vehicle` e adiciona uma propriedade `model`. O construtor de `Car` deve receber `brand` e `model`.

<details>
<summary>Ver Solução</summary>

---

```typescript
class Vehicle {
  constructor(public brand: string) {}

  startEngine(): void {
    console.log("Motor ligado.");
  }
}

class Car extends Vehicle {
  constructor(brand: string, public model: string) {
    super(brand); // Passa a marca para o construtor da classe pai
  }

  displayInfo(): void {
    console.log(`Carro: ${this.brand} ${this.model}`);
  }
}

// Teste
const myCar = new Car("Volkswagen", "Gol");
myCar.displayInfo(); // "Carro: Volkswagen Gol"
myCar.startEngine(); // "Motor ligado."
```
</details>

**Nível 2: Intermediário**
Crie uma classe abstrata `LoggerBase` com um método abstrato `log(message: string)`. Crie duas classes concretas que herdam de `LoggerBase`: `ConsoleLogger` (que imprime a mensagem no console) e `FileLogger` (que simula a escrita da mensagem em um arquivo, imprimindo "Escrevendo no arquivo: [mensagem]").

<details>
<summary>Ver Solução</summary>

---

```typescript
abstract class LoggerBase {
  abstract log(message: string): void;

  logWithTimestamp(message: string): void {
    const timestamp = new Date().toISOString();
    this.log(`[${timestamp}] ${message}`);
  }
}

class ConsoleLogger extends LoggerBase {
  log(message: string): void {
    console.log(message);
  }
}

class FileLogger extends LoggerBase {
  constructor(private filePath: string) {
    super();
  }

  log(message: string): void {
    // Simulação
    console.log(`Escrevendo em ${this.filePath}: ${message}`);
  }
}

// Teste
const consoleLogger = new ConsoleLogger();
consoleLogger.logWithTimestamp("Esta é uma mensagem de teste.");

const fileLogger = new FileLogger("/var/log/app.log");
fileLogger.logWithTimestamp("Erro crítico no sistema.");
```
</details>

**Nível 3: Avançado**
Recrie a estrutura do seu `GetTableDataService` usando uma classe abstrata. Crie uma classe `BaseService<TResponse, TParams>` com um método abstrato `handle(params: TParams): Promise<TResponse>`. Crie uma classe concreta `FetchUsersService` que herda de `BaseService` e implementa o método `handle` para "buscar" uma lista de usuários.

<details>
<summary>Ver Solução</summary>

---

```typescript
// Definição dos tipos para o serviço concreto
interface User {
  id: number;
  name: string;
}

interface FetchUsersParams {
  page: number;
  limit: number;
}

// A classe abstrata base
abstract class BaseService<TResponse, TParams> {
  abstract handle(params: TParams): Promise<TResponse>;

  // Um método concreto que pode ser compartilhado
  protected logRequest(params: TParams): void {
    console.log("Iniciando requisição com os parâmetros:", params);
  }
}

// A implementação concreta
class FetchUsersService extends BaseService<User[], FetchUsersParams> {
  async handle(params: FetchUsersParams): Promise<User[]> {
    this.logRequest(params);
    console.log(`Buscando usuários... Página: ${params.page}, Limite: ${params.limit}`);

    // Simula uma chamada de API
    const fakeUsers: User[] = [
      { id: 1, name: "Lucas" },
      { id: 2, name: "Ana" },
    ];

    return Promise.resolve(fakeUsers);
  }
}

// Teste
async function runService() {
  const userService = new FetchUsersService();
  const users = await userService.handle({ page: 1, limit: 10 });
  console.log("Usuários recebidos:", users);
}

runService();
```
</details>

### Checklist do Dia
- [ ] Entendi como `extends` e `super()` funcionam.
- [ ] Sei a diferença entre uma classe normal e uma abstrata.
- [ ] Implementei um método abstrato em uma subclasse.
- [ ] Criei uma hierarquia de classes para reutilizar código.

---

## Dia 9: Generics em Classes

### Foco do Dia
Escrever classes flexíveis e reutilizáveis que podem trabalhar com diferentes tipos de dados usando Generics (`<T>`).

### Leitura e Teoria (Aprofundada)
Generics permitem que você crie componentes que funcionam com qualquer tipo, em vez de um tipo específico. Isso aumenta drasticamente a reutilização de código e a segurança de tipo.

- **Parâmetros de Tipo (`<T>`)**: A letra `T` é uma convenção para "Type". Você pode usar qualquer nome (`<TData>`, `<TValue>`, etc.). Ela atua como uma variável para o tipo.
- **Constraints (`extends`)**: Você pode restringir os tipos que podem ser usados com um generic. `class MyClass<T extends SomeType>` significa que `T` deve ser compatível com `SomeType`.
- **Múltiplos Parâmetros de Tipo**: Uma classe pode ter vários parâmetros de tipo, como `class Pair<K, V>` para um par chave-valor.
- **Tipos Padrão**: Você pode fornecer um tipo padrão para um parâmetro genérico: `class MyClass<T = string>`.

### Documentação Essencial
- [Generics (Handbook)](https://www.typescriptlang.org/docs/handbook/2/generics.html)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Uma Caixa Genérica**
```typescript
class Box<T> {
  private content: T;

  constructor(initialContent: T) {
    this.content = initialContent;
  }

  getContent(): T {
    return this.content;
  }
}

const stringBox = new Box<string>("Olá, Generics!");
const numberBox = new Box<number>(123);

console.log(stringBox.getContent().toUpperCase());
console.log(numberBox.getContent().toFixed(2));
```

**Exemplo 2: Generic com Constraints**
```typescript
interface WithLength {
  length: number;
}

// T pode ser qualquer tipo, desde que tenha uma propriedade `length`
class LengthReporter<T extends WithLength> {
  constructor(private value: T) {}

  report() {
    console.log(`O comprimento é ${this.value.length}`);
  }
}

const stringReporter = new LengthReporter("uma string");
const arrayReporter = new LengthReporter([1, 2, 3]);
// const numberReporter = new LengthReporter(123); // Erro: `number` não tem a propriedade `length`.

stringReporter.report();
arrayReporter.report();
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma classe genérica `DataStore<T>` que armazena um array de itens do tipo `T`. Ela deve ter os métodos `add(item: T)` e `getAll(): T[]`.

<details>
<summary>Ver Solução</summary>

---

```typescript
class DataStore<T> {
  private data: T[] = [];

  add(item: T): void {
    this.data.push(item);
  }

  getAll(): T[] {
    return this.data;
  }
}

// Teste com strings
const stringStore = new DataStore<string>();
stringStore.add("TypeScript");
stringStore.add("Generics");
console.log(stringStore.getAll()); // ["TypeScript", "Generics"]

// Teste com números
const numberStore = new DataStore<number>();
numberStore.add(10);
numberStore.add(20);
console.log(numberStore.getAll()); // [10, 20]
```
</details>

**Nível 2: Intermediário**
Crie uma classe genérica `Cache<T>` que armazena um valor do tipo `T` e uma data de expiração. Implemente os métodos `set(value: T, ttl: number)` (ttl em segundos) e `get(): T | null`. O método `get` deve retornar `null` se o cache tiver expirado.

<details>
<summary>Ver Solução</summary>

---

```typescript
class Cache<T> {
  private value: T | null = null;
  private expiresAt: Date | null = null;

  set(value: T, ttlInSeconds: number): void {
    this.value = value;
    const now = new Date();
    this.expiresAt = new Date(now.getTime() + ttlInSeconds * 1000);
    console.log(`Valor armazenado no cache. Expira em: ${this.expiresAt.toLocaleTimeString()}`);
  }

  get(): T | null {
    if (this.expiresAt && this.expiresAt > new Date() && this.value) {
      console.log("Valor retornado do cache.");
      return this.value;
    }
    console.log("Cache expirado ou vazio.");
    this.value = null;
    this.expiresAt = null;
    return null;
  }
}

// Teste
async function testCache() {
  const userCache = new Cache<{ name: string }>();
  userCache.set({ name: "Lucas" }, 3); // Expira em 3 segundos

  console.log(userCache.get()); // Retorna o objeto

  await new Promise(resolve => setTimeout(resolve, 4000)); // Espera 4 segundos

  console.log(userCache.get()); // Retorna null
}

testCache();
```
</details>

**Nível 3: Avançado**
Crie uma classe `DataService<T extends { id: K }, K extends string | number>`. Esta classe deve gerenciar uma coleção de entidades `T`. Implemente os métodos `add(item: T)` e `findById(id: K): T | undefined`. O uso de múltiplos generics (`T` e `K`) garante que o tipo do `id` seja consistente.

<details>
<summary>Ver Solução</summary>

---

```typescript
// A constraint genérica
interface BaseEntity<K extends string | number> {
  id: K;
}

class DataService<T extends BaseEntity<K>, K extends string | number> {
  private items: Record<K, T> = {} as Record<K, T>;

  add(item: T): void {
    this.items[item.id] = item;
  }

  findById(id: K): T | undefined {
    return this.items[id];
  }

  getAll(): T[] {
    return Object.values(this.items);
  }
}

// Teste
interface Product {
  id: number;
  name: string;
  price: number;
}

// O TS infere que K é `number` a partir de Product['id']
const productService = new DataService<Product, number>();
productService.add({ id: 101, name: "Laptop", price: 5000 });
productService.add({ id: 102, name: "Mouse", price: 150 });

console.log(productService.findById(101));
console.log(productService.findById(999));
console.log(productService.getAll());
```
</details>

### Checklist do Dia
- [ ] Criei uma classe genérica simples.
- [ ] Usei constraints (`extends`) para limitar os tipos de um generic.
- [ ] Entendi como usar múltiplos parâmetros de tipo.
- [ ] Apliquei generics para criar uma classe de serviço reutilizável.

---

## Dia 10: Contexto `this` - Parte 2 Avançada

### Foco do Dia
Explorar técnicas avançadas para controlar o `this`, incluindo `this parameters` para adicionar tipagem explícita ao `this` em funções e `ThisType<T>` para fornecer contexto a objetos literais.

### Leitura e Teoria (Aprofundada)
- **`this` Parameters**: TypeScript permite que você declare o tipo de `this` como o primeiro parâmetro de uma função. Este parâmetro é falso (não existe no JavaScript compilado), mas é usado pelo TypeScript para garantir que a função seja chamada com o contexto correto.
- **`ThisType<T>`**: Um tipo utilitário que não retorna um novo tipo, mas sim modifica o contexto de `this` dentro de um objeto literal. É muito útil para criar APIs onde você define métodos em um objeto, mas quer que `this` se refira a um tipo maior e mais complexo.

### Documentação Essencial
- [This Parameters (Handbook)](https://www.typescriptlang.org/docs/handbook/2/functions.html#this-parameters)
- [ThisType<T> (Handbook)](https://www.typescriptlang.org/docs/handbook/utility-types.html#thistypet)

### Prática Guiada (Passo a Passo)

**Exemplo 1: `this` Parameter para Segurança**
```typescript
// A função espera que `this` seja um objeto com uma propriedade `name`
function sayHello(this: { name: string }) {
  console.log(`Hello, ${this.name}!`);
}

const person = { name: "Lucas", sayHello };
const anotherPerson = { name: "Ana", sayHello };

person.sayHello(); // OK
anotherPerson.sayHello(); // OK

// sayHello(); // Erro: O `this` da função não é do tipo `{ name: string }`.
```

**Exemplo 2: `ThisType<T>` para Objetos de Configuração**
```typescript
interface ComponentOptions<T> {
  data: () => T;
  methods: Record<string, (this: T, ...args: any[]) => any>;
}

// O tipo `T` em `ThisType<T>` define o tipo de `this` nos métodos
function createComponent<T>(options: ComponentOptions<T> & { methods: ThisType<T> }): void {
  // Lógica de criação do componente...
  console.log("Component created.");
}

createComponent({
  data: () => ({ count: 0, message: "Hello" }),
  methods: {
    increment() {
      // Graças a `ThisType`, o TS sabe que `this` tem `count` e `message`
      this.count++;
    },
    logMessage() {
      console.log(this.message);
    }
  }
});
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma interface `User` com `name: string`. Crie uma função `printUserName` que não recebe argumentos, mas espera que o `this` seja do tipo `User`. Demonstre seu uso correto e incorreto.

<details>
<summary>Ver Solução</summary>

---

```typescript
interface User {
  name: string;
}

function printUserName(this: User): void {
  console.log(`User name: ${this.name}`);
}

// Teste
const user1: User = { name: "Alice" };
const user2: User = { name: "Bob" };

// Para chamar a função, precisamos fornecer o contexto `this`
printUserName.call(user1); // "User name: Alice"
printUserName.apply(user2); // "User name: Bob"

// const standaloneCall = printUserName; // Erro ao tentar chamar standaloneCall()
```
</details>

**Nível 2: Intermediário**
Crie uma classe `Configuration<T>` que armazena um objeto de configuração. Crie um método `update(updater: (this: T, currentConfig: T) => T)`. A função `updater` deve receber a configuração atual, mas seu `this` deve ser tipado como a própria configuração `T`, permitindo acesso direto às propriedades.

<details>
<summary>Ver Solução</summary>

---

```typescript
class Configuration<T> {
  constructor(private config: T) {}

  public update(updater: (this: T, currentConfig: T) => T): void {
    const newConfig = updater.call(this.config, this.config);
    this.config = newConfig;
  }

  public getConfig(): T {
    return this.config;
  }
}

// Teste
interface AppConfig {
  apiUrl: string;
  timeout: number;
}

const myConfig = new Configuration<AppConfig>({ 
  apiUrl: "/api/v1", 
  timeout: 5000 
});

// A função updater pode usar `this` para acessar as propriedades de AppConfig
myConfig.update(function(current) {
  return {
    ...current,
    timeout: this.timeout + 1000 // `this` é do tipo AppConfig
  };
});

console.log(myConfig.getConfig()); // { apiUrl: '/api/v1', timeout: 6000 }
```
</details>

**Nível 3: Avançado**
Implemente a classe `FormBuilder<T>` do plano de estudos. Ela deve ter um método `field<K extends keyof T>(name: K, value: T[K]): this` que adiciona um campo ao formulário. O método `field` deve retornar `this` para encadeamento. Crie um método `build(): T` que retorna o objeto de formulário completo.

<details>
<summary>Ver Solução</summary>

---

```typescript
class FormBuilder<T extends object> {
  private formData: Partial<T> = {};

  // O tipo de retorno `this` permite o encadeamento
  public field<K extends keyof T>(name: K, value: T[K]): this {
    this.formData[name] = value;
    return this;
  }

  // O type guard `this is { formData: T }` ajuda o TS a saber que o form está completo
  private isComplete(): this is { formData: T } {
    // Em um cenário real, verificaríamos se todas as chaves de T existem em formData
    return true; // Simplificação para o exercício
  }

  public build(): T | Error {
    if (this.isComplete()) {
      return this.formData; // Graças ao type guard, o TS sabe que formData é T, não Partial<T>
    }
    return new Error("Formulário incompleto.");
  }
}

// Teste
interface UserForm {
  name: string;
  email: string;
  age: number;
}

const userFormBuilder = new FormBuilder<UserForm>();

const newUser = userFormBuilder
  .field("name", "Lucas")
  .field("email", "lucas@ts.com")
  .field("age", 30)
  .build();

console.log(newUser);
```
</details>

### Checklist do Dia
- [ ] Entendi o propósito de um `this` parameter.
- [ ] Usei um `this` parameter para adicionar segurança a uma função.
- [ ] Entendi como `ThisType<T>` funciona em objetos literais.
- [ ] Criei uma classe builder usando `this` para encadeamento.

---

## Dia 11: Decorators

### Foco do Dia
Introduzir `decorators`, uma proposta do ECMAScript para adicionar anotações e modificar classes e seus membros em tempo de design.

### Leitura e Teoria (Aprofundada)
Decorators são funções especiais que podem ser anexadas a classes, métodos, propriedades ou parâmetros. Eles são executados durante a definição da classe, não durante a instanciação.

**Para usar decorators, você precisa habilitar a opção `experimentalDecorators` no seu `tsconfig.json`.**

- **Tipos de Decorators**: Class, Method, Accessor, Property, Parameter.
- **Fábrica de Decorators (Decorator Factory)**: Uma função que retorna a expressão do decorator. Isso permite que você configure o decorator, como `@log("INFO")`.
- **Composição**: Múltiplos decorators podem ser aplicados a uma declaração.
- **`reflect-metadata`**: Uma biblioteca usada para adicionar metadados a classes e propriedades, que podem ser lidos posteriormente pelos decorators.

### Documentação Essencial
- [Decorators (Handbook)](https://www.typescriptlang.org/docs/handbook/decorators.html) (Nota: esta é uma feature experimental)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Decorator de Método Simples**
```typescript
// O decorator recebe o alvo (a classe), a chave (nome do método) e o descritor da propriedade
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function(...args: any[]) {
    console.log(`Chamando o método ${propertyKey} com os argumentos:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`O método ${propertyKey} retornou:`, result);
    return result;
  }
}

class Calculator {
  @log
  add(a: number, b: number): number {
    return a + b;
  }
}

new Calculator().add(2, 3);
```

**Exemplo 2: Decorator Factory**
```typescript
function Enumerable(isEnumerable: boolean) {
  return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    descriptor.enumerable = isEnumerable;
  }
}

class Person {
  constructor(private name: string) {}

  @Enumerable(true)
  getName() { return this.name; }

  @Enumerable(false)
  getAge() { return 30; }
}

// O método getName aparecerá no loop, mas getAge não.
for (const key in new Person("Lucas")) {
  console.log(key);
}
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie um decorator de método chamado `@deprecated`. Quando um método decorado com ele for chamado, ele deve imprimir um aviso no console: `Aviso: O método [nome do método] está obsoleto e será removido em futuras versões.`

<details>
<summary>Ver Solução</summary>

---

```typescript
function deprecated(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function(...args: any[]) {
    console.warn(`Aviso: O método ${propertyKey} está obsoleto e será removido em futuras versões.`);
    return originalMethod.apply(this, args);
  }
}

class OldApiService {
  @deprecated
  findUsers() {
    console.log("Buscando usuários...");
    return [{ name: "Lucas" }];
  }
}

// Teste
new OldApiService().findUsers();
```
</details>

**Nível 2: Intermediário**
Crie um decorator de propriedade `@min(minValue: number)`. Este decorator deve garantir que, sempre que a propriedade for alterada, seu novo valor não seja menor que `minValue`. Se for, um erro deve ser lançado.

<details>
<summary>Ver Solução</summary>

---

```typescript
function min(minValue: number) {
  return function(target: any, propertyKey: string) {
    let value = target[propertyKey];

    const getter = () => value;
    const setter = (newValue: number) => {
      if (newValue < minValue) {
        throw new Error(`O valor de ${propertyKey} não pode ser menor que ${minValue}.`);
      }
      value = newValue;
    };

    Object.defineProperty(target, propertyKey, {
      get: getter,
      set: setter,
      enumerable: true,
      configurable: true,
    });
  }
}

class Product {
  @min(0)
  price: number;

  @min(0)
  stock: number;

  constructor(price: number, stock: number) {
    this.price = price;
    this.stock = stock;
  }
}

// Teste
const product = new Product(50, 100);
console.log(product.price); // 50
product.price = 75;
console.log(product.price); // 75

try {
  product.stock = -10;
} catch (e: any) {
  console.error(e.message);
}
```
</details>

**Nível 3: Avançado**
Crie um decorator de método `@cache`. Ele deve armazenar o resultado da primeira chamada do método. Nas chamadas subsequentes com os mesmos argumentos, ele deve retornar o resultado do cache em vez de executar o método novamente. Use `JSON.stringify` para criar uma chave de cache a partir dos argumentos.

<details>
<summary>Ver Solução</summary>

---

```typescript
function cache(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  const cache = new Map<string, any>();

  descriptor.value = function(...args: any[]) {
    const cacheKey = JSON.stringify(args);

    if (cache.has(cacheKey)) {
      console.log(`Retornando do cache para a chave: ${cacheKey}`);
      return cache.get(cacheKey);
    }

    console.log(`Executando o método pela primeira vez para a chave: ${cacheKey}`);
    const result = originalMethod.apply(this, args);
    cache.set(cacheKey, result);
    return result;
  }
}

class MathOperations {
  @cache
  heavyCalculation(a: number, b: number): number {
    // Simula uma operação pesada
    const start = Date.now();
    while (Date.now() - start < 1000) {}
    return a + b;
  }
}

// Teste
const math = new MathOperations();
console.log(math.heavyCalculation(2, 3)); // Executa, demora 1s, retorna 5
console.log(math.heavyCalculation(5, 10)); // Executa, demora 1s, retorna 15
console.log(math.heavyCalculation(2, 3)); // Retorna do cache, instantâneo, retorna 5
```
</details>

### Checklist do Dia
- [ ] Habilitei `experimentalDecorators` no `tsconfig.json`.
- [ ] Entendi a diferença entre um decorator e uma decorator factory.
- [ ] Criei um decorator de método simples.
- [ ] Criei um decorator de propriedade que modifica seu comportamento.

---

## Dia 12: Eliminando `any` - Parte 2

### Foco do Dia
Introduzir tipos avançados para aumentar a segurança: `never` para código inalcançável e `branded types` para criar tipos nominais que o TypeScript não suporta nativamente.

### Leitura e Teoria (Aprofundada)
- **`never`**: Representa o tipo de valores que nunca ocorrem. É usado em dois cenários principais:
  1.  Em funções que nunca retornam (ex: lançam uma exceção ou entram em um loop infinito).
  2.  Para fazer **verificação exaustiva (exhaustive checking)** em `switch` ou `if/else`, garantindo que todos os casos de uma união foram tratados.
- **Branded Types (ou Opaque Types)**: Uma técnica para criar tipos que são estruturalmente idênticos (ex: ambos são `string`), mas nominalmente diferentes para o TypeScript. Isso evita que você passe um `UserID` para uma função que espera um `ProductID`. É um padrão, não uma feature nativa.

### Documentação Essencial
- [The `never` Type (Handbook)](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)
- [Exhaustiveness checking (Handbook)](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking)

### Prática Guiada (Passo a Passo)

**Exemplo 1: `never` para Verificação Exaustiva**
```typescript
type Shape = { kind: "circle"; radius: number } | { kind: "square"; sideLength: number };

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      // Se adicionarmos um novo tipo a Shape (ex: triangle), o TS dará um erro aqui,
      // pois o novo tipo não pode ser atribuído a `never`.
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
  }
}
```

**Exemplo 2: Branded Types**
```typescript
// O "brand" que torna o tipo único
type Brand<K, T> = K & { __brand: T };

// Criando tipos nominais a partir de tipos primitivos
type UserId = Brand<string, "UserId">;
type ProductId = Brand<string, "ProductId">;

function getUser(id: UserId) {
  console.log(`Buscando usuário com ID: ${id}`);
}

const userId = "user-123" as UserId;
const productId = "prod-456" as ProductId;

getUser(userId); // OK
// getUser(productId); // Erro! O tipo `ProductId` não é compatível com `UserId`.
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma função `fail(message: string): never` que sempre lança um erro com a mensagem fornecida. Use-a em uma função que processa um `string | number` e chama `fail` se o tipo não for nenhum dos dois.

<details>
<summary>Ver Solução</summary>

---

```typescript
function fail(message: string): never {
  throw new Error(message);
}

function processValue(value: string | number) {
  if (typeof value === 'string') {
    console.log("Processando string:", value.toUpperCase());
  } else if (typeof value === 'number') {
    console.log("Processando número:", value.toFixed(2));
  } else {
    // O TS sabe que este código é inalcançável
    fail("Valor inesperado!");
  }
}

// Teste
processValue("hello");
processValue(123);
```
</details>

**Nível 2: Intermediário**
Crie um branded type `Email` a partir de `string`. Crie uma função `sendEmail(to: Email, subject: string)` que só aceita o tipo `Email`. Crie uma função `createEmail(address: string): Email | null` que valida se a string contém um `@` antes de fazer a asserção de tipo para `Email`.

<details>
<summary>Ver Solução</summary>

---

```typescript
type Brand<K, T> = K & { __brand: T };
type Email = Brand<string, "Email">;

function createEmail(address: string): Email | null {
  if (address.includes("@")) {
    return address as Email;
  }
  return null;
}

function sendEmail(to: Email, subject: string): void {
  console.log(`Enviando email para ${to} com o assunto: "${subject}"`);
}

// Teste
const userEmail = createEmail("lucas@example.com");
const invalidEmail = createEmail("invalid-address");
const plainString = "another@test.com";

if (userEmail) {
  sendEmail(userEmail, "Olá!"); // OK
}

if (invalidEmail === null) {
  console.log("Endereço de email inválido detectado.");
}

// sendEmail(plainString, "Assunto"); // Erro: `string` não é compatível com `Email`.
```
</details>

**Nível 3: Avançado**
Crie um wrapper type-safe para uma biblioteca externa falsa. A biblioteca tem um objeto `untypedLibrary` que é `any`. Crie funções `safeGetNumber(key: string)` e `safeGetString(key: string)` que usam o `untypedLibrary`, verificam o tipo do valor retornado, e o retornam com o tipo correto ou `undefined` se o tipo não corresponder.

<details>
<summary>Ver Solução</summary>

---

```typescript
// A biblioteca externa perigosa
declare const untypedLibrary: any;
const untypedLibrary = {
  appName: "Super App",
  version: 2.1,
  userCount: 1500,
  settings: { theme: "dark" }
};

// O wrapper seguro
class SafeLibraryWrapper {
  private lib: any;

  constructor(library: any) {
    this.lib = library;
  }

  public get(key: string): unknown {
    return this.lib[key];
  }

  public getString(key: string): string | undefined {
    const value = this.get(key);
    if (typeof value === 'string') {
      return value;
    }
    return undefined;
  }

  public getNumber(key: string): number | undefined {
    const value = this.get(key);
    if (typeof value === 'number') {
      return value;
    }
    return undefined;
  }
}

// Teste
const safeLib = new SafeLibraryWrapper(untypedLibrary);

const appName = safeLib.getString("appName");
const version = safeLib.getNumber("version");
const userCount = safeLib.getNumber("userCount");
const settings = safeLib.getString("settings"); // Retorna undefined, pois não é string

console.log(`App: ${appName}, Versão: ${version}, Usuários: ${userCount}`);
console.log(`Settings (string):`, settings);
```
</details>

### Checklist do Dia
- [ ] Entendi o propósito do tipo `never`.
- [ ] Usei `never` para fazer verificação exaustiva.
- [ ] Entendi o padrão de Branded Types e por que ele é útil.
- [ ] Criei e usei um branded type para aumentar a segurança de tipo.

---

## Dia 13: Conditional Types

### Foco do Dia
Entender `Conditional Types`, que permitem que um tipo seja escolhido com base em uma condição, e a palavra-chave `infer` para extrair tipos de dentro de outros tipos.

### Leitura e Teoria (Aprofundada)
Conditional Types têm a forma `T extends U ? X : Y`, que se lê como: "Se `T` for compatível com `U`, então o tipo é `X`, senão o tipo é `Y`".

- **`infer`**: A palavra-chave `infer` pode ser usada dentro da cláusula `extends` para declarar uma nova variável de tipo genérico. Ela "captura" o tipo que está naquela posição para que você possa usá-lo.

  Exemplo com `infer`:
  `type UnpackPromise<T> = T extends Promise<infer U> ? U : T;`
  Aqui, `infer U` captura o tipo que está dentro do `Promise` (ex: `string` em `Promise<string>`) e o retorna. Se `T` não for uma `Promise`, ele retorna `T`.

### Documentação Essencial
- [Conditional Types (Handbook)](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)
- [Inferring with `infer` (Handbook)](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#inferring-with-infer)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Tipo Condicional Simples**
```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```

**Exemplo 2: `infer` para Obter o Tipo de Retorno de uma Função**
```typescript
type GetReturnType<T> = T extends (...args: any[]) => infer R ? R : T;

type Fn = () => number;
type Num = GetReturnType<Fn>; // number

type Str = GetReturnType<string>; // string (cai no caso `Y`)
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie um tipo utilitário `ElementTypeOf<T>` que obtém o tipo dos elementos de um array. Se `T` não for um array, ele deve retornar `never`. Ex: `ElementTypeOf<string[]>` deve ser `string`.

<details>
<summary>Ver Solução</summary>

---

```typescript
type ElementTypeOf<T> = T extends (infer E)[] ? E : never;

// Teste
type A = ElementTypeOf<string[]>; // string
type B = ElementTypeOf<number[]>; // number
type C = ElementTypeOf<{ name: string }[]>; // { name: string }
type D = ElementTypeOf<string>; // never
```
</details>

**Nível 2: Intermediário**
Implemente seu próprio `NonNullable<T>`. Este tipo utilitário deve remover `null` e `undefined` de um tipo `T`.

<details>
<summary>Ver Solução</summary>

---

```typescript
type MyNonNullable<T> = T extends null | undefined ? never : T;

// Teste
type A = MyNonNullable<string | null>; // string
type B = MyNonNullable<string | number | undefined>; // string | number
type C = MyNonNullable<null | undefined>; // never
```
</details>

**Nível 3: Avançado**
Implemente o tipo `DeepPartial<T>` do plano de estudos. Ele deve tornar todas as propriedades de um objeto e de seus sub-objetos aninhados opcionais. Dica: você precisará de recursão e mapped types.

<details>
<summary>Ver Solução</summary>

---

```typescript
type DeepPartial<T> = T extends object ? {
  // Para cada propriedade P no objeto T
  [P in keyof T]?: DeepPartial<T[P]>; // Torna a propriedade opcional e aplica DeepPartial recursivamente
} : T; // Se T não for um objeto, retorna o próprio tipo

// Teste
interface UserProfile {
  id: number;
  details: {
    name: string;
    address: {
      street: string;
      city: string;
    }
  }
}

type PartialUserProfile = DeepPartial<UserProfile>;

const partialProfile: PartialUserProfile = {
  id: 1,
  details: {
    address: {
      city: "São Paulo"
    }
  }
};
```
</details>

### Checklist do Dia
- [ ] Entendi a sintaxe `T extends U ? X : Y`.
- [ ] Usei `infer` para extrair um tipo de dentro de outro.
- [ ] Criei um tipo utilitário condicional simples.
- [ ] Implementei um tipo utilitário recursivo (`DeepPartial`).

---

## Dia 14: Projeto Mini #2 - Sistema de Autenticação

### Foco do Dia
Consolidar os conceitos da semana (Herança, Generics, Decorators, Conditional Types) para construir um mini-sistema de autenticação type-safe.

### Leitura e Teoria (Revisão)
- **Abstract Classes**: Para definir um contrato para provedores de autenticação.
- **Generics**: Para lidar com diferentes tipos de dados de usuário (ex: `User`, `Admin`).
- **Decorators**: Para adicionar verificação de permissões de forma declarativa.
- **Conditional Types**: Para criar tipos de permissão dinâmicos.

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma classe abstrata `AuthProvider` com um método abstrato `authenticate(credentials: any): Promise<boolean>`. Crie duas classes concretas: `EmailProvider` e `GoogleProvider`, que herdam de `AuthProvider` e implementam o método `authenticate` (pode apenas simular a lógica e retornar `true`).

<details>
<summary>Ver Solução</summary>

---

```typescript
abstract class AuthProvider {
  abstract authenticate(credentials: any): Promise<boolean>;
}

class EmailProvider extends AuthProvider {
  async authenticate(credentials: { email: string, pass: string }): Promise<boolean> {
    console.log(`Autenticando com email: ${credentials.email}`);
    return true; // Simulação
  }
}

class GoogleProvider extends AuthProvider {
  async authenticate(credentials: { token: string }): Promise<boolean> {
    console.log(`Autenticando com token do Google: ${credentials.token.substring(0, 10)}...`);
    return true; // Simulação
  }
}

// Teste
const emailAuth = new EmailProvider();
emailAuth.authenticate({ email: "test@test.com", pass: "123" });

const googleAuth = new GoogleProvider();
googleAuth.authenticate({ token: "abc123xyz" });
```
</details>

**Nível 2: Intermediário**
Melhore o sistema do Nível 1. Torne a classe `AuthProvider` genérica: `AuthProvider<TUser, TCreds>`. O método `authenticate` deve agora retornar `Promise<TUser | null>`. Adapte as classes `EmailProvider` e `GoogleProvider` para usar tipos específicos de usuário e credenciais.

<details>
<summary>Ver Solução</summary>

---

```typescript
interface BaseUser { id: number; name: string; }

abstract class AuthProvider<TUser extends BaseUser, TCreds> {
  abstract authenticate(credentials: TCreds): Promise<TUser | null>;
}

// Tipos para Email
interface EmailUser extends BaseUser { email: string; }
interface EmailCreds { email: string; pass: string; }

class EmailProvider extends AuthProvider<EmailUser, EmailCreds> {
  async authenticate(credentials: EmailCreds): Promise<EmailUser | null> {
    console.log(`Autenticando com email: ${credentials.email}`);
    if (credentials.pass === "123") {
      return { id: 1, name: "Usuário de Email", email: credentials.email };
    }
    return null;
  }
}

// Tipos para Google
interface GoogleUser extends BaseUser { googleId: string; }
interface GoogleCreds { token: string; }

class GoogleProvider extends AuthProvider<GoogleUser, GoogleCreds> {
  async authenticate(credentials: GoogleCreds): Promise<GoogleUser | null> {
    console.log(`Autenticando com token do Google...`);
    return { id: 2, name: "Usuário do Google", googleId: "g-123" };
  }
}

// Teste
async function testAuth() {
  const emailAuth = new EmailProvider();
  const user1 = await emailAuth.authenticate({ email: "test@test.com", pass: "123" });
  console.log("Usuário 1:", user1);

  const googleAuth = new GoogleProvider();
  const user2 = await googleAuth.authenticate({ token: "abc123xyz" });
  console.log("Usuário 2:", user2);
}

testAuth();
```
</details>

**Nível 3: Avançado**
Crie um decorator de método `@permission(requiredRole: Role)`. Crie um `enum Role { USER, ADMIN }`. Crie uma classe `ProtectedService` com um usuário logado (que tem uma propriedade `role`). Adicione um método `sensitiveData()` decorado com `@permission('ADMIN')`. O decorator deve verificar se o `role` do usuário na instância do serviço corresponde ao `requiredRole` antes de executar o método. Se não, deve lançar um erro.

<details>
<summary>Ver Solução</summary>

---

```typescript
enum Role { USER, ADMIN }

interface UserSession {
  name: string;
  role: Role;
}

// Decorator Factory
function permission(requiredRole: Role) {
  return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function(...args: any[]) {
      // `this` aqui se refere à instância de ProtectedService
      const user = (this as any).currentUser as UserSession;

      if (user && user.role === requiredRole) {
        console.log(`Permissão concedida para ${user.name} (${user.role}).`);
        return originalMethod.apply(this, args);
      } else {
        throw new Error("Acesso negado: permissão insuficiente.");
      }
    }
  }
}

class ProtectedService {
  currentUser: UserSession;

  constructor(user: UserSession) {
    this.currentUser = user;
  }

  @permission(Role.ADMIN)
  deleteEverything(): void {
    console.log("Todos os dados foram deletados com sucesso!");
  }

  @permission(Role.USER)
  viewDashboard(): void {
    console.log("Bem-vindo ao seu dashboard!");
  }
}

// Teste
const adminService = new ProtectedService({ name: "Admin", role: Role.ADMIN });
const userService = new ProtectedService({ name: "User", role: Role.USER });

adminService.deleteEverything(); // OK
userService.viewDashboard(); // OK

try {
  userService.deleteEverything(); // Lança erro
} catch (e: any) {
  console.error(e.message);
}
```
</details>

### Checklist do Dia
- [ ] Usei uma classe abstrata para definir um contrato.
- [ ] Apliquei generics para tornar o sistema de autenticação flexível.
- [ ] Criei um decorator para lidar com permissões de forma declarativa.
- [ ] Combinei múltiplos conceitos da semana em um único projeto.
