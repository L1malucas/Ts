📚 **APOSTILA: ARMAZENAMENTO WEB MODERNO**

**Disciplina: Tecnologias de Armazenamento Client-Side**

📋 **SUMÁRIO**

1.  localStorage
2.  sessionStorage
3.  Cookies
4.  IndexedDB
5.  Cache API (Service Workers)
6.  OPFS (Origin Private File System)
7.  Projeto Final

🎯 **OBJETIVOS DE APRENDIZAGEM**

Ao final desta disciplina, o aluno será capaz de:

- Compreender as diferenças entre os tipos de storage do navegador
- Implementar soluções de armazenamento adequadas para cada cenário
- Desenvolver aplicações offline-first
- Otimizar performance através de cache inteligente
- Trabalhar com grandes volumes de dados no client-side

---

## 1. localStorage

📖 **CONCEITOS FUNDAMENTAIS**

O localStorage é uma API de armazenamento web que permite salvar dados no navegador de forma **persistente**. Os dados permanecem disponíveis mesmo após fechar o navegador.

Características:

- **Capacidade**: ~5-10MB por origem
- **Persistência**: Permanente (até ser limpo manualmente)
- **Escopo**: Por origem (protocolo + domínio + porta)
- **Sincronismo**: API síncrona
- **Tipo de dados**: Apenas strings

Quando usar:

- Preferências do usuário
- Configurações da aplicação
- Cache de dados pequenos
- Estado da aplicação que deve persistir

🔧 **IMPLEMENTAÇÃO PRÁTICA**

Operações Básicas:

```javascript
// Salvar dados
localStorage.setItem('usuario', 'João Silva');
localStorage.setItem('configuracoes', JSON.stringify({
  tema: 'dark',
  idioma: 'pt-BR'
}));

// Recuperar dados
const usuario = localStorage.getItem('usuario');
const config = JSON.parse(localStorage.getItem('configuracoes') || '{}');

// Verificar se existe
if (localStorage.getItem('token')) {
  console.log('Usuário logado');
}

// Remover item específico
localStorage.removeItem('usuario');

// Limpar tudo
localStorage.clear();

// Obter número de itens
console.log(localStorage.length);

// Iterar pelos itens
for (let i = 0; i < localStorage.length; i++) {
  const key = localStorage.key(i);
  const value = localStorage.getItem(key);
  console.log(key, value);
}
```

Service Angular para localStorage:

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class LocalStorageService {
  setItem(key: string, value: any): void {
    try {
      localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error('Erro ao salvar no localStorage:', error);
    }
  }

  getItem<T>(key: string): T | null {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : null;
    } catch (error) {
      console.error('Erro ao ler do localStorage:', error);
      return null;
    }
  }

  removeItem(key: string): void {
    localStorage.removeItem(key);
  }

  clear(): void {
    localStorage.clear();
  }

  exists(key: string): boolean {
    return localStorage.getItem(key) !== null;
  }
}
```

📺 **CONTEÚDOS PARA ASSISTIR**

- **YouTube**: "LocalStorage JavaScript Tutorial" - Programming with Mosh
- **YouTube**: "Angular LocalStorage Service" - Academind
- **Curso Online**: "JavaScript Web Storage" - freeCodeCamp

📚 **FONTES DE ESTUDO**

- [TreinaWeb - LocalStorage vs SessionStorage](https://www.treinaweb.com.br/blog/quando-usar-sessionstorage-e-localstorage)
- [Angular Training - Storage Guide](https://www.angulartraining.com/daily-newsletter/localstorage-and-sessionstorage/)
- [JSCrambler - Angular Local Storage](https://jscrambler.com/blog/working-with-angular-local-storage)

📝 **EXERCÍCIOS PRÁTICOS**

Exercício 1 - Sistema de Preferências

Crie um sistema que salve as preferências do usuário:

- Tema (claro/escuro)
- Idioma preferido
- Tamanho da fonte

Exercício 2 - Carrinho de Compras

Implemente um carrinho que persista entre sessões:

- Adicionar/remover produtos
- Calcular total
- Manter estado após fechar o navegador

Exercício 3 - Histórico de Pesquisas

Desenvolva um sistema que armazene:

- Últimas 10 pesquisas do usuário
- Data/hora de cada pesquisa
- Funcionalidade de limpar histórico

---

## 2. sessionStorage

📖 **CONCEITOS FUNDAMENTAIS**

O sessionStorage funciona de forma similar ao localStorage, mas os dados são **temporários** e existem apenas durante a sessão da aba do navegador.

Características:

- **Capacidade**: ~5-10MB por origem
- **Persistência**: Apenas durante a sessão da aba
- **Escopo**: Por aba do navegador
- **Sincronismo**: API síncrona
- **Tipo de dados**: Apenas strings

Quando usar:

- Dados temporários de formulários
- Estado da navegação
- Informações sensíveis que não devem persistir
- Dados de sessão de usuário

🔧 **IMPLEMENTAÇÃO PRÁTICA**

Operações Básicas:

```javascript
// API idêntica ao localStorage
sessionStorage.setItem('sessaoAtual', JSON.stringify({
  usuario: 'João',
  inicioSessao: new Date().getTime(),
  paginaAtual: 'dashboard'
}));

