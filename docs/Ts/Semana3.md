# Semana 3: Patterns Arquiteturais e Function Types  

## Visão Geral da Semana
Na terceira semana, mudamos o foco para padrões de código e arquitetura de software em TypeScript. Começaremos com técnicas avançadas de tipagem de funções, como `function overloading`, e mergulharemos fundo nos `mapped types` para transformações de tipos complexas. Em seguida, aplicaremos esses conceitos para construir uma camada de serviço robusta, implementar um sistema de tratamento de erros type-safe (Result Pattern) e dominar padrões avançados com `Record`. A semana culmina na construção de um cliente HTTP type-safe, consolidando tudo o que aprendemos.

---

## Dia 15: Function Overloading

### Foco do Dia
Definir múltiplas assinaturas de tipo para uma única função, permitindo que ela se comporte de maneira diferente e retorne tipos diferentes com base nos argumentos fornecidos.

### Leitura e Teoria (Aprofundada)
Function Overloading em TypeScript consiste em duas partes:
1.  **Assinaturas de Sobrecarga (Overload Signatures)**: Uma ou mais declarações do tipo da função, sem corpo. Elas definem as maneiras públicas como a função pode ser chamada.
2.  **Assinatura de Implementação (Implementation Signature)**: Uma única declaração de função com um corpo. Sua assinatura de tipo deve ser geral o suficiente para ser compatível com todas as assinaturas de sobrecarga. O corpo da função geralmente precisa verificar os tipos dos argumentos para executar a lógica correta.

O TypeScript só verifica a compatibilidade com as assinaturas de sobrecarga ao chamar a função, não com a assinatura de implementação.

