# Semana 4: Integração e Refinamento  

## Visão Geral da Semana
Na última semana, nosso foco é a aplicação prática e a integração do TypeScript em ecossistemas do mundo real. Vamos aprender a integrar com bibliotecas externas como o React Hook Form, gerenciar estado de forma segura, organizar nosso código com módulos e path mapping, e escrever testes que também validam nossos tipos. Finalizaremos com uma olhada em otimização de performance e padrões de design avançados, culminando em um projeto final que une todo o conhecimento adquirido.

---

## Dia 22: Form Integration (React Hook Form)

### Foco do Dia
Integrar TypeScript com bibliotecas externas, usando o React Hook Form como exemplo, e aprender a usar `Module Augmentation` para estender tipos de bibliotecas de terceiros.

### Leitura e Teoria (Aprofundada)
- **Integração com Bibliotecas**: Muitas bibliotecas populares (React, Vue, etc.) são escritas em TypeScript ou fornecem seus próprios arquivos de declaração de tipo (`.d.ts`). Isso nos permite usar a biblioteca de forma type-safe.
- **Tipos Utilitários de Bibliotecas**: Bibliotecas como React Hook Form exportam seus próprios tipos utilitários (ex: `Control`, `FieldErrors`, `UseFormSetValue`) que são genéricos e devem ser usados com os tipos do nosso formulário.
- **Module Augmentation**: Permite que você "adicione" declarações a um módulo existente. É útil para estender interfaces de bibliotecas de terceiros para adicionar propriedades customizadas sem precisar criar um fork da biblioteca.