// Recuperar dados da sessão
const sessao = JSON.parse(sessionStorage.getItem('sessaoAtual') || '{}');

// Verificar tempo de sessão
const tempoSessao = Date.now() - sessao.inicioSessao;
if (tempoSessao > 30 * 60 * 1000) { // 30 minutos
  console.log('Sessão expirada');
  sessionStorage.clear();
}
```

Service Angular para sessionStorage:

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class SessionStorageService {
  private readonly SESSION_TIMEOUT = 30 * 60 * 1000; // 30 minutos

  setItem(key: string, value: any): void {
    try {
      const data = {
        value,
        timestamp: Date.now()
      };
      sessionStorage.setItem(key, JSON.stringify(data));
    } catch (error) {
      console.error('Erro ao salvar no sessionStorage:', error);
    }
  }

  getItem<T>(key: string): T | null {
    try {
      const item = sessionStorage.getItem(key);
      if (!item) return null;

      const data = JSON.parse(item);

      // Verificar se não expirou
      if (Date.now() - data.timestamp > this.SESSION_TIMEOUT) {
        this.removeItem(key);
        return null;
      }
      return data.value;
    } catch (error) {
      console.error('Erro ao ler do sessionStorage:', error);
      return null;
    }
  }

  removeItem(key: string): void {
    sessionStorage.removeItem(key);
  }

  clear(): void {
    sessionStorage.clear();
  }
}
```

📺 **CONTEÚDOS PARA ASSISTIR**

- **YouTube**: "SessionStorage vs LocalStorage" - Web Dev Simplified
- **YouTube**: "Angular Session Management" - Codevolution
- **Tutorial**: "Browser Storage Comparison" - Traversy Media

📚 **FONTES DE ESTUDO**