### Documentação Essencial
- [Function Overloads (Handbook)](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Sobrecarga Simples**
```typescript
// Assinatura 1: recebe número, retorna número
function reverse(x: number): number;
// Assinatura 2: recebe string, retorna string
function reverse(x: string): string;
// Assinatura de Implementação
function reverse(x: number | string): number | string {
  if (typeof x === 'string') {
    return x.split('').reverse().join('');
  }
  return Number(x.toString().split('').reverse().join(''));
}

const reversedString = reverse("hello"); // O TS sabe que o tipo é string
const reversedNumber = reverse(12345); // O TS sabe que o tipo é number
```

**Exemplo 2: Sobrecarga com Número de Argumentos Diferente**
```typescript
// Assinatura 1
function makeDate(timestamp: number): Date;
// Assinatura 2
function makeDate(year: number, month: number, day: number): Date;
// Assinatura de Implementação
function makeDate(arg1: number, arg2?: number, arg3?: number): Date {
  if (arg2 !== undefined && arg3 !== undefined) {
    return new Date(arg1, arg2, arg3);
  }
  return new Date(arg1);
}

const d1 = makeDate(1234567890);
const d2 = makeDate(2023, 11, 24);
// const d3 = makeDate(2023, 11); // Erro: Nenhuma sobrecarga corresponde a esta chamada.
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma função `doubleMe` que tem duas sobrecargas: se receber um `number`, retorna um `number` (o dobro). Se receber um `string`, retorna uma `string` (a string concatenada com ela mesma).

<details>
<summary>Ver Solução</summary>

```typescript
function doubleMe(x: number): number;
function doubleMe(x: string): string;
function doubleMe(x: any): any {
  if (typeof x === 'number') {
    return x * 2;
  }
  if (typeof x === 'string') {
    return x + x;
  }
}

// Teste
const numResult = doubleMe(10); // 20 (tipo number)
const strResult = doubleMe("hi"); // "hihi" (tipo string)

console.log(typeof numResult, numResult);
console.log(typeof strResult, strResult);
```
</details>

**Nível 2: Intermediário**
Crie uma função `createElement`. Se ela receber um único argumento (`'div'`), ela deve retornar um `HTMLDivElement`. Se receber `'input'`, deve retornar um `HTMLInputElement`. Use sobrecargas para tipar o retorno corretamente.

<details>
<summary>Ver Solução</summary>

```typescript
// Tipos de retorno simulados, já que não estamos no DOM
interface HTMLDivElement { type: 'div'; }
interface HTMLInputElement { type: 'input'; }

// Sobrecargas
function createElement(tag: 'div'): HTMLDivElement;
function createElement(tag: 'input'): HTMLInputElement;
// Implementação
function createElement(tag: 'div' | 'input'): HTMLDivElement | HTMLInputElement {
  if (tag === 'div') {
    return { type: 'div' };
  }
  // Se não for 'div', deve ser 'input'
  return { type: 'input' };
}

// Teste
const div = createElement('div'); // O tipo de `div` é HTMLDivElement
const input = createElement('input'); // O tipo de `input` é HTMLInputElement

console.log(div.type);
console.log(input.type);
```
</details>

**Nível 3: Avançado**
Implemente a função `getData` do plano de estudos. Ela deve ter duas sobrecargas:
1.  `getData(id: string): Promise<User>`: Busca um único usuário.
2.  `getData(filter: Filter): Promise<User[]>`: Busca uma lista de usuários.
Simule a lógica de busca e os tipos `User` e `Filter`.

<details>
<summary>Ver Solução</summary>

```typescript
interface User { id: string; name: string; }
interface Filter { status: 'active' | 'inactive'; }

// Sobrecarga 1
function getData(id: string): Promise<User>;
// Sobrecarga 2
function getData(filter: Filter): Promise<User[]>;
// Implementação
async function getData(arg: string | Filter): Promise<User | User[]> {
  if (typeof arg === 'string') {
    console.log(`Buscando usuário com id: ${arg}`);
    return { id: arg, name: 'Lucas' }; // Simulação
  }
  
  console.log(`Buscando usuários com filtro:`, arg);
  return [
    { id: '1', name: 'Ana' },
    { id: '2', name: 'Beto' },
  ]; // Simulação
}

// Teste
async function testGetData() {
  const singleUser = await getData('user-1');
  console.log('Usuário único:', singleUser.name);

  const userList = await getData({ status: 'active' });
  console.log('Lista de usuários:', userList.length);
}

testGetData();
```
</details>

### Checklist do Dia
- [ ] Entendi a diferença entre assinatura de sobrecarga e de implementação.
- [ ] Criei uma função com múltiplas assinaturas de tipo.
- [ ] Usei sobrecargas para retornar tipos diferentes com base nos argumentos.
- [ ] Implementei uma função assíncrona com sobrecargas.

---

## Dia 16: Mapped Types Avançados

### Foco do Dia
Dominar `Mapped Types` para criar novos tipos transformando as propriedades de tipos existentes, incluindo `key remapping` com `as` para renomear chaves.

### Leitura e Teoria (Aprofundada)
Mapped Types iteram sobre as chaves de um tipo para criar um novo tipo. A sintaxe é `[P in keyof T]: ...`.

- **Modificadores**: Você pode adicionar ou remover modificadores como `readonly` e `?` (opcional) durante o mapeamento. ` -readonly` remove, `+readonly` (ou apenas `readonly`) adiciona.
- **Key Remapping com `as`**: Permite transformar os nomes das chaves. `[P in keyof T as NewKeyType]`.
- **Template Literal Types em Mapped Types**: A combinação mais poderosa. Permite criar novas chaves baseadas em um padrão, como adicionar `get` ou `set` como prefixo.

### Documentação Essencial
- [Mapped Types (Handbook)](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)
- [Key Remapping via `as` (Handbook)](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html#key-remapping-via-as)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Criando um tipo de Getters**
```typescript
interface Person { name: string; age: number; }

// Para cada chave P em Person, cria uma chave `getName`, `getAge`
// que é uma função retornando o tipo da propriedade original.
type Getters<T> = {
  [P in keyof T as `get${Capitalize<string & P>}`]: () => T[P]
};

type PersonGetters = Getters<Person>;
// Equivale a: { getName: () => string; getAge: () => number; }
```

**Exemplo 2: Removendo `readonly`**
```typescript
interface LockedConfig { readonly apiUrl: string; readonly apiKey: string; }

type Mutable<T> = {
  -readonly [P in keyof T]: T[P]
};

type UnlockedConfig = Mutable<LockedConfig>;
// Equivale a: { apiUrl: string; apiKey: string; }
```

**Exemplo 3: Filtrando Chaves**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  passwordHash: string;
}

// Filtra chaves cujo valor não é do tipo `string`
type StringPropertiesOnly<T> = {
  [K in keyof T as T[K] extends string ? K : never]: T[K]
};

type UserStringProps = StringPropertiesOnly<User>;
// Equivale a: { name: string; email: string; passwordHash: string; }
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie um Mapped Type `ObjectWithSetters<T>` que pega um objeto `T` e cria um novo tipo onde cada propriedade `prop` se torna um método `setProp(value: T[prop]): void`.

<details>
<summary>Ver Solução</summary>

```typescript
type ObjectWithSetters<T> = {
  [P in keyof T as `set${Capitalize<string & P>}`]: (value: T[P]) => void;
};

// Teste
interface Settings {
  theme: string;
  fontSize: number;
}

type SettingsSetters = ObjectWithSetters<Settings>;
// Esperado: { setTheme: (value: string) => void; setFontSize: (value: number) => void; }

const setters: SettingsSetters = {
  setTheme: (s) => console.log(s),
  setFontSize: (n) => console.log(n),
};
```
</details>

**Nível 2: Intermediário**
Crie um Mapped Type `FilterByType<T, U>` que pega um tipo `T` e remove todas as propriedades que não são do tipo `U`.

<details>
<summary>Ver Solução</summary>

```typescript
type FilterByType<T, U> = {
  [P in keyof T as T[P] extends U ? P : never]: T[P];
};

// Teste
interface MixedBag {
  name: string;
  count: number;
  active: boolean;
  value: number;
}

type NumberOnlyBag = FilterByType<MixedBag, number>;
// Esperado: { count: number; value: number; }

const numbers: NumberOnlyBag = { count: 5, value: 10 };
```
</details>

**Nível 3: Avançado**
Implemente o `FormConfig<T>` do plano de estudos. Ele deve pegar um tipo `T` e gerar um novo tipo onde cada chave `K` é transformada em `${string & K}Config` e o valor é um objeto de configuração `FieldConfig<T[K]>`.

<details>
<summary>Ver Solução</summary>

```typescript
// Tipo auxiliar para a configuração do campo
interface FieldConfig<T> {
  label: string;
  type: T extends string ? 'text' : T extends number ? 'number' : 'checkbox';
  defaultValue: T;
}

// O Mapped Type avançado
type FormConfig<T> = {
  [K in keyof T as `${string & K}Config`]: FieldConfig<T[K]>;
};

// Teste
interface UserForm {
  name: string;
  age: number;
  isAdmin: boolean;
}

type GeneratedFormConfig = FormConfig<UserForm>;
/*
Esperado:
{
  nameConfig: FieldConfig<string>;
  ageConfig: FieldConfig<number>;
  isAdminConfig: FieldConfig<boolean>;
}
*/

const userFormConfig: GeneratedFormConfig = {
  nameConfig: { label: 'Nome', type: 'text', defaultValue: '' },
  ageConfig: { label: 'Idade', type: 'number', defaultValue: 0 },
  isAdminConfig: { label: 'É Admin?', type: 'checkbox', defaultValue: false },
};
```
</details>

### Checklist do Dia
- [ ] Criei um mapped type simples.
- [ ] Usei `as` para renomear chaves (key remapping).
- [ ] Combinei mapped types com template literals.
- [ ] Usei um tipo condicional para filtrar chaves em um mapped type.

---

## Dia 17: Service Layer Architecture

### Foco do Dia
Aplicar os conceitos de classes abstratas e interfaces para projetar uma camada de serviço (Service Layer) desacoplada e testável.

### Leitura e Teoria (Aprofundada)
- **Service Layer**: Uma camada na arquitetura de uma aplicação que encapsula a lógica de negócio. Ela coordena o trabalho entre a camada de apresentação (ex: controllers) e a camada de acesso a dados (ex: repositories).
- **Dependency Injection (DI)**: Um padrão onde as dependências de uma classe (outros objetos que ela precisa para funcionar) são "injetadas" de fora (geralmente no construtor), em vez de serem criadas dentro da própria classe. Isso torna as classes mais desacopladas e fáceis de testar, pois você pode injetar "mocks" (dependências falsas) durante os testes.
- **Interface Segregation Principle**: Um dos princípios SOLID. Diz que é melhor ter muitas interfaces pequenas e específicas do que uma única interface grande e genérica. Em TypeScript, usamos `interface` ou `type` para definir os "contratos" que as classes devem seguir.

### Documentação Essencial
- [Interfaces (Handbook)](https://www.typescriptlang.org/docs/handbook/2/objects.html)

### Prática Guiada (Passo a Passo)

**Exemplo: Injetando um Repositório em um Serviço**
```typescript
// Contrato para o repositório de usuários
interface IUserRepository {
  findById(id: string): Promise<{ id: string, name: string } | null>;
}

// Serviço que depende do repositório
class UserService {
  // A dependência é injetada no construtor
  constructor(private userRepository: IUserRepository) {}

  async getUserName(id: string): Promise<string> {
    const user = await this.userRepository.findById(id);
    if (!user) {
      return "Usuário não encontrado";
    }
    return user.name;
  }
}

// Implementação real do repositório
class UserRepository implements IUserRepository {
  async findById(id: string) {
    return { id, name: "Lucas da Silva" }; // Simulação
  }
}

// Implementação falsa (mock) para testes
class MockUserRepository implements IUserRepository {
  async findById(id: string) {
    if (id === '1') return { id: '1', name: 'Mock User' };
    return null;
  }
}

// Uso em produção
const realService = new UserService(new UserRepository());
// Uso em testes
const testService = new UserService(new MockUserRepository());
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Defina uma interface `INotifier` com um método `notify(message: string): void`. Crie uma classe `OrderService` que recebe um `INotifier` no construtor. Crie um método `placeOrder()` na `OrderService` que, após simular a criação de um pedido, chama `notifier.notify("Pedido realizado com sucesso!")`.

<details>
<summary>Ver Solução</summary>

```typescript
interface INotifier {
  notify(message: string): void;
}

class EmailNotifier implements INotifier {
  notify(message: string): void {
    console.log(`Enviando email: ${message}`);
  }
}

class OrderService {
  constructor(private notifier: INotifier) {}

  placeOrder(): void {
    console.log("Processando pedido...");
    // Lógica do pedido...
    console.log("Pedido processado.");
    this.notifier.notify("Seu pedido foi realizado com sucesso!");
  }
}

// Teste
const emailNotifier = new EmailNotifier();
const orderService = new OrderService(emailNotifier);
orderService.placeOrder();
```
</details>

**Nível 2: Intermediário**
Crie uma interface `IProductRepository` com um método `getProductPrice(productId: number): Promise<number>`. Crie uma classe `PricingService` que depende de `IProductRepository`. O serviço deve ter um método `calculateDiscount(productId: number, discountPercentage: number)` que busca o preço do produto e retorna o valor do desconto.

<details>
<summary>Ver Solução</summary>

```typescript
interface IProductRepository {
  getProductPrice(productId: number): Promise<number>;
}

// Implementação real
class ProductRepository implements IProductRepository {
  async getProductPrice(productId: number): Promise<number> {
    // Simula busca no banco de dados
    const prices: Record<number, number> = { 101: 50, 102: 120 };
    return prices[productId] || 0;
  }
}

class PricingService {
  constructor(private productRepo: IProductRepository) {}

  async calculateDiscount(productId: number, discountPercentage: number): Promise<number> {
    const price = await this.productRepo.getProductPrice(productId);
    if (price === 0) {
      console.log("Produto não encontrado.");
      return 0;
    }
    const discount = price * (discountPercentage / 100);
    return discount;
  }
}

// Teste
async function testPricing() {
  const repo = new ProductRepository();
  const service = new PricingService(repo);
  const discountValue = await service.calculateDiscount(101, 10); // 10% de 50
  console.log(`Valor do desconto: R$${discountValue}`); // 5
}

testPricing();
```
</details>

**Nível 3: Avançado**
Recrie e melhore seu `GetTableDataService`. Defina uma classe abstrata `GetTableDataService<TResponse, TParams>` com um método abstrato `handle(params: TParams): Promise<TResponse>`. Crie uma implementação concreta `UserTableService` que depende de um `IUserRepository` (com um método `find(params: TParams)`) injetado no construtor.

<details>
<summary>Ver Solução</summary>

```typescript
// Contratos e Tipos
interface User { id: number; name: string; status: string; }
interface UserFindParams { page: number; filterByStatus?: string; }
interface IUserRepository {
  find(params: UserFindParams): Promise<User[]>;
}

// Classe Abstrata do Serviço
abstract class GetTableDataService<TResponse, TParams> {
  abstract handle(params: TParams): Promise<TResponse>;
}

// Implementação do Repositório
class UserRepository implements IUserRepository {
  private allUsers: User[] = [
    { id: 1, name: 'Alice', status: 'active' },
    { id: 2, name: 'Bob', status: 'inactive' },
    { id: 3, name: 'Charlie', status: 'active' },
  ];

  async find(params: UserFindParams): Promise<User[]> {
    if (params.filterByStatus) {
      return this.allUsers.filter(u => u.status === params.filterByStatus);
    }
    return this.allUsers;
  }
}

// Implementação Concreta do Serviço
class UserTableService extends GetTableDataService<User[], UserFindParams> {
  constructor(private userRepository: IUserRepository) {
    super();
  }

  handle(params: UserFindParams): Promise<User[]> {
    console.log("Service: buscando dados da tabela de usuários...");
    return this.userRepository.find(params);
  }
}

// Teste
async function testTableService() {
  const repo = new UserRepository();
  const tableService = new UserTableService(repo);
  const activeUsers = await tableService.handle({ page: 1, filterByStatus: 'active' });
  console.log("Usuários ativos:", activeUsers);
}

testTableService();
```
</details>

### Checklist do Dia
- [ ] Entendi o que é uma camada de serviço.
- [ ] Usei injeção de dependência para desacoplar uma classe.
- [ ] Defini um contrato com `interface` para uma dependência.
- [ ] Implementei um serviço que depende de um repositório.

---

## Dia 18: Error Handling Type-Safe

### Foco do Dia
Implementar um sistema de tratamento de erros sem usar `try/catch` em toda parte, utilizando o padrão `Result` (também conhecido como `Either`) com discriminated unions.

### Leitura e Teoria (Aprofundada)
Lançar exceções é útil, mas pode tornar o fluxo de controle difícil de seguir. Uma alternativa comum em programação funcional é fazer com que as funções retornem um tipo que representa tanto o sucesso quanto o fracasso.

- **Result Pattern**: Uma função, em vez de retornar um valor `T` ou lançar um `Error`, retorna um objeto `Result<T, E>`. Este objeto é uma união discriminada de dois tipos:
  - `Success<T>`: Contém o valor de sucesso.
  - `Failure<E>`: Contém o valor do erro.

Isso força quem chama a função a verificar explicitamente se a operação foi bem-sucedida ou não, tornando o código mais previsível e seguro.

### Documentação Essencial
- [Discriminated Unions (Revisão)](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions)

### Prática Guiada (Passo a Passo)

**Exemplo: Definindo e usando o tipo `Result`**
```typescript
// 1. Definir os tipos Success, Failure e Result
type Success<T> = { success: true; value: T };
type Failure<E> = { success: false; error: E };
type Result<T, E> = Success<T> | Failure<E>;

// 2. Criar uma função que retorna um Result
function safeDivide(a: number, b: number): Result<number, string> {
  if (b === 0) {
    return { success: false, error: "Divisão por zero!" };
  }
  return { success: true, value: a / b };
}

// 3. Consumir a função de forma segura
const result = safeDivide(10, 2);
if (result.success) {
  // O TS sabe que `result` é Success<number> aqui
  console.log("Resultado:", result.value);
} else {
  // O TS sabe que `result` é Failure<string> aqui
  console.error("Erro:", result.error);
}
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma função `parseNumber(s: string): Result<number, string>` que tenta converter uma string para um número. Se `isNaN(result)` for verdadeiro, retorne um `Failure`. Caso contrário, retorne um `Success`.

<details>
<summary>Ver Solução</summary>

```typescript
type Success<T> = { success: true; value: T };
type Failure<E> = { success: false; error: E };
type Result<T, E> = Success<T> | Failure<E>;

function parseNumber(s: string): Result<number, string> {
  const num = Number(s);
  if (isNaN(num)) {
    return { success: false, error: `\'${s}\' não é um número válido.` };
  }
  return { success: true, value: num };
}

// Teste
const res1 = parseNumber("123");
if (res1.success) console.log(res1.value);

const res2 = parseNumber("abc");
if (!res2.success) console.error(res2.error);
```
</details>

**Nível 2: Intermediário**
Crie uma classe `Result` com métodos estáticos `ok<T>(value: T)` e `fail<E>(error: E)` para facilitar a criação. Refatore o exercício anterior para usar `Result.ok(...)` e `Result.fail(...)`.

<details>
<summary>Ver Solução</summary>

```typescript
// Tipos auxiliares
class Success<T> { readonly value: T; readonly success = true; constructor(value: T) { this.value = value; } }
class Failure<E> { readonly error: E; readonly success = false; constructor(error: E) { this.error = error; } }

// A classe Result com métodos estáticos
class Result<T, E> {
  static ok<T, E>(value: T): Result<T, E> {
    return new Success(value);
  }

  static fail<T, E>(error: E): Result<T, E> {
    return new Failure(error);
  }
}

function parseNumber(s: string): Success<number> | Failure<string> {
  const num = Number(s);
  if (isNaN(num)) {
    return new Failure(`\'${s}\' não é um número válido.`);
  }
  return new Success(num);
}

// Teste
const res = parseNumber("456");
if (res.success) {
  console.log("Valor parseado:", res.value);
}
```
</details>

**Nível 3: Avançado**
Crie uma função `fetchUserProfile(userId: string): Promise<Result<User, Error>>`. Esta função deve simular uma chamada de API. Se o `userId` for `'1'`, retorne um `Success` com um objeto `User`. Se for qualquer outro valor, retorne um `Failure` com um `new Error("Usuário não encontrado")`.

<details>
<summary>Ver Solução</summary>

```typescript
// Reutilizando os tipos do Nível 1
type Success<T> = { success: true; value: T };
type Failure<E> = { success: false; error: E };
type Result<T, E> = Success<T> | Failure<E>;

interface User { id: string; name: string; }

async function fetchUserProfile(userId: string): Promise<Result<User, Error>> {
  console.log(`Buscando perfil para o usuário ${userId}...`);
  // Simulação de chamada de API
  if (userId === '1') {
    const user: User = { id: '1', name: 'Admin User' };
    return { success: true, value: user };
  }
  return { success: false, error: new Error("Usuário não encontrado") };
}

// Teste
async function testFetch() {
  const result1 = await fetchUserProfile('1');
  if (result1.success) {
    console.log("Bem-vindo,", result1.value.name);
  } else {
    console.error(result1.error.message);
  }

  const result2 = await fetchUserProfile('2');
  if (result2.success) {
    console.log("Bem-vindo,", result2.value.name);
  } else {
    console.error(result2.error.message);
  }
}

testFetch();
```
</details>

### Checklist do Dia
- [ ] Entendi o Result Pattern e suas vantagens.
- [ ] Criei um tipo `Result` usando discriminated unions.
- [ ] Implementei uma função que retorna `Success` ou `Failure`.
- [ ] Consumi uma função que retorna `Result` de forma type-safe.

---

## Dia 19: Advanced Record Patterns

### Foco do Dia
Explorar usos avançados do tipo `Record<K, V>`, combinando-o com `const assertions` para imutabilidade e template literal types para criar dicionários mais seguros e expressivos.

### Leitura e Teoria (Aprofundada)
- **`Record<Keys, Type>`**: Cria um tipo de objeto com um conjunto específico de chaves (`Keys`) e um tipo de valor (`Type`).
- **`const` Assertions (`as const`)**: Quando usado em um objeto literal, diz ao TypeScript para tratar o objeto como profundamente `readonly`. As propriedades se tornam `readonly` e os literais (strings, números) se tornam tipos literais, não tipos gerais (`'myString'` se torna tipo `'myString'`, não `string`).
- **Combinação**: Usar `Record` para definir a estrutura e `as const` para garantir imutabilidade e tipos literais precisos é um padrão poderoso para configurações, dicionários de tradução, etc.

### Documentação Essencial
- [Utility Types (Revisão)](https://www.typescriptlang.org/docs/handbook/utility-types.html)
- [Const Assertions (Handbook)](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)

### Prática Guiada (Passo a Passo)

**Exemplo: Dicionário de Configuração com `as const`**
```typescript
const AppConfig = {
  API_URL: "/api",
  TIMEOUT: 5000,
  THEME: "dark",
} as const;

// AppConfig.API_URL = "/api/v2"; // Erro: Cannot assign to 'API_URL' because it is a read-only property.

// O tipo de THEME é 'dark', não string!
type Theme = typeof AppConfig["THEME"]; // 'dark'
```

**Exemplo: `Record` com Template Literals**
```typescript
type IconName = 'user' | 'cart' | 'home';
type IconPath = `/icons/${IconName}.svg`;

const iconMap: Record<IconName, IconPath> = {
  user: '/icons/user.svg',
  cart: '/icons/cart.svg',
  home: '/icons/home.svg',
};
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie um objeto `HttpStatusCodes` que mapeia nomes de status (`OK`, `NOT_FOUND`, `SERVER_ERROR`) para seus códigos numéricos (200, 404, 500). Use `as const` para garantir que os valores não possam ser alterados.

<details>
<summary>Ver Solução</summary>

```typescript
const HttpStatusCodes = {
  OK: 200,
  NOT_FOUND: 404,
  SERVER_ERROR: 500,
} as const;

// Teste
console.log(HttpStatusCodes.OK); // 200
// HttpStatusCodes.OK = 201; // Erro: Cannot assign to 'OK' because it is a read-only property.

// O tipo de HttpStatusCodes.OK é 200, não number.
type OkStatus = typeof HttpStatusCodes.OK; // 200
```
</details>

**Nível 2: Intermediário**
Crie um tipo `UserAction` (`'create'` | `'edit'` | `'delete'`). Crie um objeto `actionPermissions` que mapeia cada `UserAction` para um nível de permissão (`'admin'` | `'editor'` | `'guest'`). Use `Record` para garantir que todas as ações sejam mapeadas.

<details>
<summary>Ver Solução</summary>

```typescript
type UserAction = 'create' | 'edit' | 'delete';
type PermissionLevel = 'admin' | 'editor' | 'guest';

const actionPermissions: Record<UserAction, PermissionLevel> = {
  create: 'admin',
  edit: 'editor',
  delete: 'admin',
};

// Teste
function checkPermission(action: UserAction) {
  console.log(`Ação '${action}' requer nível '${actionPermissions[action]}'.`);
}

checkPermission('edit');
// const p: Record<UserAction, PermissionLevel> = { create: 'admin' }; // Erro: Faltam 'edit' e 'delete'
```
</details>

**Nível 3: Avançado**
Implemente o `tooltipContent` do plano de estudos. Crie um `enum ColumnType`. Crie um tipo `DictionaryPath` usando template literals. Crie o objeto `tooltipContent` usando `Record` para o mapeamento e `as const` para imutabilidade total.

<details>
<summary>Ver Solução</summary>

```typescript
enum ColumnType {
  YIELD = 'yield',
  PRICE = 'price',
}

type DictionaryPath = `min_fare.table.tooltips.${string}`;

// O tipo garante que todas as chaves de ColumnType existam e que os valores sigam o padrão.
const tooltipContent: Record<ColumnType, DictionaryPath> = {
  [ColumnType.YIELD]: 'min_fare.table.tooltips.yield',
  [ColumnType.PRICE]: 'min_fare.table.tooltips.price',
};

// Adicionando `as const` para imutabilidade e tipos literais precisos
const immutableTooltipContent = {
  [ColumnType.YIELD]: 'min_fare.table.tooltips.yield',
  [ColumnType.PRICE]: 'min_fare.table.tooltips.price',
} as const;

// immutableTooltipContent.yield = '...'; // Erro: readonly

// O tipo do valor é literal, não string
type YieldTooltipPath = typeof immutableTooltipContent[ColumnType.YIELD];
```
</details>

### Checklist do Dia
- [ ] Usei `as const` para criar um objeto imutável.
- [ ] Entendi como `as const` afeta a inferência de tipo.
- [ ] Combinei `Record` com `enum` para criar um dicionário seguro.
- [ ] Combinei `Record`, template literals e `as const`.

---

## Dia 20: Utility Types Avançados

### Foco do Dia
Dominar os utility types que operam sobre funções: `Parameters`, `ReturnType`, `ConstructorParameters`, e `InstanceType`.

### Leitura e Teoria (Aprofundada)
Estes tipos permitem extrair "partes" de tipos de função, o que é extremamente útil para metaprogramação e para manter a consistência de tipos sem repetição.

- **`Parameters<T>`**: Extrai os tipos dos parâmetros de uma função `T` como uma tupla.
- **`ReturnType<T>`**: Extrai o tipo de retorno de uma função `T`.
- **`ConstructorParameters<T>`**: Extrai os tipos dos parâmetros do construtor de uma classe `T` como uma tupla.
- **`InstanceType<T>`**: Extrai o tipo da instância de uma classe `T`.

### Documentação Essencial
- [Advanced Utility Types (Handbook)](https://www.typescriptlang.org/docs/handbook/utility-types.html#parameterst)

### Prática Guiada (Passo a Passo)

**Exemplo 1: `Parameters` e `ReturnType`**
```typescript
function greet(name: string, age: number): string {
  return `Hello ${name}, you are ${age} years old.`;
}

type GreetParams = Parameters<typeof greet>; // [string, number]
type GreetReturn = ReturnType<typeof greet>;   // string
```

**Exemplo 2: `ConstructorParameters` e `InstanceType`**
```typescript
class Person {
  constructor(public name: string, public age: number) {}
}

type PersonConstructorParams = ConstructorParameters<typeof Person>; // [string, number]
type PersonInstance = InstanceType<typeof Person>; // Person
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma função `log(message: string, level: 'info' | 'warn' | 'error')`. Use `Parameters<T>` para criar um tipo `LogParams` que represente os parâmetros da função `log`.

<details>
<summary>Ver Solução</summary>

```typescript
function log(message: string, level: 'info' | 'warn' | 'error'): void {
  console.log(`[${level.toUpperCase()}] ${message}`);
}

type LogParams = Parameters<typeof log>; // [string, 'info' | 'warn' | 'error']

// Teste
const params: LogParams = ["Usuário logado com sucesso", "info"];
log(...params);
```
</details>

**Nível 2: Intermediário**
Crie uma função `fetchData(): Promise<{ data: string[] }>` que simula uma chamada de API. Use `ReturnType<T>` para extrair o tipo de retorno. Em seguida, use `Awaited<T>` (um tipo nativo) para extrair o tipo resolvido da Promise.

<details>
<summary>Ver Solução</summary>

```typescript
async function fetchData(): Promise<{ data: string[] }> {
  return { data: ['a', 'b', 'c'] };
}

// O tipo de retorno da função em si
type FetchDataReturn = ReturnType<typeof fetchData>; // Promise<{ data: string[] }>

// O tipo que a Promise resolve
type FetchedData = Awaited<FetchDataReturn>; // { data: string[] }

// Teste
async function processData() {
  const data: FetchedData = await fetchData();
  console.log(data.data.join(', '));
}

processData();
```
</details>

**Nível 3: Avançado**
Implemente o `ServiceFactory<T>` do plano de estudos. Deve ser um tipo que representa um objeto com um método `create`. Este método `create` deve aceitar os mesmos parâmetros que o construtor da classe `T` e retornar uma instância de `T`.

<details>
<summary>Ver Solução</summary>

```typescript
// O tipo genérico da fábrica
type ServiceFactory<T extends new (...args: any[]) => any> = {
  create(...args: ConstructorParameters<T>): InstanceType<T>;
};

// Classe de exemplo
class ProductService {
  constructor(private apiVersion: string) {
    console.log(`ProductService inicializado com API v${apiVersion}`);
  }

  getProducts() { /* ... */ }
}

// Implementação da fábrica
const ProductServiceFactory: ServiceFactory<typeof ProductService> = {
  create(...args) {
    return new ProductService(...args);
  },
};

// Teste
const productServiceInstance = ProductServiceFactory.create("2.0");
console.log(productServiceInstance instanceof ProductService); // true
```
</details>

### Checklist do Dia
- [ ] Usei `Parameters` para extrair os tipos dos parâmetros de uma função.
- [ ] Usei `ReturnType` para extrair o tipo de retorno.
- [ ] Usei `ConstructorParameters` e `InstanceType` em uma classe.
- [ ] Criei um tipo de fábrica genérico usando utility types de função.

---

## Dia 21: Projeto Mini #3 - HTTP Client Type-Safe

### Foco do Dia
Consolidar os conceitos da semana (Function Overloading, Mapped Types, Service Layer, Result Pattern) para construir um cliente HTTP type-safe.

### Leitura e Teoria (Revisão)
- **Function Overloading**: Para criar métodos como `client.get(...)` que podem ter assinaturas diferentes.
- **Result Pattern**: Para tratar erros de rede (ex: 404, 500) de forma explícita.
- **Generics**: Para tipar a resposta esperada da API (`TResponse`).
- **Record Patterns**: Para configurar headers e parâmetros.

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma classe `HttpClient` com um único método `get<T>(url: string): Promise<T>`. Este método deve simular uma chamada de rede e retornar um objeto `T`. Não se preocupe com erros ainda.

<details>
<summary>Ver Solução</summary>

```typescript
class HttpClient {
  async get<T>(url: string): Promise<T> {
    console.log(`Fazendo GET para: ${url}`);
    // Simulação
    const response = { data: { id: 1, name: "Item Falso" } };
    return response as T;
  }
}

// Teste
interface UserResponse { data: { id: number; name: string; } }

async function testClient() {
  const client = new HttpClient();
  const response = await client.get<UserResponse>("/api/users/1");
  console.log(response.data.name);
}

testClient();
```
</details>

**Nível 2: Intermediário**
Melhore o `HttpClient` do Nível 1 para usar o `Result` pattern. O método `get<T>(url: string)` deve agora retornar `Promise<Result<T, Error>>`. Se a URL contiver a palavra `'fail'`, simule um erro e retorne um `Failure`. Caso contrário, retorne um `Success`.

<details>
<summary>Ver Solução</summary>

```typescript
// Reutilizando o tipo Result
type Success<T> = { success: true; value: T };
type Failure<E> = { success: false; error: E };
type Result<T, E> = Success<T> | Failure<E>;

class HttpClient {
  async get<T>(url: string): Promise<Result<T, Error>> {
    console.log(`Fazendo GET para: ${url}`);

    // Simulação de erro
    if (url.includes('fail')) {
      return { success: false, error: new Error("Erro de Rede 404") };
    }

    // Simulação de sucesso
    const response = { data: { id: 1, name: "Item Falso" } };
    return { success: true, value: response as T };
  }
}

// Teste
interface UserResponse { data: { id: number; name: string; } }

async function testClient() {
  const client = new HttpClient();
  
  const successResult = await client.get<UserResponse>("/api/users/1");
  if (successResult.success) {
    console.log("Sucesso:", successResult.value.data.name);
  }

  const errorResult = await client.get<UserResponse>("/api/fail/users/1");
  if (!errorResult.success) {
    console.error("Erro:", errorResult.error.message);
  }
}

testClient();
```
</details>

**Nível 3: Avançado**
Adicione sobrecarga de função ao `HttpClient`. Crie um método `request` com duas sobrecargas:
1.  `request<T>(method: 'GET', url: string): Promise<Result<T, Error>>`
2.  `request<T>(method: 'POST', url: string, body: any): Promise<Result<T, Error>>`
Implemente a lógica na assinatura de implementação para lidar com ambos os casos.

<details>
<summary>Ver Solução</summary>

```typescript
// Reutilizando o tipo Result
type Success<T> = { success: true; value: T };
type Failure<E> = { success: false; error: E };
type Result<T, E> = Success<T> | Failure<E>;

class HttpClient {
  // Sobrecarga para GET
  request<T>(method: 'GET', url: string): Promise<Result<T, Error>>;
  // Sobrecarga para POST
  request<T>(method: 'POST', url: string, body: any): Promise<Result<T, Error>>;
  // Implementação
  async request<
    T
  >(
    method: 'GET' | 'POST',
    url: string,
    body?: any
  ): Promise<Result<T, Error>> {
    console.log(`Fazendo ${method} para: ${url}`);

    if (method === 'POST') {
      console.log("Corpo da requisição:", body);
    }

    // Simulação de erro
    if (url.includes('fail')) {
      return { success: false, error: new Error(`Erro de Rede ${method === 'GET' ? 404 : 500}`) };
    }

    // Simulação de sucesso
    const response = { data: { id: 1, name: "Item Falso" } };
    return { success: true, value: response as T };
  }
}

// Teste
interface UserResponse { data: { id: number; name: string; } }

async function testClient() {
  const client = new HttpClient();
  
  const getResult = await client.request<UserResponse>('GET', "/api/users/1");
  if (getResult.success) console.log("GET Sucesso:", getResult.value.data.name);

  const postResult = await client.request<UserResponse>('POST', "/api/users", { name: 'Novo Usuário' });
  if (postResult.success) console.log("POST Sucesso:", postResult.value.data.name);
}

testClient();
```
</details>

### Checklist do Dia
- [ ] Usei sobrecarga de função para um método de cliente HTTP.
- [ ] Integrei o Result Pattern no tratamento de erros de rede.
- [ ] Usei generics para tipar a resposta da API.
- [ ] Combinei múltiplos conceitos da semana em um único projeto.