### Documentação Essencial
- [TypeScript com React (React Docs)](https://react.dev/learn/typescript)
- [TypeScript com React Hook Form (RHF Docs)](https://react-hook-form.com/ts)
- [Module Augmentation (Handbook)](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation)

### Prática Guiada (Passo a Passo)

**Exemplo: Tipando um Formulário Simples com React Hook Form**
```typescript
import { useForm, Control, FieldErrors, UseFormSetValue } from 'react-hook-form';

// 1. Definir o tipo dos valores do formulário
interface MyFormValues {
  firstName: string;
  age: number;
}

// 2. Usar o tipo com o hook `useForm`
function MyForm() {
  const { control, formState: { errors }, setValue } = useForm<MyFormValues>();

  // O tipo de `control` é Control<MyFormValues>
  // O tipo de `errors` é FieldErrors<MyFormValues>
  // O tipo de `setValue` é UseFormSetValue<MyFormValues>

  // ... resto do componente JSX
}
```

**Exemplo: Recriando seu tipo `RenderMinFareForm`**
```typescript
import { Control, FieldErrors, UseFormSetValue, UseFormClearErrors } from 'react-hook-form';

// O tipo que define os valores do seu formulário
interface MinFareFormValue {
  yield: number;
  price: number;
}

// O tipo que agrupa todos os props necessários para renderizar o form
export type RenderMinFareForm = {
  control: Control<MinFareFormValue>;
  errors: FieldErrors<MinFareFormValue>;
  setValue: UseFormSetValue<MinFareFormValue>;
  clearErrors: UseFormClearErrors<MinFareFormValue>;
};
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Defina um tipo para um formulário de login, `LoginFormValues`, que contém `email` (string) e `password` (string). Em seguida, crie um tipo `LoginProps` que represente as propriedades que um componente de formulário de login receberia, incluindo `onSubmit` que é uma função que recebe os `LoginFormValues`.

<details>
<summary>Ver Solução</summary>

```typescript
interface LoginFormValues {
  email: string;
  password: string;
}

type LoginProps = {
  onSubmit: (data: LoginFormValues) => void;
};

// Exemplo de uso em um componente (simulado)
function LoginForm(props: LoginProps) {
  // const { handleSubmit } = useForm<LoginFormValues>();
  // return <form onSubmit={handleSubmit(props.onSubmit)}>...</form>
  console.log("Componente de formulário de login renderizado.");
}
```
</details>

**Nível 2: Intermediário**
Crie um tipo `ProfileFormValues` com `name` (string) e `bio` (string, opcional). Crie um tipo `ProfileFormProps` que agrupa as propriedades `control` e `errors` do React Hook Form, devidamente tipadas com `ProfileFormValues`.

<details>
<summary>Ver Solução</summary>

```typescript
import { Control, FieldErrors } from 'react-hook-form';

interface ProfileFormValues {
  name: string;
  bio?: string;
}

type ProfileFormProps = {
  control: Control<ProfileFormValues>;
  errors: FieldErrors<ProfileFormValues>;
};

// Simulação de um componente que recebe esses props
function ProfileFormComponent(props: ProfileFormProps) {
  // <Controller name="name" control={props.control} ... />
  // {props.errors.name && <p>Erro no nome</p>}
  console.log("Componente de perfil renderizado.");
}
```
</details>

**Nível 3: Avançado**
Usando `Module Augmentation`, estenda a interface `DefaultTheme` de uma biblioteca de estilização (ex: `styled-components`). Adicione uma propriedade `customColors` que é um objeto com chaves `primary` e `secondary`.

<details>
<summary>Ver Solução</summary>

```typescript
// Em um arquivo, ex: styled.d.ts

// 1. Importe o tipo original da biblioteca
import 'styled-components';

// 2. Declare o módulo novamente para estendê-lo
declare module 'styled-components' {
  // 3. Estenda a interface DefaultTheme
  export interface DefaultTheme {
    customColors: {
      primary: string;
      secondary: string;
      background: string;
    };
  }
}

// Em outro arquivo, ex: theme.ts
import { DefaultTheme } from 'styled-components';

const myTheme: DefaultTheme = {
  // Agora o TS espera a propriedade customColors
  customColors: {
    primary: '#007bff',
    secondary: '#6c757d',
    background: '#f8f9fa',
  },
};
```
</details>

### Checklist do Dia
- [ ] Entendi como usar tipos genéricos de bibliotecas externas.
- [ ] Criei um tipo para os valores de um formulário.
- [ ] Agrupei os props de um formulário em um único tipo.
- [ ] Usei `Module Augmentation` para estender uma interface de biblioteca.

---

## Dia 23: State Management Type-Safe

### Foco do Dia
Projetar um sistema de gerenciamento de estado (como Redux ou Zustand) de forma totalmente type-safe, com foco em `action creators` e `selectors` tipados.

### Leitura e Teoria (Aprofundada)
- **Store**: Um objeto único que contém todo o estado da aplicação.
- **Actions**: Objetos que descrevem uma intenção de mudar o estado. Geralmente têm uma propriedade `type` (uma string literal) e um `payload` opcional.
- **Reducers**: Funções puras que recebem o estado atual e uma ação, e retornam o novo estado. `(currentState, action) => newState`.
- **Action Creators**: Funções que criam e retornam objetos de ação. Ajuda a evitar erros de digitação no `type` da ação.
- **Selectors**: Funções que extraem e computam dados derivados do estado do store. Memoização (como na biblioteca `reselect`) é frequentemente usada para performance.

### Documentação Essencial
- [Static Typing com Redux (Redux Docs)](https://redux.js.org/usage/usage-with-typescript)

### Prática Guiada (Passo a Passo)

**Exemplo: Tipando Ações e Reducer do Redux**
```typescript
// 1. Definir o tipo do estado
interface CounterState {
  value: number;
}

// 2. Definir os tipos das ações
const INCREMENT = 'counter/increment';
const ADD = 'counter/add';

interface IncrementAction { type: typeof INCREMENT; }
interface AddAction { type: typeof ADD; payload: number; }

type CounterAction = IncrementAction | AddAction;

// 3. Criar o reducer
const initialState: CounterState = { value: 0 };

function counterReducer(state = initialState, action: CounterAction): CounterState {
  switch (action.type) {
    case INCREMENT:
      return { ...state, value: state.value + 1 };
    case ADD:
      // O TS sabe que `action` tem `payload` aqui
      return { ...state, value: state.value + action.payload };
    default:
      return state;
  }
}
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie os `action creators` para as ações `IncrementAction` e `AddAction` do exemplo guiado. As funções devem retornar os objetos de ação devidamente tipados.

<details>
<summary>Ver Solução</summary>

```typescript
// Tipos do exemplo anterior...
const INCREMENT = 'counter/increment';
const ADD = 'counter/add';
interface IncrementAction { type: typeof INCREMENT; }
interface AddAction { type: typeof ADD; payload: number; }

// Action Creators
function increment(): IncrementAction {
  return { type: INCREMENT };
}

function add(amount: number): AddAction {
  return { type: ADD, payload: amount };
}

// Teste
const incrementAction = increment(); // tipo IncrementAction
const addAction = add(5); // tipo AddAction

console.log(incrementAction);
console.log(addAction);
```
</details>

**Nível 2: Intermediário**
Defina um estado para uma lista de tarefas (`todos`). Crie os tipos de ação e o reducer para adicionar uma nova tarefa (`ADD_TODO`) e marcar uma tarefa como completa (`TOGGLE_TODO`).

<details>
<summary>Ver Solução</summary>

```typescript
// 1. Tipos de Estado
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

interface TodosState {
  todos: Todo[];
}

// 2. Tipos de Ação
const ADD_TODO = 'todos/add';
const TOGGLE_TODO = 'todos/toggle';

interface AddTodoAction { type: typeof ADD_TODO; payload: { text: string } }
interface ToggleTodoAction { type: typeof TOGGLE_TODO; payload: { id: number } }

type TodoAction = AddTodoAction | ToggleTodoAction;

// 3. Reducer
const initialTodosState: TodosState = { todos: [] };
let nextTodoId = 0;

function todosReducer(state = initialTodosState, action: TodoAction): TodosState {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, { id: nextTodoId++, text: action.payload.text, completed: false }]
      };
    case TOGGLE_TODO:
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload.id ? { ...todo, completed: !todo.completed } : todo
        )
      };
    default:
      return state;
  }
}
```
</details>

**Nível 3: Avançado**
Crie um seletor tipado `selectCompletedTodos` que recebe o estado completo da aplicação (que contém `todosState`) e retorna apenas a lista de tarefas completas.

<details>
<summary>Ver Solução</summary>

```typescript
// Reutilizando os tipos do Nível 2
interface Todo { id: number; text: string; completed: boolean; }
interface TodosState { todos: Todo[]; }

// Estado global da aplicação
interface AppState {
  todosState: TodosState;
  // ... outros estados
}

// O seletor
function selectCompletedTodos(state: AppState): Todo[] {
  return state.todosState.todos.filter(todo => todo.completed);
}

// Teste
const testState: AppState = {
  todosState: {
    todos: [
      { id: 0, text: 'Aprender TS', completed: true },
      { id: 1, text: 'Dominar o mundo', completed: false },
      { id: 2, text: 'Tomar café', completed: true },
    ]
  }
};

const completed = selectCompletedTodos(testState);
console.log(completed);
```
</details>

### Checklist do Dia
- [ ] Defini o tipo para uma fatia (slice) do estado.
- [ ] Criei tipos de ação usando discriminated unions.
- [ ] Implementei um reducer type-safe.
- [ ] Criei action creators e seletores tipados.

---

## Dia 24: Module System e Path Mapping

### Foco do Dia
Organizar a arquitetura de código usando o sistema de módulos do TypeScript, incluindo `barrel exports` e `path mapping` para imports mais limpos.

### Leitura e Teoria (Aprofundada)
- **Módulos**: Cada arquivo em TypeScript é um módulo. `export` torna variáveis, funções e classes disponíveis para outros módulos. `import` as consome.
- **Barrel Exports**: Um arquivo, geralmente chamado `index.ts`, que re-exporta todos os exports de um diretório. Isso permite que os consumidores importem tudo de um único local, em vez de múltiplos caminhos.
  `import { ServiceA, ServiceB } from './services';` em vez de `... from './services/ServiceA'` e `... from './services/ServiceB'`.
- **Path Mapping**: Uma feature do `tsconfig.json` que permite criar aliases para caminhos de importação. Isso evita imports relativos longos como `../../../../components` e os substitui por aliases como `@components`.

### Documentação Essencial
- [Modules (Handbook)](https://www.typescriptlang.org/docs/handbook/modules.html)
- [Path mapping (tsconfig Reference)](https://www.typescriptlang.org/tsconfig#paths)

### Prática Guiada (Passo a Passo)

**Exemplo 1: Barrel Exports**
```typescript
// Em src/utils/stringUtils.ts
export const capitalize = (s: string) => s.charAt(0).toUpperCase() + s.slice(1);

// Em src/utils/numberUtils.ts
export const isEven = (n: number) => n % 2 === 0;

// Em src/utils/index.ts (o "barrel")
export * from './stringUtils';
export * from './numberUtils';

// Em outro arquivo
import { capitalize, isEven } from '../utils'; // Importa de um único lugar
```

**Exemplo 2: Configurando Path Mapping**
No `tsconfig.json`:
```json
{
  "compilerOptions": {
    "baseUrl": "./src", // Essencial para o path mapping
    "paths": {
      "@components/*": ["components/*"],
      "@services/*": ["services/*"],
      "@domain/*": ["domain/*"]
    }
  }
}
```
Uso no código:
```typescript
// Em vez de: import { User } from '../../domain/models/User';
import { User } from '@domain/models/User';

// Em vez de: import { Button } from '../components/Button';
import { Button } from '@components/Button';
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma estrutura de pastas `domain/errors`. Dentro, crie dois arquivos: `UnexpectedError.ts` e `NotFoundError.ts`, cada um exportando uma classe de erro. Crie um arquivo `index.ts` em `domain/errors` que exporta ambas as classes.

<details>
<summary>Ver Solução</summary>

```typescript
// Em domain/errors/UnexpectedError.ts
export class UnexpectedError extends Error {
  constructor() {
    super('Um erro inesperado aconteceu.');
    this.name = 'UnexpectedError';
  }
}

// Em domain/errors/NotFoundError.ts
export class NotFoundError extends Error {
  constructor() {
    super('Recurso não encontrado.');
    this.name = 'NotFoundError';
  }
}

// Em domain/errors/index.ts
export * from './UnexpectedError';
export * from './NotFoundError';

// Em outro arquivo (ex: app.ts)
// import { UnexpectedError, NotFoundError } from './domain/errors';
```
</details>

**Nível 2: Intermediário**
Configure o `path mapping` no seu `tsconfig.json` para criar um alias `@/` que aponte para o diretório `src/`. Refatore um import que usa um caminho relativo (ex: `../utils`) para usar o novo alias (`@/utils`).

<details>
<summary>Ver Solução</summary>

No `tsconfig.json`:
```json
{
  "compilerOptions": {
    "baseUrl": ".", // ou "./src"
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

No código:
```typescript
// Antes:
// import { capitalize } from '../../utils/stringUtils';

// Depois:
// import { capitalize } from '@/utils/stringUtils';
```
</details>

**Nível 3: Avançado**
Crie uma estrutura de módulos para a sua camada de dados, como no seu exemplo. Crie `data/protocols/http/` com um `index.ts` e um `http-client.ts`. O `http-client.ts` deve exportar um `HttpStatusCode` (enum) e uma interface `HttpClient`. O `index.ts` deve exportar tudo de `http-client.ts`. Configure um alias `@data` para a pasta `data`.

<details>
<summary>Ver Solução</summary>

No `tsconfig.json`:
```json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "@data/*": ["data/*"]
    }
  }
}
```

Estrutura de arquivos:
```
src/
  data/
    protocols/
      http/
        http-client.ts
        index.ts
```

Em `src/data/protocols/http/http-client.ts`:
```typescript
export enum HttpStatusCode {
  ok = 200,
  noContent = 204,
  badRequest = 400,
  notFound = 404,
  serverError = 500,
}

export interface HttpClient<R = any> {
  request: (data: HttpRequest) => Promise<HttpResponse<R>>;
}

export type HttpRequest = { url: string; method: string; body?: any; headers?: any; };
export type HttpResponse<R = any> = { statusCode: HttpStatusCode; body?: R; };
```

Em `src/data/protocols/http/index.ts`:
```typescript
export * from './http-client';
```

Em outro arquivo:
```typescript
import { HttpStatusCode, HttpClient } from '@data/protocols/http';
```
</details>

### Checklist do Dia
- [ ] Entendi a diferença entre `export` e `export default`.
- [ ] Criei um barrel export (`index.ts`) para simplificar imports.
- [ ] Configurei `baseUrl` e `paths` no `tsconfig.json`.
- [ ] Refatorei imports relativos para usar aliases de caminho.

---

## Dia 25: Testing Types

### Foco do Dia
Escrever testes que não apenas validam a lógica de execução, mas também a correção dos tipos, usando mocks type-safe e testes de asserção de tipo.

### Leitura e Teoria (Aprofundada)
- **Mocks Type-Safe**: Ao mockar (simular) módulos ou classes, é crucial que o mock tenha o mesmo tipo do original. Ferramentas como `jest.Mocked<T>` ajudam a garantir isso.
- **Testando Tipos**: Às vezes, queremos testar apenas o tipo, não o valor. Por exemplo, garantir que uma função não pode ser chamada com argumentos errados. Isso geralmente é feito em arquivos `.test-d.ts` (testes de declaração) com ferramentas como `tsd` ou `expect-type`.
- **Mock Factories**: Criar funções que geram mocks consistentes e tipados para os seus testes. Isso reduz a duplicação de código nos testes.

### Documentação Essencial
- [TypeScript com Jest](https://jestjs.io/docs/getting-started#using-typescript)

### Prática Guiada (Passo a Passo)

**Exemplo: Mockando um Serviço com Tipagem**
```typescript
// Em user.service.ts
export class UserService {
  async getUserName(id: string): Promise<string> {
    // ... lógica real
    return "Nome Real";
  }
}

// Em user.controller.test.ts
import { UserService } from './user.service';
import { jest } from '@jest/globals';

// Mocka o módulo inteiro
jest.mock('./user.service');

// Cria uma versão tipada da classe mockada
const MockedUserService = UserService as jest.MockedClass<typeof UserService>;

// Teste
it('should return user name', async () => {
  // O TS sabe que `mock.instances[0].getUserName` existe e é um mock
  MockedUserService.prototype.getUserName.mockResolvedValue("Nome Mockado");
  
  const serviceInstance = new UserService();
  const name = await serviceInstance.getUserName('1');
  expect(name).toBe("Nome Mockado");
});
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Crie uma interface `INotifier` com um método `notify(message: string)`. Crie um mock simples para esta interface em um teste, garantindo que o mock satisfaça a interface.

<details>
<summary>Ver Solução</summary>

```typescript
// Interface original
interface INotifier {
  notify(message: string): void;
}

// Teste
describe('Notifier Test', () => {
  it('should be called with the correct message', () => {
    // O mock é tipado como a interface
    const mockNotifier: INotifier = {
      notify: jest.fn(), // jest.fn() cria uma função mock
    };

    // Simula o uso do notifier
    mockNotifier.notify("hello");

    expect(mockNotifier.notify).toHaveBeenCalledWith("hello");
  });
});
```
</details>

**Nível 2: Intermediário**
Crie uma "mock factory" para um objeto `User`. A factory deve ser uma função `createMockUser(overrides: Partial<User>): User` que cria um usuário padrão e permite sobrescrever propriedades específicas para cada teste.

<details>
<summary>Ver Solução</summary>

```typescript
interface User {
  id: string;
  name: string;
  email: string;
  isAdmin: boolean;
}

function createMockUser(overrides?: Partial<User>): User {
  const defaultUser: User = {
    id: 'user-1',
    name: 'Usuário Padrão',
    email: 'default@test.com',
    isAdmin: false,
  };

  return { ...defaultUser, ...overrides };
}

// Teste
describe('Mock User Factory', () => {
  it('should create a default user', () => {
    const user = createMockUser();
    expect(user.name).toBe('Usuário Padrão');
  });

  it('should override properties', () => {
    const adminUser = createMockUser({ name: 'Admin', isAdmin: true });
    expect(adminUser.name).toBe('Admin');
    expect(adminUser.isAdmin).toBe(true);
  });
});
```
</details>

**Nível 3: Avançado**
Usando uma ferramenta como `expect-type` (ou apenas com comentários, se não estiver configurada), escreva um teste de tipo para garantir que uma função `add(a: number, b: number)` não pode ser chamada com strings.

<details>
<summary>Ver Solução</summary>

```typescript
import { expectTypeOf } from 'expect-type';

function add(a: number, b: number): number {
  return a + b;
}

// Teste de tipo
test('type tests for add function', () => {
  // Verifica se a função aceita números
  expectTypeOf(add).toBeCallableWith(1, 2);

  // Verifica se a função NÃO aceita strings
  expectTypeOf(add).not.toBeCallableWith('1', '2');

  // Verifica o tipo de retorno
  expectTypeOf(add).returns.toBeNumber();
});
```

**Solução sem biblioteca (usando comentários de erro esperado):**
```typescript
function add(a: number, b: number): number {
  return a + b;
}

// @ts-expect-error - Testando que isso deve dar um erro de tipo
add('1', '2');

const result = add(1, 2);
// @ts-expect-error - Testando que o resultado não é uma string
const resultIsString: string = result;
```
</details>

### Checklist do Dia
- [ ] Criei um mock tipado para uma interface.
- [ ] Usei `jest.MockedClass` para mockar uma classe.
- [ ] Criei uma factory para gerar mocks consistentes.
- [ ] Entendi como testar a correção de tipos, não apenas de valores.

---

## Dia 26: Performance e Optimization

### Foco do Dia
Entender como tipos complexos podem impactar a performance do compilador TypeScript e aprender técnicas para otimizá-los.

### Leitura e Teoria (Aprofundada)
- **Custo da Tipagem**: Tipos muito complexos, especialmente os recursivos ou que geram uniões muito grandes, podem deixar o `tsc` (compilador do TypeScript) e o IntelliSense lentos.
- **Análise de Performance**: O compilador do TypeScript tem flags para ajudar a diagnosticar problemas de performance, como `--diagnostics` e `--generateTrace`.
- **Técnicas de Otimização**:
  1.  **Evitar Recursão Infinita**: Cuidado com tipos recursivos que não têm um caso base claro.
  2.  **Interfaces vs. Types**: Interfaces são geralmente melhores para objetos, pois são extensíveis e podem ser ligeiramente mais performáticas em alguns casos devido à forma como são cacheadas internamente.
  3.  **Simplificar Tipos Condicionais**: Tente quebrar tipos condicionais complexos em tipos auxiliares menores.
  4.  **Adiar Computação de Tipos**: Em vez de um tipo que calcula tudo de uma vez, use um tipo genérico que é resolvido apenas quando usado.

### Documentação Essencial
- [TypeScript Performance Wiki](https://github.com/microsoft/TypeScript/wiki/Performance)

### Prática Guiada (Passo a Passo)

**Exemplo: Tipo Recursivo que Pode Ser Lento**
```typescript
// Este tipo pode ser lento se a profundidade for grande
type DeeplyNested<T> = { content: T; next?: DeeplyNested<T> };

// Uma alternativa pode ser limitar a profundidade, se possível
type LimitedDepth<T, D extends number> = D extends 0 ? T : { content: T; next?: LimitedDepth<T, any /* D-1 */> };
// (A matemática de tipos para subtrair 1 é complexa, mas a ideia é limitar a recursão)
```

**Exemplo: Simplificando Uniões Grandes**
```typescript
// Lento: Gera uma união de 1000 tipos literais
type Thousand = 1 | 2 | 3 | ... | 1000;

// Mais performático: Usa um tipo mais geral e valida em tempo de execução
type SmallNumber = number & { __brand: 'SmallNumber' };
function createSmallNumber(n: number): SmallNumber | null {
  if (n > 0 && n <= 1000) return n as SmallNumber;
  return null;
}
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Refatore um tipo que usa `type` para um objeto para usar `interface`. Explique por que `interface` pode ser preferível para objetos que podem ser estendidos.

<details>
<summary>Ver Solução</summary>

```typescript
// Antes
type UserType = {
  id: number;
  name: string;
};

// Depois (Refatorado)
interface UserInterface {
  id: number;
  name: string;
}

// Interfaces podem ser estendidas por outras interfaces e aumentadas (module augmentation).
// Isso as torna mais flexíveis para objetos, especialmente em APIs públicas.
interface AdminUser extends UserInterface {
  permissions: string[];
}
```
</details>

**Nível 2: Intermediário**
Dado um tipo `ComplexMappedType` que faz várias operações, quebre-o em tipos utilitários menores e mais legíveis.

<details>
<summary>Ver Solução</summary>

```typescript
interface Props {
  name: string;
  age: number;
  onNameChange: (name: string) => void;
  onAgeChange: (age: number) => void;
}

// Antes: Um tipo monolítico
type CallbacksOnlyBefore<T> = {
  [K in keyof T as T[K] extends (...args: any[]) => any ? K : never]: T[K]
};

// Depois: Refatorado em tipos menores
// 1. Pega as chaves cujos valores são funções
type FunctionKeys<T> = {
  [K in keyof T]: T[K] extends (...args: any[]) => any ? K : never
}[keyof T];

// 2. Usa Pick para criar o tipo final
type CallbacksOnlyAfter<T> = Pick<T, FunctionKeys<T>>;

// Teste
type PropCallbacks = CallbacksOnlyAfter<Props>;
// { onNameChange: (name: string) => void; onAgeChange: (age: number) => void; }
```
</details>

**Nível 3: Avançado**
Crie um tipo `Path<T>` que gera todas as chaves de um objeto aninhado como uma string com pontos (ex: `'details.address.city'`). Este é um tipo inerentemente recursivo e pesado. Pense em como você poderia otimizá-lo ou quais seriam seus gargalos de performance.

<details>
<summary>Ver Solução</summary>

```typescript
// Este tipo é conhecido por ser pesado e pode causar lentidão no compilador
// com objetos muito grandes ou profundos.

type Path<T, K extends keyof T = keyof T> = 
  K extends string 
    ? T[K] extends Record<string, any> 
      ? `${K}.${Path<T[K]>}` | K
      : K
    : never;

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

type UserProfilePaths = Path<UserProfile>;
// "id" | "details" | "details.name" | "details.address" | "details.address.street" | "details.address.city"

/*
Otimização/Gargalos:
1. Profundidade da Recursão: A principal causa de lentidão. Uma otimização seria adicionar um parâmetro de profundidade para limitar a recursão.
2. Largura do Objeto: Muitos campos em cada nível aumentam o número de uniões geradas.
3. Tipos Condicionais: Cada verificação `T[K] extends Record<string, any>` adiciona custo computacional.
Uma otimização real em um projeto seria talvez não usar este tipo e preferir uma validação em tempo de execução para caminhos de objetos.
*/
```
</details>

### Checklist do Dia
- [ ] Entendi que tipos complexos podem impactar a performance.
- [ ] Sei a diferença entre `interface` e `type` e quando usar cada um.
- [ ] Refatorei um tipo complexo em tipos auxiliares menores.
- [ ] Analisei um tipo recursivo e identifiquei seus possíveis gargalos.

---

## Dia 27: Advanced Patterns

### Foco do Dia
Implementar padrões de design de software clássicos (Design Patterns) em TypeScript, aproveitando o sistema de tipos para torná-los mais seguros e expressivos.

### Leitura e Teoria (Aprofundada)
- **Builder Pattern**: Usado para construir objetos complexos passo a passo. Permite produzir diferentes tipos e representações de um objeto usando o mesmo processo de construção. Ideal para objetos com muitos parâmetros de configuração.
- **Factory Pattern**: Usado para criar objetos sem expor a lógica de criação ao cliente. Uma função ou método "fábrica" decide qual classe concreta instanciar com base em algum parâmetro.
- **Observer Pattern**: Usado para criar uma relação de um-para-muitos entre objetos. Quando um objeto (o `subject`) muda de estado, todos os seus dependentes (os `observers`) são notificados e atualizados automaticamente.

### Documentação Essencial
- [Design Patterns (Refactoring Guru)](https://refactoring.guru/design-patterns)

### Prática Guiada (Passo a Passo)

**Exemplo: Observer Pattern**
```typescript
// A interface para os observadores
interface Observer<T> { update(data: T): void; }

// O sujeito que os observadores observam
class Subject<T> {
  private observers = new Set<Observer<T>>();

  subscribe(observer: Observer<T>) { this.observers.add(observer); }
  unsubscribe(observer: Observer<T>) { this.observers.delete(observer); }

  notify(data: T) {
    this.observers.forEach(observer => observer.update(data));
  }
}

// Exemplo de uso
const newsFeed = new Subject<string>();
const observerA = { update: (data: string) => console.log(`Observer A: ${data}`) };
const observerB = { update: (data: string) => console.log(`Observer B: ${data}`) };

newsFeed.subscribe(observerA);
newsFeed.subscribe(observerB);

newsFeed.notify("Nova notícia importante!");
```

### Exercícios Práticos (Níveis Crescentes)

**Nível 1: Básico**
Implemente uma `SimpleFactory` para criar objetos `Logger`. A factory deve ter um método `createLogger(type: 'console' | 'file')` que retorna uma instância de `ConsoleLogger` ou `FileLogger`, ambas implementando uma interface `ILogger`.

<details>
<summary>Ver Solução</summary>

```typescript
interface ILogger { log(message: string): void; }

class ConsoleLogger implements ILogger {
  log(message: string) { console.log(message); }
}

class FileLogger implements ILogger {
  log(message: string) { console.log(`File: ${message}`); }
}

class LoggerFactory {
  public createLogger(type: 'console' | 'file'): ILogger {
    if (type === 'file') {
      return new FileLogger();
    }
    return new ConsoleLogger();
  }
}

// Teste
const factory = new LoggerFactory();
const consoleLogger = factory.createLogger('console');
const fileLogger = factory.createLogger('file');

consoleLogger.log("Teste console");
fileLogger.log("Teste arquivo");
```
</details>

**Nível 2: Intermediário**
Implemente o **Builder Pattern** para criar um objeto `Pizza`. A classe `PizzaBuilder` deve ter métodos como `setCheese(cheese: string)`, `addTopping(topping: string)`, e um método `build()` que retorna o objeto `Pizza` final.

<details>
<summary>Ver Solução</summary>

```typescript
class Pizza {
  public cheese?: string;
  public toppings: string[] = [];

  describe(): void {
    console.log(`Pizza com queijo ${this.cheese} e coberturas: ${this.toppings.join(', ')}`);
  }
}

class PizzaBuilder {
  private pizza: Pizza;

  constructor() {
    this.pizza = new Pizza();
  }

  setCheese(cheese: string): this {
    this.pizza.cheese = cheese;
    return this;
  }

  addTopping(topping: string): this {
    this.pizza.toppings.push(topping);
    return this;
  }

  build(): Pizza {
    return this.pizza;
  }
}

// Teste
const myPizza = new PizzaBuilder()
  .setCheese('mussarela')
  .addTopping('calabresa')
  .addTopping('cebola')
  .build();

myPizza.describe();
```
</details>

**Nível 3: Avançado**
Crie um sistema de eventos type-safe usando o Observer Pattern. Crie uma classe `EventManager` que permite se inscrever (`on`) e emitir (`emit`) eventos. O sistema deve ser tipado de forma que, ao emitir um evento, o payload seja do tipo correto esperado pelos listeners daquele evento.

<details>
<summary>Ver Solução</summary>

```typescript
// Mapeia nomes de eventos para os tipos de seus payloads
interface EventMap {
  'user:created': { userId: string; name: string; };
  'user:deleted': { userId: string; };
  'product:viewed': { productId: string; };
}

type EventKey = keyof EventMap;

class EventManager {
  private listeners: { [K in EventKey]?: ((payload: EventMap[K]) => void)[] } = {};

  // Se inscreve em um evento
  public on<K extends EventKey>(eventName: K, listener: (payload: EventMap[K]) => void): void {
    if (!this.listeners[eventName]) {
      this.listeners[eventName] = [];
    }
    this.listeners[eventName]?.push(listener);
  }

  // Emite um evento
  public emit<K extends EventKey>(eventName: K, payload: EventMap[K]): void {
    this.listeners[eventName]?.forEach(listener => listener(payload));
  }
}

// Teste
const events = new EventManager();

events.on('user:created', (payload) => {
  // O tipo de `payload` é { userId: string; name: string; }
  console.log(`Novo usuário criado: ${payload.name} (ID: ${payload.userId})`);
});

events.on('user:deleted', (payload) => {
  // O tipo de `payload` é { userId: string; }
  console.log(`Usuário deletado: ${payload.userId}`);
});

events.emit('user:created', { userId: 'u-123', name: 'Lucas' });
events.emit('user:deleted', { userId: 'u-456' });

// events.emit('user:created', { userId: 'u-789' }); // Erro: a propriedade 'name' está faltando.
```
</details>

### Checklist do Dia
- [ ] Implementei o Factory Pattern para criar objetos.
- [ ] Implementei o Builder Pattern para construir um objeto complexo.
- [ ] Implementei o Observer Pattern para notificação de eventos.
- [ ] Usei os recursos de tipo do TypeScript para tornar os padrões mais seguros.

---

## Dias 28-30: Projeto Final

### Foco do Projeto
Consolidar todo o conhecimento adquirido ao longo das 4 semanas para planejar, implementar e refinar uma pequena aplicação ou um componente de sistema complexo. O objetivo é aplicar os padrões e técnicas aprendidas em um contexto coeso.

### Ideia do Projeto: Um Mini-Framework de Formulários Type-Safe
Vamos construir um pequeno framework para gerenciar o estado de formulários, inspirado em bibliotecas como Formik ou React Hook Form, mas muito mais simples. Ele irá demonstrar o uso de classes, generics, mapped types, e mais.

### Dia 28: Planejamento e Estrutura

**Exercício:** Defina os tipos e a classe principal.
1.  Crie uma classe `FormStore<T extends object>` que será o coração do nosso framework.
2.  No construtor, ela deve receber um `initialValues: T`.
3.  Ela deve ter propriedades para armazenar os valores (`values: T`), os erros (`errors: FormErrors<T>`) e o estado de "tocado" (`touched: FormTouched<T>`).
4.  Defina os tipos utilitários `FormErrors<T>` e `FormTouched<T>` usando Mapped Types. `FormErrors` deve ter as mesmas chaves de `T`, mas com valores `string | undefined`. `FormTouched` deve ter valores `boolean | undefined`.

<details>
<summary>Ver Solução</summary>

```typescript
// Tipos utilitários
type FormErrors<T> = {
  [P in keyof T]?: string;
};

type FormTouched<T> = {
  [P in keyof T]?: boolean;
};

class FormStore<T extends object> {
  public values: T;
  public errors: FormErrors<T> = {};
  public touched: FormTouched<T> = {};

  constructor(initialValues: T) {
    this.values = initialValues;
  }

  public getState() {
    return {
      values: this.values,
      errors: this.errors,
      touched: this.touched,
    };
  }
}

// Teste da estrutura
const form = new FormStore({ name: '', email: '' });
console.log(form.getState());
```
</details>

### Dia 29: Implementação dos Métodos

**Exercício:** Adicione os métodos para interagir com o formulário.
1.  Adicione um método `setFieldValue<K extends keyof T>(field: K, value: T[K]): void` que atualiza um valor no `values`.
2.  Adicione um método `setFieldTouched<K extends keyof T>(field: K, isTouched: boolean): void`.
3.  Adicione um método `setErrors(errors: FormErrors<T>): void` que substitui o objeto de erros.
4.  Adicione um método de validação `validate(validationSchema: ValidationSchema<T>): boolean`. O `validationSchema` deve ser um objeto onde cada chave de `T` tem uma função que recebe o valor do campo e retorna uma `string` de erro ou `undefined`.

<details>
<summary>Ver Solução</summary>

```typescript
// Tipos do dia anterior...
type FormErrors<T> = { [P in keyof T]?: string; };
type FormTouched<T> = { [P in keyof T]?: boolean; };

// Novo tipo para o esquema de validação
type ValidationSchema<T> = {
  [K in keyof T]?: (value: T[K]) => string | undefined;
};

class FormStore<T extends object> {
  public values: T;
  public errors: FormErrors<T> = {};
  public touched: FormTouched<T> = {};

  constructor(initialValues: T) { this.values = initialValues; }

  public setFieldValue<K extends keyof T>(field: K, value: T[K]): void {
    this.values[field] = value;
  }

  public setFieldTouched<K extends keyof T>(field: K, isTouched: boolean = true): void {
    this.touched[field] = isTouched;
  }

  public setErrors(errors: FormErrors<T>): void {
    this.errors = errors;
  }

  public validate(validationSchema: ValidationSchema<T>): boolean {
    const newErrors: FormErrors<T> = {};
    let isValid = true;

    for (const key in validationSchema) {
      const validator = validationSchema[key];
      if (validator) {
        const error = validator(this.values[key]);
        if (error) {
          newErrors[key] = error;
          isValid = false;
        }
      }
    }
    this.setErrors(newErrors);
    return isValid;
  }
}
```
</details>

### Dia 30: Refinamento e Uso

**Exercício:** Use o `FormStore` para gerenciar um formulário de registro de usuário.
1.  Defina a interface `UserSignupForm` com `name`, `email`, e `password`.
2.  Crie uma instância do `FormStore` com os valores iniciais.
3.  Crie um `ValidationSchema` para o formulário (ex: nome é obrigatório, email deve conter `@`, senha deve ter mais de 6 caracteres).
4.  Simule a interação do usuário: mude valores, toque em campos e chame a validação. Imprima o estado (`values`, `errors`, `touched`) no console a cada passo.

<details>
<summary>Ver Solução</summary>

```typescript
// Classe e tipos do dia anterior...

// 1. Definir a interface do formulário
interface UserSignupForm {
  name: string;
  email: string;
  password: string;
}

// 2. Criar a instância do FormStore
const signupForm = new FormStore<UserSignupForm>({
  name: '',
  email: '',
  password: '',
});

// 3. Criar o esquema de validação
const signupValidationSchema: ValidationSchema<UserSignupForm> = {
  name: (value) => (value ? undefined : 'Nome é obrigatório'),
  email: (value) => (value.includes('@') ? undefined : 'Email inválido'),
  password: (value) => (value.length > 6 ? undefined : 'Senha muito curta'),
};

// 4. Simular a interação
console.log("Estado Inicial:", signupForm.errors);

// Usuário digita o nome
signupForm.setFieldValue('name', 'Lucas');
signupForm.setFieldTouched('name');

// Usuário digita um email inválido e sai do campo
signupForm.setFieldValue('email', 'lucas.com');
signupForm.setFieldTouched('email');

// Valida o formulário
signupForm.validate(signupValidationSchema);
console.log("Estado após validação 1:", signupForm.errors);
// { email: 'Email inválido', password: 'Senha muito curta' }

// Usuário corrige o email e a senha
signupForm.setFieldValue('email', 'lucas@ts.com');
signupForm.setFieldValue('password', '1234567');

// Valida novamente
signupForm.validate(signupValidationSchema);
console.log("Estado após validação 2:", signupForm.errors);
// {}
```
</details>

### Checklist Final
- [ ] Planejei a estrutura de uma classe complexa com tipos genéricos.
- [ ] Usei Mapped Types para criar tipos de estado derivados.
- [ ] Implementei a lógica de negócio em métodos de classe.
- [ ] Usei a classe final para resolver um problema prático.
- [ ] Sinto-me confiante para arquitetar aplicações TypeScript intermediárias.