- [TechSpawn - Comparação de Storage](https://techspawn.com/localstorage-session-storage-or-cookies-in-angular/)
- [NgGirls - Tutorial Prático](https://ng-girls.gitbook.io/todo-list-tutorial-portuguese/local_storage)

📝 **EXERCÍCIOS PRÁTICOS**

Exercício 1 - Formulário Multipasso

Implemente um wizard que salve o progresso:

- Salvar dados a cada passo
- Restaurar se usuário voltar à página
- Limpar ao concluir

Exercício 2 - Estado de Navegação

Crie um sistema que mantenha:

- Filtros aplicados em uma listagem
- Página atual de paginação
- Ordenação selecionada

---

## 3. Cookies

📖 **CONCEITOS FUNDAMENTAIS**

Cookies são pequenos arquivos de texto que o navegador armazena e envia automaticamente para o servidor a cada requisição HTTP.

Características:

- **Capacidade**: ~4KB por cookie
- **Persistência**: Configurável (sessão ou data específica)
- **Escopo**: Configurável (domínio e path)
- **Automatismo**: Enviados automaticamente ao servidor
- **Segurança**: Opções HttpOnly, Secure, SameSite

Quando usar:

- Autenticação e sessões
- Rastreamento de usuário
- Personalização baseada em servidor
- Conformidade com LGPD/GDPR

🔧 **IMPLEMENTAÇÃO PRÁTICA**

Manipulação Básica:

```javascript
// Criar cookie simples
document.cookie = "usuario=joao";

// Cookie com expiração
const dataExpiracao = new Date();
dataExpiracao.setTime(dataExpiracao.getTime() + (24 * 60 * 60 * 1000)); // 24 horas
document.cookie = `sessao=abc123; expires=${dataExpiracao.toUTCString()}; path=/`;

// Cookie seguro
document.cookie = "token=xyz789; Secure; HttpOnly; SameSite=Strict";

// Função para ler cookie
function getCookie(nome) {
  const nomeEQ = nome + "=";
  const cookies = document.cookie.split(';');
  for(let i = 0; i < cookies.length; i++) {
    let cookie = cookies[i];
    while (cookie.charAt(0) === ' ') {
      cookie = cookie.substring(1, cookie.length);
    }
    if (cookie.indexOf(nomeEQ) === 0) {
      return cookie.substring(nomeEQ.length, cookie.length);
    }
  }
  return null;
}

// Função para deletar cookie
function deleteCookie(nome) {
  document.cookie = nome + "=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
}
```

Service Angular para Cookies:

```typescript
import { Injectable } from '@angular/core';

export interface CookieOptions {
  expires?: Date;
  path?: string;
  domain?: string;
  secure?: boolean;
  sameSite?: 'Strict' | 'Lax' | 'None';
}

@Injectable({
  providedIn: 'root'
})
export class CookieService {
  set(name: string, value: string, options: CookieOptions = {}): void {
    let cookie = `${encodeURIComponent(name)}=${encodeURIComponent(value)}`;

    if (options.expires) {
      cookie += `; expires=${options.expires.toUTCString()}`;
    }
    if (options.path) {
      cookie += `; path=${options.path}`;
    }
    if (options.domain) {
      cookie += `; domain=${options.domain}`;
    }
    if (options.secure) {
      cookie += '; Secure';
    }
    if (options.sameSite) {
      cookie += `; SameSite=${options.sameSite}`;
    }
    document.cookie = cookie;
  }

  get(name: string): string | null {
    const nameEQ = encodeURIComponent(name) + "=";
    const cookies = document.cookie.split(';');

    for (let cookie of cookies) {
      cookie = cookie.trim();
      if (cookie.indexOf(nameEQ) === 0) {
        return decodeURIComponent(cookie.substring(nameEQ.length));
      }
    }
    return null;
  }

  delete(name: string, path: string = '/'): void {
    this.set(name, '', {
      expires: new Date(0),
      path: path
    });
  }

  exists(name: string): boolean {
    return this.get(name) !== null;
  }

  getAll(): { [key: string]: string } {
    const cookies: { [key: string]: string } = {};
    if (document.cookie) {
      document.cookie.split(';').forEach(cookie => {
        const [name, value] = cookie.trim().split('=');
        cookies[decodeURIComponent(name)] = decodeURIComponent(value || '');
      });
    }
    return cookies;
  }
}
```

📺 **CONTEÚDOS PARA ASSISTIR**

- **YouTube**: "HTTP Cookies Explained" - Hussein Nasser
- **YouTube**: "Cookie vs LocalStorage vs SessionStorage" - Web Dev Simplified
- **Curso**: "Web Security & Cookies" - OWASP

📚 **FONTES DE ESTUDO**

- [GeeksforGeeks - Diferenças entre Storage](https://www.geeksforgeeks.org/difference-between-local-storage-session-storage-and-cookies/)
- [DigitalOcean - Introdução ao Storage](https://www.digitalocean.com/community/tutorials/js-introduction-localstorage-sessionstorage)

📝 **EXERCÍCIOS PRÁTICOS**

Exercício 1 - Sistema de Login

Implemente autenticação com cookies:

- Cookie de sessão
- Lembrar usuário (persistent cookie)
- Logout seguro

Exercício 2 - Banner de Cookies (LGPD)

Crie um sistema de consentimento:

- Banner de aceitação
- Categorização de cookies
- Configurações de privacidade

---

## 4. IndexedDB

📖 **CONCEITOS FUNDAMENTAIS**

IndexedDB é um banco de dados NoSQL completo que roda no navegador, permitindo armazenar grandes quantidades de dados estruturados.

Características:

- **Capacidade**: Limitada pelo espaço em disco
- **Persistência**: Permanente
- **Tipo de dados**: Objetos JavaScript complexos
- **Transações**: Suporte completo ACID
- **Índices**: Consultas rápidas
- **Assíncrono**: API baseada em Promises/Events

Quando usar:

- Aplicações offline-first
- Cache de dados grandes
- Sincronização de dados
- Armazenamento de arquivos/blobs

🔧 **IMPLEMENTAÇÃO PRÁTICA**

Configuração Básica:

```javascript
// Abrir banco de dados
function abrirBanco() {
  return new Promise((resolve, reject) => {
    const request = indexedDB.open('MeuApp', 1);

    request.onerror = () => reject(request.error);
    request.onsuccess = () => resolve(request.result);

    request.onupgradeneeded = (event) => {
      const db = event.target.result;
      // Criar object store
      if (!db.objectStoreNames.contains('usuarios')) {
        const store = db.createObjectStore('usuarios', {
          keyPath: 'id',
          autoIncrement: true
        });
        // Criar índices
        store.createIndex('email', 'email', { unique: true });
        store.createIndex('nome', 'nome', { unique: false });
      }
    };
  });
}

// Operações CRUD
class UsuarioRepository {
  constructor(db) {
    this.db = db;
  }

  async criar(usuario) {
    const transaction = this.db.transaction(['usuarios'], 'readwrite');
    const store = transaction.objectStore('usuarios');
    return store.add(usuario);
  }

  async buscarPorId(id) {
    const transaction = this.db.transaction(['usuarios'], 'readonly');
    const store = transaction.objectStore('usuarios');
    return store.get(id);
  }

  async buscarPorEmail(email) {
    const transaction = this.db.transaction(['usuarios'], 'readonly');
    const store = transaction.objectStore('usuarios');
    const index = store.index('email');
    return index.get(email);
  }

  async listarTodos() {
    const transaction = this.db.transaction(['usuarios'], 'readonly');
    const store = transaction.objectStore('usuarios');
    return store.getAll();
  }

  async atualizar(usuario) {
    const transaction = this.db.transaction(['usuarios'], 'readwrite');
    const store = transaction.objectStore('usuarios');
    return store.put(usuario);
  }

  async deletar(id) {
    const transaction = this.db.transaction(['usuarios'], 'readwrite');
    const store = transaction.objectStore('usuarios');
    return store.delete(id);
  }
}
```

Usando Dexie.js (Wrapper Simplificado):

```typescript
import Dexie, { Table } from 'dexie';

export interface Usuario {
  id?: number;
  nome: string;
  email: string;
  idade: number;
  criadoEm: Date;
}

export class AppDatabase extends Dexie {
  usuarios!: Table<Usuario>;

  constructor() {
    super('AppDatabase');
    this.version(1).stores({
      usuarios: '++id, nome, email, idade, criadoEm'
    });
  }
}

export const db = new AppDatabase();

// Service Angular com Dexie
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class UsuarioService {
  async criarUsuario(usuario: Omit<Usuario, 'id'>): Promise<number> {
    return await db.usuarios.add({
      ...usuario,
      criadoEm: new Date()
    });
  }

  async buscarUsuarios(): Promise<Usuario[]> {
    return await db.usuarios.orderBy('nome').toArray();
  }

  async buscarPorEmail(email: string): Promise<Usuario | undefined> {
    return await db.usuarios.where('email').equals(email).first();
  }

  async atualizarUsuario(id: number, dados: Partial<Usuario>): Promise<number> {
    return await db.usuarios.update(id, dados);
  }

  async deletarUsuario(id: number): Promise<void> {
    await db.usuarios.delete(id);
  }

  async pesquisarPorNome(termo: string): Promise<Usuario[]> {
    return await db.usuarios
      .where('nome')
      .startsWithIgnoreCase(termo)
      .toArray();
  }
}
```

📺 **CONTEÚDOS PARA ASSISTIR**

- **YouTube**: "IndexedDB Crash Course" - Traversy Media
- **YouTube**: "Dexie.js Tutorial" - The Net Ninja
- **Curso**: "Offline First Web Apps" - Google Developers

📚 **FONTES DE ESTUDO**

- [FreeCodeCamp - Guia Completo IndexedDB](https://www.freecodecamp.org/news/a-quick-but-complete-guide-to-indexeddb-25f030425501/)
- [Telerik - Guia para Iniciantes](https://www.telerik.com/blogs/beginners-guide-indexeddb)
- [Dexie.js - Tutorial Angular](https://dexie.org/docs/Tutorial/Angular)

📝 **EXERCÍCIOS PRÁTICOS**

Exercício 1 - Sistema de Tarefas Offline

Desenvolva um todo app que funcione offline:

- CRUD completo de tarefas
- Categorias e tags
- Busca por texto
- Sincronização quando online

Exercício 2 - Cache de Dados da API

Implemente um sistema de cache inteligente:

- Armazenar respostas da API
- Refresh automático
- Estratégia de invalidação

---

## 5. Cache API (Service Workers)

📖 **CONCEITOS FUNDAMENTAIS**

A Cache API, junto com Service Workers, permite criar aplicações web que funcionam offline interceptando requisições de rede e servindo conteúdo cacheado.

Características:

- **Capacidade**: Limitada pelo espaço em disco
- **Persistência**: Permanente (até ser limpa)
- **Escopo**: Controlado por Service Worker
- **Estratégias**: Cache-first, Network-first, etc.
- **Atualizações**: Controle granular de versioning

Quando usar:

- Progressive Web Apps (PWA)
- Aplicações offline-first
- Cache de recursos estáticos
- Otimização de performance

🔧 **IMPLEMENTAÇÃO PRÁTICA**

Service Worker Básico:

```javascript
// sw.js
const CACHE_NAME = 'meu-app-v1';
const urlsToCache = [
  '/',
  '/styles/main.css',
  '/scripts/app.js',
  '/images/logo.png'
];

// Instalação - cachear recursos
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => {
        console.log('Cache aberto');
        return cache.addAll(urlsToCache);
      })
  );
});

// Interceptar requisições
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        // Cache hit - retornar resposta
        if (response) {
          return response;
        }
        // Cache miss - fazer requisição
        return fetch(event.request)
          .then((response) => {
            const responseClone = response.clone();
            caches.open(CACHE_NAME)
              .then((cache) => {
                cache.put(event.request, responseClone);
              });
            return response;
          });
      })
  );
});

// Atualização - limpar caches antigos
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cacheName) => {
          if (cacheName !== CACHE_NAME) {
            console.log('Deletando cache antigo:', cacheName);
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
});
```

Estratégias de Cache:

```javascript
// Cache First - para recursos estáticos
async function cacheFirst(request) {
  const cachedResponse = await caches.match(request);
  return cachedResponse || fetch(request);
}

// Network First - para dados dinâmicos
async function networkFirst(request) {
  try {
    const response = await fetch(request);
    const cache = await caches.open(CACHE_NAME);
    cache.put(request, response.clone());
    return response;
  } catch (error) {
    const cachedResponse = await caches.match(request);
    return cachedResponse || new Response('Offline', { status: 503 });
  }
}

// Stale While Revalidate - melhor UX
async function staleWhileRevalidate(request) {
  const cachedResponse = await caches.match(request);
  const fetchPromise = fetch(request).then((response) => {
    const cache = caches.open(CACHE_NAME);
    cache.then((c) => c.put(request, response.clone()));
    return response;
  });
  return cachedResponse || fetchPromise;
}
```

Angular Service Worker:

```typescript
// Adicionar ao projeto Angular
// ng add @angular/pwa

// Configuração ngsw-config.json
/*
{
  "index": "/index.html",
  "assetGroups": [
    {
      "name": "app",
      "installMode": "prefetch",
      "resources": {
        "files": [
          "/favicon.ico",
          "/index.html",
          "/manifest.webmanifest",
          "/*.css",
          "/*.js"
        ]
      }
    }
  ],
  "dataGroups": [
    {
      "name": "api-cache",
      "urls": ["/api/**"],
      "cacheConfig": {
        "strategy": "freshness",
        "maxSize": 100,
        "maxAge": "1d",
        "timeout": "5s"
      }
    }
  ]
}
*/

// Service para interagir com SW
import { Injectable } from '@angular/core';
import { SwUpdate } from '@angular/service-worker';

@Injectable({
  providedIn: 'root'
})
export class PwaService {
  constructor(private swUpdate: SwUpdate) {
    if (swUpdate.isEnabled) {
      swUpdate.available.subscribe(() => {
        if (confirm('Nova versão disponível. Atualizar?')) {
          window.location.reload();
        }
      });
    }
  }

  checkForUpdate(): void {
    if (this.swUpdate.isEnabled) {
      this.swUpdate.checkForUpdate();
    }
  }
}
```

📺 **CONTEÚDOS PARA ASSISTIR**

- **YouTube**: "Service Workers Explained" - Google Chrome Developers
- **YouTube**: "Building PWAs with Angular" - Angular
- **Curso**: "Progressive Web Apps Training" - Google Developers

📚 **FONTES DE ESTUDO**

- [Angular University - Service Worker Guide](https://blog.angular-university.io/angular-service-worker/)
- [CodeZup - Practical Guide](https://codezup.com/angular-service-worker-a-practical-guide-to-caching-and-offline-support/)
- [Web.dev - Service Worker Caching](https://web.dev/articles/service-worker-caching-and-http-caching)

📝 **EXERCÍCIOS PRÁTICOS**

Exercício 1 - PWA Completa

Transforme uma aplicação em PWA:

- Service Worker customizado
- Cache de recursos e APIs
- Funcionalidade offline
- Installable app

Exercício 2 - Cache Inteligente

Implemente diferentes estratégias:

- Cache-first para CSS/JS
- Network-first para dados
- Background sync para formulários

---

## 6. OPFS (Origin Private File System)

📖 **CONCEITOS FUNDAMENTAIS**

OPFS é uma API moderna que permite aplicações web criar, ler e manipular arquivos em um sistema de arquivos virtual privado por origem.

Características:

- **Capacidade**: Limitada pelo espaço em disco disponível
- **Performance**: Acesso byte-a-byte otimizado
- **Privacidade**: Não visível ao usuário, privado por origem
- **Segurança**: Não requer permissões do usuário
- **Streaming**: Suporte a manipulação de arquivos grandes

Quando usar:

- Manipulação de arquivos grandes
- Bancos de dados personalizados
- Cache de alta performance
- Aplicações que precisam de I/O intensivo

🔧 **IMPLEMENTAÇÃO PRÁTICA**

Operações Básicas:

```javascript
// Verificar suporte
if ('storage' in navigator && 'getDirectory' in navigator.storage) {
  console.log('OPFS suportado');
} else {
  console.log('OPFS não suportado');
}

// Obter diretório raiz
async function obterDiretorioRaiz() {
  return await navigator.storage.getDirectory();
}

// Criar arquivo
async function criarArquivo(nome, conteudo) {
  const root = await navigator.storage.getDirectory();
  const fileHandle = await root.getFileHandle(nome, { create: true });
  const writable = await fileHandle.createWritable();
  await writable.write(conteudo);
  await writable.close();
  return fileHandle;
}

// Ler arquivo
async function lerArquivo(nome) {
  const root = await navigator.storage.getDirectory();
  const fileHandle = await root.getFileHandle(nome);
  const file = await fileHandle.getFile();
  return await file.text();
}

// Listar arquivos
async function listarArquivos() {
  const root = await navigator.storage.getDirectory();
  const arquivos = [];
  for await (const [nome, handle] of root.entries()) {
    arquivos.push({
      nome,
      tipo: handle.kind,
      handle
    });
  }
  return arquivos;
}

// Deletar arquivo
async function deletarArquivo(nome) {
  const root = await navigator.storage.getDirectory();
  await root.removeEntry(nome);
}
```

Manipulação Avançada com Streams:

```javascript
// Acesso síncrono (apenas em Web Workers)
async function accessoSincrono() {
  const root = await navigator.storage.getDirectory();
  const fileHandle = await root.getFileHandle('dados.db', { create: true });
  // Criar handle de acesso síncrono
  const syncHandle = await fileHandle.createSyncAccessHandle();
  // Escrever dados
  const encoder = new TextEncoder();
  const data = encoder.encode('Dados importantes');
  syncHandle.write(data, { at: 0 });
  // Ler dados
  const buffer = new ArrayBuffer(1024);
  const bytesRead = syncHandle.read(buffer, { at: 0 });
  const decoder = new TextDecoder();
  const texto = decoder.decode(buffer.slice(0, bytesRead));
  // Obter tamanho do arquivo
  const tamanho = syncHandle.getSize();
  // Truncar arquivo
  syncHandle.truncate(100);
  // Fechar handle
  syncHandle.close();
}

// Sistema de arquivos hierárquico
async function estruturaHierarquica() {
  const root = await navigator.storage.getDirectory();
  // Criar diretório
  const pastaDados = await root.getDirectoryHandle('dados', { create: true });
  const pastaImagens = await root.getDirectoryHandle('imagens', { create: true });
  // Criar arquivo em subdiretório
  const arquivo = await pastaDados.getFileHandle('config.json', { create: true });
  const writable = await arquivo.createWritable();
  await writable.write(JSON.stringify({
    versao: '1.0',
    configuracoes: {
      tema: 'dark',
      idioma: 'pt-BR'
    }
  }));
  await writable.close();
}
```

Service Angular para OPFS:

```typescript
import { Injectable } from '@angular/core';

export interface ArquivoOPFS {
  nome: string;
  tamanho: number;
  tipo: 'file' | 'directory';
  ultimaModificacao: Date;
}

@Injectable({
  providedIn: 'root'
})
export class OpfsService {
  private rootDirectoryHandle: FileSystemDirectoryHandle | null = null;

  async inicializar(): Promise<boolean> {
    if ('storage' in navigator && 'getDirectory' in navigator.storage) {
      this.rootDirectoryHandle = await navigator.storage.getDirectory();
      return true;
    }
    return false;
  }

  async salvarArquivo(caminho: string, conteudo: string | ArrayBuffer): Promise<void> {
    if (!this.rootDirectoryHandle) {
      throw new Error('OPFS não inicializado');
    }
    const fileHandle = await this.rootDirectoryHandle.getFileHandle(caminho, { create: true });
    const writable = await fileHandle.createWritable();
    await writable.write(conteudo);
    await writable.close();
  }

  async lerArquivo(caminho: string): Promise<string> {
    if (!this.rootDirectoryHandle) {
      throw new Error('OPFS não inicializado');
    }
    const fileHandle = await this.rootDirectoryHandle.getFileHandle(caminho);
    const file = await fileHandle.getFile();
    return await file.text();
  }

  async lerArquivoBinario(caminho: string): Promise<ArrayBuffer> {
    if (!this.rootDirectoryHandle) {
      throw new Error('OPFS não inicializado');
    }
    const fileHandle = await this.rootDirectoryHandle.getFileHandle(caminho);
    const file = await fileHandle.getFile();
    return await file.arrayBuffer();
  }

  async listarArquivos(diretorio: string = ''): Promise<ArquivoOPFS[]> {
    if (!this.rootDirectoryHandle) {
      throw new Error('OPFS não inicializado');
    }
    let currentDir = this.rootDirectoryHandle;
    if (diretorio) {
      currentDir = await this.rootDirectoryHandle.getDirectoryHandle(diretorio);
    }
    const arquivos: ArquivoOPFS[] = [];
    for await (const [nome, handle] of currentDir.entries()) {
      if (handle.kind === 'file') {
        const file = await (handle as FileSystemFileHandle).getFile();
        arquivos.push({
          nome,
          tamanho: file.size,
          tipo: 'file',
          ultimaModificacao: new Date(file.lastModified)
        });
      } else {
        arquivos.push({
          nome,
          tamanho: 0,
          tipo: 'directory',
          ultimaModificacao: new Date()
        });
      }
    }
    return arquivos;
  }

  async criarDiretorio(nome: string): Promise<void> {
    if (!this.rootDirectoryHandle) {
      throw new Error('OPFS não inicializado');
    }
    await this.rootDirectoryHandle.getDirectoryHandle(nome, { create: true });
  }

  async deletarArquivo(caminho: string): Promise<void> {
    if (!this.rootDirectoryHandle) {
      throw new Error('OPFS não inicializado');
    }
    await this.rootDirectoryHandle.removeEntry(caminho);
  }

  async obterTamanhoArquivo(caminho: string): Promise<number> {
    if (!this.rootDirectoryHandle) {
      throw new Error('OPFS não inicializado');
    }
    const fileHandle = await this.rootDirectoryHandle.getFileHandle(caminho);
    const file = await fileHandle.getFile();
    return file.size;
  }

  async arquivoExiste(caminho: string): Promise<boolean> {
    if (!this.rootDirectoryHandle) {
      return false;
    }
    try {
      await this.rootDirectoryHandle.getFileHandle(caminho);
      return true;
    } catch {
      return false;
    }
  }
}
```

Exemplo Prático - Editor de Arquivos:

```typescript
import { Component, OnInit } from '@angular/core';
import { OpfsService } from './opfs.service';

@Component({
  selector: 'app-editor-arquivos',
  template: `
    <div class="editor-container">
      <div class="sidebar">
        <h3>Arquivos</h3>
        <button (click)="criarNovoArquivo()">Novo Arquivo</button>
        <ul>
          <li *ngFor="let arquivo of arquivos"
              (click)="abrirArquivo(arquivo.nome)"
              [class.selected]="arquivoAtual === arquivo.nome">
            {{ arquivo.nome }} ({{ arquivo.tamanho }} bytes)
          </li>
        </ul>
      </div>
      <div class="editor">
        <div class="toolbar">
          <input [(ngModel)]="nomeArquivo" placeholder="Nome do arquivo">
          <button (click)="salvarArquivo()" [disabled]="!arquivoAtual">Salvar</button>
          <button (click)="deletarArquivo()" [disabled]="!arquivoAtual">Deletar</button>
        </div>
        <textarea
          [(ngModel)]="conteudoArquivo"
          placeholder="Digite seu conteúdo aqui..."
          rows="20"
          cols="80">
        </textarea>
      </div>
    </div>
  `
})
export class EditorArquivosComponent implements OnInit {
  arquivos: any[] = [];
  arquivoAtual: string = '';
  nomeArquivo: string = '';
  conteudoArquivo: string = '';

  constructor(private opfsService: OpfsService) {}

  async ngOnInit() {
    const suportado = await this.opfsService.inicializar();
    if (suportado) {
      await this.carregarListaArquivos();
    } else {
      alert('OPFS não é suportado neste navegador');
    }
  }

  async carregarListaArquivos() {
    this.arquivos = await this.opfsService.listarArquivos();
  }

  async abrirArquivo(nome: string) {
    try {
      this.conteudoArquivo = await this.opfsService.lerArquivo(nome);
      this.arquivoAtual = nome;
      this.nomeArquivo = nome;
    } catch (error) {
      console.error('Erro ao abrir arquivo:', error);
    }
  }

  async salvarArquivo() {
    if (!this.nomeArquivo || !this.conteudoArquivo) return;
    try {
      await this.opfsService.salvarArquivo(this.nomeArquivo, this.conteudoArquivo);
      this.arquivoAtual = this.nomeArquivo;
      await this.carregarListaArquivos();
    } catch (error) {
      console.error('Erro ao salvar arquivo:', error);
    }
  }

  async deletarArquivo() {
    if (!this.arquivoAtual) return;
    if (confirm(`Deletar arquivo ${this.arquivoAtual}?`)) {
      try {
        await this.opfsService.deletarArquivo(this.arquivoAtual);
        this.arquivoAtual = '';
        this.nomeArquivo = '';
        this.conteudoArquivo = '';
        await this.carregarListaArquivos();
      } catch (error) {
        console.error('Erro ao deletar arquivo:', error);
      }
    }
  }

  criarNovoArquivo() {
    this.arquivoAtual = '';
    this.nomeArquivo = 'novo-arquivo.txt';
    this.conteudoArquivo = '';
  }
}
```

📺 **CONTEÚDOS PARA ASSISTIR**

- **YouTube**: "Origin Private File System API" - Chrome for Developers
- **YouTube**: "File System Access API vs OPFS" - Web Platform News
- **Talk**: "The Future of File Storage on the Web" - Google I/O

📚 **FONTES DE ESTUDO**

- [Web.dev - Origin Private File System](https://web.dev/articles/origin-private-file-system-api)
- [MDN - File System API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API)
- [Chrome Platform Status - OPFS](https://chromestatus.com/feature/6284708426022912)

📝 **EXERCÍCIOS PRÁTICOS**

Exercício 1 - Sistema de Backup Local

Implemente um sistema que:

- Faça backup automático de dados da aplicação
- Mantenha múltiplas versões
- Permita restauração seletiva

Exercício 2 - Editor de Imagens Offline

Crie um editor que:

- Carregue e salve imagens localmente
- Mantenha histórico de edições
- Exporte em diferentes formatos

---

## 7. PROJETO FINAL

🎯 **ESPECIFICAÇÕES DO PROJETO**

Tema: Sistema de Gestão de Conteúdo Offline-First

Desenvolva uma aplicação Angular que demonstre o uso integrado de todas as tecnologias de storage estudadas:

Funcionalidades Obrigatórias:

1.  **Sistema de Usuários** (Cookies + localStorage)
    - Login/logout com cookies seguros
    - Preferências salvas em localStorage
    - Sessão com timeout
2.  **Gestão de Conteúdo** (IndexedDB + sessionStorage)
    - CRUD completo de artigos/posts
    - Rascunhos salvos automaticamente
    - Cache de dados da API
3.  **Funcionalidade Offline** (Service Workers + Cache API)
    - Aplicação funciona sem internet
    - Sincronização quando voltar online
    - Updates automáticos da aplicação
4.  **Sistema de Arquivos** (OPFS)
    - Upload e gestão de imagens
    - Export de dados
    - Backup local automático

Arquitetura Sugerida:

```
src/
├── app/
│   ├── core/
│   │   ├── services/
│   │   │   ├── storage/
│   │   │   │   ├── local-storage.service.ts
│   │   │   │   ├── session-storage.service.ts
│   │   │   │   ├── cookie.service.ts
│   │   │   │   ├── indexeddb.service.ts
│   │   │   │   ├── opfs.service.ts
│   │   │   │   └── cache.service.ts
│   │   │   ├── auth.service.ts
│   │   │   └── sync.service.ts
│   │   └── models/
│   ├── features/
│   │   ├── auth/
│   │   ├── content/
│   │   ├── files/
│   │   └── settings/
│   └── shared/
└── sw.js
```

Critérios de Avaliação:

- **Implementação Técnica (40%)**
  - Uso correto de cada tecnologia de storage
  - Tratamento de erros
  - Performance
- **Arquitetura (30%)**
  - Organização do código
  - Separação de responsabilidades
  - Reutilização
- **UX/UI (20%)**
  - Interface intuitiva
  - Feedback ao usuário
  - Responsividade
- **Documentação (10%)**
  - README completo
  - Comentários no código
  - Guia de instalação

📋 **ENTREGÁVEIS**

1.  **Código fonte** no GitHub
2.  **Demo online** (GitHub Pages/Netlify)
3.  **Documentação técnica**
4.  **Apresentação** (10 minutos)

⏰ **Cronograma**

- **Semana 1-2**: Configuração inicial e autenticação
- **Semana 3-4**: Gestão de conteúdo e IndexedDB
- **Semana 5-6**: Service Workers e funcionalidade offline
- **Semana 7-8**: OPFS e refinamentos finais

---

📚 **RECURSOS ADICIONAIS**

Trilha de Conteúdo Complementar:

Nível Iniciante:

- [NgGirls - Tutorial LocalStorage](https://ng-girls.gitbook.io/todo-list-tutorial-portuguese/local_storage)
- [TreinaWeb - Storage Comparison](https://www.treinaweb.com.br/blog/quando-usar-sessionstorage-e-localstorage)
- [Angular Training - Storage Guide](https://www.angulartraining.com/daily-newsletter/localstorage-and-sessionstorage/)

Nível Intermediário:

- [FreeCodeCamp - IndexedDB Guide](https://www.freecodecamp.org/news/a-quick-but-complete-guide-to-indexeddb-25f030425501/)
- [Dexie.js - Angular Tutorial](https://dexie.org/docs/Tutorial/Angular)
- [Telerik - IndexedDB for Beginners](https://www.telerik.com/blogs/beginners-guide-indexeddb)

Nível Avançado:

- [RxDB - Angular IndexedDB](https://rxdb.info/articles/angular-indexeddb.html)
- [Akita - State Management](https://opensource.salesforce.com/akita/docs/enhancers/persist-state/)
- [Auth0 - Akita Integration](https://auth0.com/blog/state-management-in-angular-with-akita-1/)

Service Workers & PWA:

- [Angular University - SW Guide](https://blog.angular-university.io/angular-service-worker/)
- [CodeZup - Practical SW Guide](https://codezup.com/angular-service-worker-a-practical-guide-to-caching-and-offline-support/)
- [Web.dev - SW Caching](https://web.dev/articles/service-worker-caching-and-http-caching)

OPFS & Modern APIs:

- [Web.dev - OPFS](https://web.dev/articles/origin-private-file-system-api)
- [Chrome Platform Status - OPFS](https://chromestatus.com/feature/6284708426022912)

Canais YouTube Recomendados:

- **Academind** - Angular tutorials
- **Programming with Mosh** - Web fundamentals
- **Traversy Media** - Full-stack development
- **Google Chrome Developers** - Web APIs
- **Angular** - Official channel

Comunidades e Fóruns:

- **Stack Overflow** - Dúvidas técnicas
- **Angular Discord** - Comunidade ativa
- **Reddit r/Angular2** - Discussões
- **Dev.to** - Artigos da comunidade

---

🏆 **CONCLUSÃO**

Esta apostila oferece uma base sólida para compreender e implementar todas as principais tecnologias de armazenamento web modernas. O domínio desses conceitos é essencial para desenvolver aplicações web robustas, performáticas e com excelente experiência do usuário.

Lembre-se: a escolha da tecnologia de storage adequada depende sempre do contexto específico do seu projeto. Use esta apostila como referência e pratique bastante com os exercícios propostos.

**Boa sorte nos estudos!** 🚀

[ref1]: Aspose.Words.782dd744-1cd8-4212-80ee-52fd88e4cf40.001.png
[ref2]: Aspose.Words.782dd744-1cd8-4212-80ee-52fd88e4cf40.002.png
[ref3]: Aspose.Words.782dd744-1cd8-4212-80ee-52fd88e4cf40.004.png