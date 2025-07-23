# Plano de Estudos TypeScript - 30 Dias
## Do Iniciante ao Intermediário

### Estrutura do Plano
- **Duração**: 30 dias (1h30 por dia)
- **Tempo por sessão**: 35min teoria + 45min prática + 10min documentação
- **Metodologia**: Progressão incremental com projetos práticos
- **Objetivo**: Dominar TypeScript intermediário com foco em classes, tipagens robustas e patterns arquiteturais

---

## SEMANA 1: Fundamentos Críticos e Classes

### **Dia 1: Classes Fundamentais**
**Teoria**: Constructor, properties, methods, access modifiers
**Prática**: Criar classe `User` com validações
**Exercício**: 
```typescript
// Implementar classe com constructor overloading
class DatabaseConnection {
  constructor(url: string);
  constructor(host: string, port: number, database: string);
}
```
**Documentação**: [Classes - TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/2/classes.html)

### **Dia 2: Context do `this` - Parte 1**
**Teoria**: `this` binding, arrow functions vs regular functions
**Prática**: Implementar método de chaining type-safe
**Exercício**: Resolver problemas de `this` context em callbacks
```typescript
class QueryBuilder {
  where(condition: string): this { /* */ }
  orderBy(field: string): this { /* */ }
}
```
**Documentação**: [This Types](https://www.typescriptlang.org/docs/handbook/2/classes.html#this-types)

### **Dia 3: Eliminando `any` - Parte 1**
**Teoria**: `unknown` vs `any`, type assertions seguras
**Prática**: Refatorar código com `any` para tipagens seguras
**Exercício**: Implementar função de parsing de JSON type-safe
```typescript
function safeJsonParse<T>(json: string): T | Error {
  // Implementar sem usar any
}
```
**Documentação**: [Unknown Type](https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown)

### **Dia 4: Type Guards e Narrowing**
**Teoria**: `typeof`, `instanceof`, custom type guards
**Prática**: Criar type guards para validação de dados da API
**Exercício**: Implementar type guards para discriminated unions
```typescript
type ApiResponse = SuccessResponse | ErrorResponse;
function isSuccessResponse(response: ApiResponse): response is SuccessResponse {
  // Implementar
}
```
**Documentação**: [Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

### **Dia 5: Utility Types Nativos - Record e Básicos**
**Teoria**: `Record<K,V>`, `Pick`, `Omit`, `Partial`, `Required`
**Prática**: Criar sistema de configuração type-safe
**Exercício**: Implementar mapeamento como seu exemplo:
```typescript
const tooltipContent: Record<string, DictionaryPath> = {
  [columnType.YIELD]: 'min_fare.table.tooltips.yield',
};
```
**Documentação**: [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

### **Dia 6: Template Literal Types**
**Teoria**: Template literals, key remapping
**Prática**: Criar sistema de rotas type-safe
**Exercício**: Implementar auto-complete para caminhos de API
```typescript
type ApiRoutes = `/api/${'users' | 'posts'}/${string}`;
```
**Documentação**: [Template Literal Types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)

### **Dia 7: Project Mini #1**
**Prática Intensiva**: Implementar classe `BaseRepository` combinando todos os conceitos
- Classes com generics
- Type guards para validação
- Methods com `this` return para chaining
- Utility types para operações CRUD

---

## SEMANA 2: Classes Avançadas e Generics

### **Dia 8: Herança e Abstract Classes**
**Teoria**: `extends`, `super`, `abstract` methods e properties
**Prática**: Implementar hierarquia de classes para diferentes tipos de usuários
**Exercício**: Criar abstract class similar ao seu `GetTableDataService`
```typescript
abstract class BaseService<TResponse, TParams> {
  abstract handle(params: TParams): Promise<TResponse>;
}
```
**Documentação**: [Abstract Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html#abstract-classes-and-members)

### **Dia 9: Generics em Classes**
**Teoria**: Generic constraints, multiple type parameters, default types
**Prática**: Criar sistema de cache genérico type-safe
**Exercício**: Implementar `DataService<T extends BaseEntity, K extends keyof T>`
**Documentação**: [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

### **Dia 10: Context `this` - Parte 2 Avançada**
**Teoria**: `this` parameters, `ThisType<T>`, `this` em generics
**Prática**: Implementar fluent API com tipagem correta do `this`
**Exercício**: Criar builder pattern type-safe
```typescript
class FormBuilder<T> {
  field<K extends keyof T>(name: K): FormBuilder<T> { /* */ }
  build(): Form<T> { /* */ }
}
```
**Documentação**: [This Parameters](https://www.typescriptlang.org/docs/handbook/2/functions.html#declaring-this-in-a-function)

### **Dia 11: Decorators**
**Teoria**: Method decorators, property decorators, metadata
**Prática**: Criar sistema de validação com decorators
**Exercício**: Implementar `@validate`, `@cache`, `@log` decorators
**Documentação**: [Decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)

### **Dia 12: Eliminando `any` - Parte 2**
**Teoria**: `never` type, exhaustive checking, branded types
**Prática**: Refatorar APIs externas sem tipagem
**Exercício**: Criar wrapper type-safe para biblioteca sem tipos
```typescript
// Converter isto para type-safe:
declare const untypedLibrary: any;
```
**Documentação**: [Never Type](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)

### **Dia 13: Conditional Types**
**Teoria**: `T extends U ? X : Y`, `infer` keyword
**Prática**: Criar utility types customizados
**Exercício**: Implementar `DeepPartial<T>`, `NonNullable<T>`
```typescript
type DeepPartial<T> = T extends object ? {
  [P in keyof T]?: DeepPartial<T[P]>;
} : T;
```
**Documentação**: [Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)

### **Dia 14: Project Mini #2**
**Prática Intensiva**: Sistema de autenticação completo
- Abstract classes para diferentes providers
- Generics para diferentes tipos de usuário
- Decorators para autorização
- Conditional types para permissões

---

## SEMANA 3: Patterns Arquiteturais e Function Types

### **Dia 15: Function Overloading**
**Teoria**: Multiple signatures, conditional returns
**Prática**: Criar API com diferentes assinaturas baseadas em parâmetros
**Exercício**: Implementar função que retorna tipos diferentes baseado no input
```typescript
function getData(id: string): Promise<User>;
function getData(filter: Filter): Promise<User[]>;
```
**Documentação**: [Function Overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads)

### **Dia 16: Mapped Types Avançados**
**Teoria**: Key remapping, filtering, template literals em mapped types
**Prática**: Criar sistema de forms dinâmico
**Exercício**: Implementar `FormConfig<T>` que gera configuração baseada no tipo
```typescript
type FormConfig<T> = {
  [K in keyof T as `${string & K}Config`]: FieldConfig<T[K]>
}
```
**Documentação**: [Mapped Types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

### **Dia 17: Service Layer Architecture**
**Teoria**: Dependency injection, interface segregation
**Prática**: Recriar e melhorar seu `GetTableDataService`
**Exercício**: Implementar sistema completo de services com DI
```typescript
abstract class GetTableDataService<TResponse, TParams extends Record<string, any>> {
  // Sua implementação + melhorias
}
```
**Documentação**: [Interfaces](https://www.typescriptlang.org/docs/handbook/2/objects.html)

### **Dia 18: Error Handling Type-Safe**
**Teoria**: Result pattern, discriminated unions para errors
**Prática**: Implementar sistema de error handling sem exceptions
**Exercício**: Criar `Result<T, E>` pattern
```typescript
type Result<T, E = Error> = Success<T> | Failure<E>;
```
**Documentação**: [Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types)

### **Dia 19: Advanced Record Patterns**
**Teoria**: `Record` com template literals, const assertions
**Prática**: Criar sistema de internacionalização type-safe
**Exercício**: Implementar dictionary pattern como seu exemplo
```typescript
const tooltipContent: Record<ColumnType, DictionaryPath> = {
  // Implementar com type safety completo
} as const;
```
**Documentação**: [Const Assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)

### **Dia 20: Utility Types Avançados**
**Teoria**: `ReturnType`, `Parameters`, `ConstructorParameters`, `InstanceType`
**Prática**: Criar meta-programming utilities
**Exercício**: Implementar factory pattern type-safe
```typescript
type ServiceFactory<T> = {
  create(...args: ConstructorParameters<T>): InstanceType<T>
}
```
**Documentação**: [Advanced Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

### **Dia 21: Project Mini #3**
**Prática Intensiva**: Sistema de HTTP client type-safe
- Service layer com error handling
- Function overloading para diferentes métodos
- Generic constraints para request/response
- Record patterns para configuração

---

## SEMANA 4: Integração e Refinamento

### **Dia 22: Form Integration (React Hook Form)**
**Teoria**: Integration com bibliotecas externas, module augmentation
**Prática**: Implementar tipagens como seu exemplo `RenderMinFareForm`
**Exercício**: Criar form handler completamente tipado
```typescript
export type RenderMinFareForm = {
  control: Control<MinFareFormValue>;
  errors: FieldErrors<MinFareFormValue>;
  setValue: UseFormSetValue<MinFareFormValue>;
  clearErrors: UseFormClearErrors<MinFareFormValue>;
};
```
**Documentação**: [Module Augmentation](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation)

### **Dia 23: State Management Type-Safe**
**Teoria**: Store patterns, action creators, selectors tipados
**Prática**: Implementar store pattern com TypeScript
**Exercício**: Criar Redux-like store completamente tipado
**Documentação**: [Literal Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types)

### **Dia 24: Module System e Path Mapping**
**Teoria**: Barrel exports, path mapping, declaration files
**Prática**: Organizar arquitetura de módulos como nos seus exemplos
**Exercício**: Configurar sistema de imports absolutos
```typescript
import { HttpStatusCode } from '@data/protocols/http';
import { UnexpectedError } from '@domain/errors';
```
**Documentação**: [Module Resolution](https://www.typescriptlang.org/docs/handbook/module-resolution.html)

### **Dia 25: Testing Types**
**Teoria**: Type-safe mocks, testing utilities
**Prática**: Criar mocks tipados para seus services
**Exercício**: Implementar mock factory type-safe
**Documentação**: [Testing](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html)

### **Dia 26: Performance e Optimization**
**Teoria**: Type checking performance, compilation optimization
**Prática**: Otimizar tipos complexos, evitar deep recursion
**Exercício**: Refatorar tipos pesados para performance
**Documentação**: [Performance](https://github.com/microsoft/TypeScript/wiki/Performance)

### **Dia 27: Advanced Patterns**
**Teoria**: Builder pattern, Factory pattern, Observer pattern em TS
**Prática**: Implementar patterns GOF com TypeScript
**Exercício**: Criar sistema de eventos type-safe
**Documentação**: [Advanced Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

### **Dia 28: Project Final - Setup**
**Planejamento**: Arquitetura completa de uma aplicação
- Definir estrutura de pastas
- Configurar tipos base
- Planejar services e repositories

### **Dia 29: Project Final - Implementation**
**Desenvolvimento**: Implementar sistema completo
- Services como `GetTableDataService`
- Form handling type-safe
- Error handling robusto
- Classes e abstrações

### **Dia 30: Project Final - Review e Refinamento**
**Finalização**: Code review, optimizations, documentação
- Revisar toda implementação
- Aplicar best practices aprendidas
- Documentar patterns utilizados

---

## Recursos Adicionais

### Documentação Principal
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Type Challenges](https://github.com/type-challenges/type-challenges)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)

### Exercícios Complementares
- Cada dia incluirá 2-3 exercícios extras para praticar
- Mini-projetos semanais para consolidar conhecimento
- Refatoração de código real (como seus exemplos)

### Métricas de Progresso
- **Dia 7**: Capaz de criar classes robustas com type safety
- **Dia 14**: Domina generics e patterns arquiteturais básicos
- **Dia 21**: Implementa services complexos sem `any`
- **Dia 30**: Arquiteta aplicações TypeScript intermediárias

Este plano foi estruturado para você evoluir naturalmente dos conceitos que já domina (interfaces, types básicos) para implementações robustas como as dos seus exemplos, sempre priorizando tipagem segura e eliminação de `any`.