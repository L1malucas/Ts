# Projeto MongoDB E-commerce

Este documento contém todas as respostas e consultas para as tarefas do projeto de banco de dados e-commerce utilizando MongoDB.

[Documentação Oficial](https://www.mongodb.com/pt-br/docs/manual/introduction/)

[Curso Básico de Mongo - Legendado ](https://www.youtube.com/playlist?list=PL4cUxeGkcC9h77dJ-QJlwGlZlTd4ecZOA)

[Diferenças entre NoSQL e SQL](https://www.youtube.com/watch?v=04gyv76r_Ts&t=1873s&ab_channel=HashtagPrograma%C3%A7%C3%A3o)

---

## Introdução

Esta apostila contém uma série de tarefas práticas para consolidar os conhecimentos adquiridos durante o Workshop de MongoDB. As tarefas estão organizadas em ordem crescente de dificuldade, acompanhando a evolução do curso e culminando no desenvolvimento do projeto final de e-commerce.

---

## Tarefa 1: Configuração do Banco de Dados

### 1. Criação do banco de dados

```javascript
// Conectar ao MongoDB
mongo

// Criar e usar o banco de dados ecommerce_db
use ecommerce_db
```

### 2. Verificação da conexão

```javascript
// Usando mongosh
test> db.getMongo().getDBNames()

// Verifique se "ecommerce_db" está na lista

// Ou simplesmente
test> show dbs
```

---

## Tarefa 2: Criação da Estrutura de Dados

### 1. Criação das coleções

```javascript
use ecommerce_db

// Criar coleções
db.createCollection("produtos")
db.createCollection("categorias")
db.createCollection("clientes")
db.createCollection("pedidos")
db.createCollection("avaliacoes")

// Verificar coleções criadas
show collections
```

### 2. Inserção de categorias

```javascript
db.categorias.insertMany([
  { nome: "Eletrônicos", descricao: "Dispositivos e gadgets eletrônicos" },
  { nome: "Móveis", descricao: "Móveis para casa e escritório" },
  { nome: "Roupas", descricao: "Vestuário e acessórios" },
  { nome: "Livros", descricao: "Livros físicos e digitais" },
  { nome: "Esportes", descricao: "Artigos esportivos e equipamentos" }
])
```

---

## Tarefa 3: Operações CRUD Básicas

### 1. Inserção de produtos

```javascript
db.produtos.insertMany([
  {
    nome: "Smartphone Galaxy X20",
    descricao: "Smartphone com 128GB, 8GB RAM e câmera de 64MP",
    preco: 2499.99,
    estoque: 50,
    categoria: "Eletrônicos",
    tags: ["smartphone", "samsung", "android"]
  },
  {
    nome: "Notebook UltraSlim",
    descricao: "Notebook leve com processador i7, 16GB RAM e SSD 512GB",
    preco: 4299.99,
    estoque: 25,
    categoria: "Eletrônicos",
    tags: ["notebook", "computador", "trabalho"]
  },
  {
    nome: "Smart TV 55'",
    descricao: "TV LED 4K com recursos smart e HDR",
    preco: 3199.99,
    estoque: 30,
    categoria: "Eletrônicos",
    tags: ["tv", "4k", "smarttv"]
  },
  {
    nome: "Sofá Retrátil",
    descricao: "Sofá de 3 lugares com assento retrátil e tecido suede",
    preco: 1899.99,
    estoque: 10,
    categoria: "Móveis",
    tags: ["sofa", "sala", "conforto"]
  },
  {
    nome: "Mesa de Jantar",
    descricao: "Mesa de jantar 6 lugares em madeira maciça",
    preco: 1299.99,
    estoque: 15,
    categoria: "Móveis",
    tags: ["mesa", "jantar", "madeira"]
  },
  {
    nome: "Guarda-roupa Casal",
    descricao: "Guarda-roupa 6 portas com espelho e gavetas",
    preco: 1599.99,
    estoque: 12,
    categoria: "Móveis",
    tags: ["guarda-roupa", "quarto", "organizacao"]
  },
  {
    nome: "Camiseta Básica",
    descricao: "Camiseta 100% algodão em diversas cores",
    preco: 49.99,
    estoque: 200,
    categoria: "Roupas",
    tags: ["camiseta", "casual", "algodao"]
  },
  {
    nome: "Calça Jeans Slim",
    descricao: "Calça jeans com corte moderno e confortável",
    preco: 129.99,
    estoque: 150,
    categoria: "Roupas",
    tags: ["calca", "jeans", "casual"]
  },
  {
    nome: "Tênis Running",
    descricao: "Tênis para corrida com amortecimento e suporte",
    preco: 299.99,
    estoque: 80,
    categoria: "Esportes",
    tags: ["tenis", "corrida", "esporte"]
  },
  {
    nome: "Livro: O Senhor dos Anéis",
    descricao: "Trilogia completa em capa dura",
    preco: 149.99,
    estoque: 30,
    categoria: "Livros",
    tags: ["livro", "fantasia", "tolkien"]
  },
  {
    nome: "Bola de Futebol Oficial",
    descricao: "Bola de futebol profissional tamanho 5",
    preco: 89.99,
    estoque: 60,
    categoria: "Esportes",
    tags: ["futebol", "bola", "esporte"]
  },
  {
    nome: "Kit Halteres",
    descricao: "Kit com 2 halteres de 5kg cada",
    preco: 159.99,
    estoque: 40,
    categoria: "Esportes",
    tags: ["musculacao", "fitness", "treino"]
  },
  {
    nome: "Fone de Ouvido Bluetooth",
    descricao: "Fone sem fio com cancelamento de ruído",
    preco: 349.99,
    estoque: 45,
    categoria: "Eletrônicos",
    tags: ["fone", "audio", "bluetooth"]
  },
  {
    nome: "Vestido de Festa",
    descricao: "Vestido longo com detalhes em renda",
    preco: 259.99,
    estoque: 0,
    categoria: "Roupas",
    tags: ["vestido", "festa", "elegante"]
  },
  {
    nome: "Livro: Programação em MongoDB",
    descricao: "Guia completo de MongoDB para desenvolvedores",
    preco: 79.99,
    estoque: 20,
    categoria: "Livros",
    tags: ["livro", "programacao", "tecnologia"]
  }
])
```

### 2. Inserção de clientes

```javascript
db.clientes.insertMany([
  {
    nome: "Ana Silva",
    email: "ana.silva@email.com",
    telefone: "(11) 98765-4321",
    endereco: {
      rua: "Rua das Flores",
      numero: 123,
      bairro: "Jardim Primavera",
      cidade: "São Paulo",
      estado: "SP",
      cep: "01234-567"
    }
  },
  {
    nome: "Carlos Oliveira",
    email: "carlos.oliveira@email.com",
    telefone: "(21) 99876-5432",
    endereco: {
      rua: "Avenida Central",
      numero: 456,
      bairro: "Centro",
      cidade: "Rio de Janeiro",
      estado: "RJ",
      cep: "20000-123"
    }
  },
  {
    nome: "Mariana Costa",
    email: "mariana.costa@email.com",
    telefone: "(31) 97654-3210",
    endereco: {
      rua: "Rua dos Ipês",
      numero: 789,
      bairro: "Savassi",
      cidade: "Belo Horizonte",
      estado: "MG",
      cep: "30000-456"
    }
  },
  {
    nome: "Pedro Santos",
    email: "pedro.santos@email.com",
    telefone: "(41) 98765-1234",
    endereco: {
      rua: "Alameda dos Pinheiros",
      numero: 321,
      bairro: "Batel",
      cidade: "Curitiba",
      estado: "PR",
      cep: "80000-789"
    }
  },
  {
    nome: "Juliana Mendes",
    email: "juliana.mendes@email.com",
    telefone: "(51) 99876-2345",
    endereco: {
      rua: "Rua da Praia",
      numero: 654,
      bairro: "Moinhos de Vento",
      cidade: "Porto Alegre",
      estado: "RS",
      cep: "90000-123"
    }
  },
  {
    nome: "Rafael Lima",
    email: "rafael.lima@email.com",
    telefone: "(81) 98765-5678",
    endereco: {
      rua: "Avenida Boa Viagem",
      numero: 987,
      bairro: "Boa Viagem",
      cidade: "Recife",
      estado: "PE",
      cep: "50000-456"
    }
  },
  {
    nome: "Fernanda Moreira",
    email: "fernanda.moreira@email.com",
    telefone: "(71) 99876-7890",
    endereco: {
      rua: "Avenida Oceânica",
      numero: 159,
      bairro: "Barra",
      cidade: "Salvador",
      estado: "BA",
      cep: "40000-789"
    }
  },
  {
    nome: "Lucas Almeida",
    email: "lucas.almeida@email.com",
    telefone: "(61) 98765-8901",
    endereco: {
      rua: "SQN 305",
      numero: 405,
      apt: "202",
      bairro: "Asa Norte",
      cidade: "Brasília",
      estado: "DF",
      cep: "70000-123"
    }
  }
])
```

### 3. Atualização de estoque

```javascript
// Atualizar estoque do Smartphone
db.produtos.updateOne(
  { nome: "Smartphone Galaxy X20" },
  { $set: { estoque: 45 } }
)

// Atualizar estoque do Notebook
db.produtos.updateOne(
  { nome: "Notebook UltraSlim" },
  { $set: { estoque: 20 } }
)

// Atualizar estoque da Camiseta
db.produtos.updateOne(
  { nome: "Camiseta Básica" },
  { $set: { estoque: 180 } }
)
```

### 4. Remoção de produto com estoque zerado

```javascript
db.produtos.deleteOne({ estoque: 0 })
```

### 5. Listagem de produtos em ordem alfabética

```javascript
db.produtos.find().sort({ nome: 1 })
```

---

## Tarefa 4: Consultas Avançadas

### 1. Busca por faixa de preço

```javascript
// Produtos entre R$100 e R$500
db.produtos.find({
  preco: { $gte: 100, $lte: 500 }
})
```

### 2. Busca por palavra no nome usando regex

```javascript
// Produtos que contenham "Smart" no nome
db.produtos.find({
  nome: { $regex: "Smart", $options: "i" }
})
```

### 3. Busca por categoria e ordenação por preço

```javascript
// Produtos da categoria Eletrônicos ordenados por preço decrescente
db.produtos.find({ categoria: "Eletrônicos" }).sort({ preco: -1 })
```

### 4. Busca de clientes por cidade

```javascript
// Clientes que moram em São Paulo
db.clientes.find({ "endereco.cidade": "São Paulo" })
```

### 5. Busca por tags específicas

```javascript
// Produtos com a tag "esporte"
db.produtos.find({ tags: "esporte" })
```

### 6. Consulta dos 5 produtos mais caros

```javascript
db.produtos.find().sort({ preco: -1 }).limit(5)
```

---

## Tarefa 5: Relacionamentos e Documentos Complexos

### 1. Criação de pedidos

```javascript
// Função auxiliar para atualizar estoque
function atualizarEstoque(produtoId, quantidade) {
  db.produtos.updateOne(
    { _id: produtoId },
    { $inc: { estoque: -quantidade } }
  );
}

// Criar 10 pedidos

// Pedido 1
let pedido1 = {
  cliente: db.clientes.findOne({ nome: "Ana Silva" })._id,
  data: new Date("2024-03-01T10:30:00Z"),
  status: "Entregue",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Smartphone Galaxy X20" })._id,
      nome: "Smartphone Galaxy X20",
      preco: 2499.99,
      quantidade: 1
    },
    {
      produto: db.produtos.findOne({ nome: "Fone de Ouvido Bluetooth" })._id,
      nome: "Fone de Ouvido Bluetooth",
      preco: 349.99,
      quantidade: 1
    }
  ],
  valorTotal: 2849.98
};

db.pedidos.insertOne(pedido1);
atualizarEstoque(pedido1.itens[0].produto, pedido1.itens[0].quantidade);
atualizarEstoque(pedido1.itens[1].produto, pedido1.itens[1].quantidade);

// Pedido 2
let pedido2 = {
  cliente: db.clientes.findOne({ nome: "Carlos Oliveira" })._id,
  data: new Date("2024-03-05T14:15:00Z"),
  status: "Entregue",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Smart TV 55'" })._id,
      nome: "Smart TV 55'",
      preco: 3199.99,
      quantidade: 1
    }
  ],
  valorTotal: 3199.99
};

db.pedidos.insertOne(pedido2);
atualizarEstoque(pedido2.itens[0].produto, pedido2.itens[0].quantidade);

// Pedido 3
let pedido3 = {
  cliente: db.clientes.findOne({ nome: "Mariana Costa" })._id,
  data: new Date("2024-03-10T09:45:00Z"),
  status: "Entregue",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Livro: O Senhor dos Anéis" })._id,
      nome: "Livro: O Senhor dos Anéis",
      preco: 149.99,
      quantidade: 1
    },
    {
      produto: db.produtos.findOne({ nome: "Livro: Programação em MongoDB" })._id,
      nome: "Livro: Programação em MongoDB",
      preco: 79.99,
      quantidade: 1
    }
  ],
  valorTotal: 229.98
};

db.pedidos.insertOne(pedido3);
atualizarEstoque(pedido3.itens[0].produto, pedido3.itens[0].quantidade);
atualizarEstoque(pedido3.itens[1].produto, pedido3.itens[1].quantidade);

// Pedido 4
let pedido4 = {
  cliente: db.clientes.findOne({ nome: "Pedro Santos" })._id,
  data: new Date("2024-03-12T16:20:00Z"),
  status: "Em transporte",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Sofá Retrátil" })._id,
      nome: "Sofá Retrátil",
      preco: 1899.99,
      quantidade: 1
    },
    {
      produto: db.produtos.findOne({ nome: "Mesa de Jantar" })._id,
      nome: "Mesa de Jantar",
      preco: 1299.99,
      quantidade: 1
    }
  ],
  valorTotal: 3199.98
};

db.pedidos.insertOne(pedido4);
atualizarEstoque(pedido4.itens[0].produto, pedido4.itens[0].quantidade);
atualizarEstoque(pedido4.itens[1].produto, pedido4.itens[1].quantidade);

// Pedido 5
let pedido5 = {
  cliente: db.clientes.findOne({ nome: "Juliana Mendes" })._id,
  data: new Date("2024-03-15T11:30:00Z"),
  status: "Em processamento",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Camiseta Básica" })._id,
      nome: "Camiseta Básica",
      preco: 49.99,
      quantidade: 3
    },
    {
      produto: db.produtos.findOne({ nome: "Calça Jeans Slim" })._id,
      nome: "Calça Jeans Slim",
      preco: 129.99,
      quantidade: 2
    }
  ],
  valorTotal: 409.95
};

db.pedidos.insertOne(pedido5);
atualizarEstoque(pedido5.itens[0].produto, pedido5.itens[0].quantidade);
atualizarEstoque(pedido5.itens[1].produto, pedido5.itens[1].quantidade);

// Pedido 6
let pedido6 = {
  cliente: db.clientes.findOne({ nome: "Rafael Lima" })._id,
  data: new Date("2024-03-18T13:45:00Z"),
  status: "Entregue",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Tênis Running" })._id,
      nome: "Tênis Running",
      preco: 299.99,
      quantidade: 1
    },
    {
      produto: db.produtos.findOne({ nome: "Bola de Futebol Oficial" })._id,
      nome: "Bola de Futebol Oficial",
      preco: 89.99,
      quantidade: 1
    },
    {
      produto: db.produtos.findOne({ nome: "Kit Halteres" })._id,
      nome: "Kit Halteres",
      preco: 159.99,
      quantidade: 1
    }
  ],
  valorTotal: 549.97
};

db.pedidos.insertOne(pedido6);
atualizarEstoque(pedido6.itens[0].produto, pedido6.itens[0].quantidade);
atualizarEstoque(pedido6.itens[1].produto, pedido6.itens[1].quantidade);
atualizarEstoque(pedido6.itens[2].produto, pedido6.itens[2].quantidade);

// Pedido 7
let pedido7 = {
  cliente: db.clientes.findOne({ nome: "Fernanda Moreira" })._id,
  data: new Date("2024-03-20T10:15:00Z"),
  status: "Entregue",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Notebook UltraSlim" })._id,
      nome: "Notebook UltraSlim",
      preco: 4299.99,
      quantidade: 1
    },
    {
      produto: db.produtos.findOne({ nome: "Fone de Ouvido Bluetooth" })._id,
      nome: "Fone de Ouvido Bluetooth",
      preco: 349.99,
      quantidade: 1
    }
  ],
  valorTotal: 4649.98
};

db.pedidos.insertOne(pedido7);
atualizarEstoque(pedido7.itens[0].produto, pedido7.itens[0].quantidade);
atualizarEstoque(pedido7.itens[1].produto, pedido7.itens[1].quantidade);

// Pedido 8
let pedido8 = {
  cliente: db.clientes.findOne({ nome: "Lucas Almeida" })._id,
  data: new Date("2024-03-22T15:30:00Z"),
  status: "Em transporte",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Guarda-roupa Casal" })._id,
      nome: "Guarda-roupa Casal",
      preco: 1599.99,
      quantidade: 1
    }
  ],
  valorTotal: 1599.99
};

db.pedidos.insertOne(pedido8);
atualizarEstoque(pedido8.itens[0].produto, pedido8.itens[0].quantidade);

// Pedido 9
let pedido9 = {
  cliente: db.clientes.findOne({ nome: "Ana Silva" })._id,
  data: new Date("2024-03-25T09:00:00Z"),
  status: "Em processamento",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Livro: Programação em MongoDB" })._id,
      nome: "Livro: Programação em MongoDB",
      preco: 79.99,
      quantidade: 1
    }
  ],
  valorTotal: 79.99
};

db.pedidos.insertOne(pedido9);
atualizarEstoque(pedido9.itens[0].produto, pedido9.itens[0].quantidade);

// Pedido 10
let pedido10 = {
  cliente: db.clientes.findOne({ nome: "Carlos Oliveira" })._id,
  data: new Date("2024-03-28T14:00:00Z"),
  status: "Aguardando pagamento",
  itens: [
    {
      produto: db.produtos.findOne({ nome: "Camiseta Básica" })._id,
      nome: "Camiseta Básica",
      preco: 49.99,
      quantidade: 2
    },
    {
      produto: db.produtos.findOne({ nome: "Calça Jeans Slim" })._id,
      nome: "Calça Jeans Slim",
      preco: 129.99,
      quantidade: 1
    }
  ],
  valorTotal: 229.97
};

db.pedidos.insertOne(pedido10);
atualizarEstoque(pedido10.itens[0].produto, pedido10.itens[0].quantidade);
atualizarEstoque(pedido10.itens[1].produto, pedido10.itens[1].quantidade);
```

### 2. Criação de avaliações

```javascript
// Avaliações para o pedido 1
db.avaliacoes.insertMany([
  {
    produto: db.produtos.findOne({ nome: "Smartphone Galaxy X20" })._id,
    cliente: db.clientes.findOne({ nome: "Ana Silva" })._id,
    pedido: db.pedidos.findOne({
      cliente: db.clientes.findOne({ nome: "Ana Silva" })._id,
      "itens.nome": "Smartphone Galaxy X20"
    })._id,
    nota: 5,
    comentario: "Excelente smartphone, câmera incrível e bateria de longa duração.",
    data: new Date("2024-03-05T14:30:00Z")
  },
  {
    produto: db.produtos.findOne({ nome: "Fone de Ouvido Bluetooth" })._id,
    cliente: db.clientes.findOne({ nome: "Ana Silva" })._id,
    pedido: db.pedidos.findOne({
      cliente: db.clientes.findOne({ nome: "Ana Silva" })._id,
      "itens.nome": "Fone de Ouvido Bluetooth"
    })._id,
    nota: 4,
    comentario: "Ótimo fone, mas a bateria poderia durar mais.",
    data: new Date("2024-03-05T14:35:00Z")
  }
]);

// Avaliações para o pedido 2
db.avaliacoes.insertMany([
  {
    produto: db.produtos.findOne({ nome: "Smart TV 55'" })._id,
    cliente: db.clientes.findOne({ nome: "Carlos Oliveira" })._id,
    pedido: db.pedidos.findOne({
      cliente: db.clientes.findOne({ nome: "Carlos Oliveira" })._id,
      "itens.nome": "Smart TV 55'"
    })._id,
    nota: 5,
    comentario: "TV com imagem perfeita e interface muito intuitiva.",
    data: new Date("2024-03-10T09:15:00Z")
  },
  {
    produto: db.produtos.findOne({ nome: "Smart TV 55'" })._id,
    cliente: db.clientes.findOne({ nome: "Carlos Oliveira" })._id,
    pedido: db.pedidos.findOne({
      cliente: db.clientes.findOne({ nome: "Carlos Oliveira" })._id,
      "itens.nome": "Smart TV 55'"
    })._id,
    nota: 5,
    comentario: "Chegou antes do prazo previsto e muito bem embalada.",
    data: new Date("2024-03-10T09:20:00Z")
  }
]);

// E assim por diante para os demais pedidos...

// Continuando com avaliações para pedidos 3 a 10...
```

### 3. Busca de pedidos de cliente específico

```javascript
// Buscar pedidos de Ana Silva com detalhes dos produtos
db.pedidos.aggregate([
  { $match: { cliente: db.clientes.findOne({ nome: "Ana Silva" })._id } },
  {
    $lookup: {
      from: "produtos",
      localField: "itens.produto",
      foreignField: "_id",
      as: "detalhes_produtos"
    }
  },
  {
    $project: {
      _id: 1,
      data: 1,
      status: 1,
      valorTotal: 1,
      itens: 1,
      "detalhes_produtos.nome": 1,
      "detalhes_produtos.descricao": 1,
      "detalhes_produtos.categoria": 1
    }
  }
])
```

---

## Tarefa 6: Índices e Otimização

### 1. Criação de índices

```javascript
// Índice para busca de produtos por nome
db.produtos.createIndex({ nome: 1 })

// Índice para busca de produtos por categoria e preço
db.produtos.createIndex({ categoria: 1, preco: 1 })

// Índice único para email de clientes
db.clientes.createIndex({ email: 1 }, { unique: true })

// Índice para busca de pedidos por cliente e data
db.pedidos.createIndex({ cliente: 1, data: 1 })
```

### 2. Análise de desempenho com explain()

```javascript
// Sem índice vs com índice
db.produtos.find({ nome: "Smartphone Galaxy X20" }).explain("executionStats")

// Com índice categoria-preço
db.produtos.find({
  categoria: "Eletrônicos",
  preco: { $gt: 1000 }
}).explain("executionStats")
```

### 3. Criação de índice de texto

```javascript
// Criar índice de texto para nome e descrição de produtos
db.produtos.createIndex(
  { nome: "text", descricao: "text" },
  { weights: { nome: 2, descricao: 1 } }
)

// Testar busca por texto
db.produtos.find({ $text: { $search: "smartphone android" } })
```

---

## Tarefa 7: Agregações Básicas (continuação)

### 5. Relatório de produtos mais vendidos

```javascript
db.pedidos.aggregate([
  { $unwind: "$itens" },
  {
    $group: {
      _id: "$itens.produto",
      totalVendido: { $sum: "$itens.quantidade" },
      valorTotal: { $sum: { $multiply: ["$itens.preco", "$itens.quantidade"] } }
    }
  },
  {
    $lookup: {
      from: "produtos",
      localField: "_id",
      foreignField: "_id",
      as: "produto_info"
    }
  },
  {
    $project: {
      _id: 0,
      produto: { $arrayElemAt: ["$produto_info.nome", 0] },
      categoria: { $arrayElemAt: ["$produto_info.categoria", 0] },
      totalVendido: 1,
      valorTotal: 1
    }
  },
  { $sort: { totalVendido: -1 } }
])
```

---

## Tarefa 8: Agregações Avançadas

### 1. Distribuição de notas nas avaliações por categoria

```javascript
db.avaliacoes.aggregate([
  {
    $lookup: {
      from: "produtos",
      localField: "produto",
      foreignField: "_id",
      as: "produto_info"
    }
  },
  {
    $project: {
      _id: 0,
      nota: 1,
      categoria: { $arrayElemAt: ["$produto_info.categoria", 0] }
    }
  },
  {
    $group: {
      _id: {
        categoria: "$categoria",
        nota: "$nota"
      },
      quantidade: { $sum: 1 }
    }
  },
  {
    $group: {
      _id: "$_id.categoria",
      distribuicao: {
        $push: {
          nota: "$_id.nota",
          quantidade: "$quantidade"
        }
      },
      mediaNotas: { $avg: "$_id.nota" }
    }
  },
  {
    $project: {
      _id: 0,
      categoria: "$_id",
      distribuicao: 1,
      mediaNotas: { $round: ["$mediaNotas", 1] }
    }
  },
  { $sort: { categoria: 1 } }
])
```

### 2. Relatório de vendas por dia da semana e hora do dia

```javascript
db.pedidos.aggregate([
  {
    $project: {
      _id: 0,
      valorTotal: 1,
      diaSemana: { $dayOfWeek: "$data" }, // 1=domingo, 2=segunda, etc.
      hora: { $hour: "$data" }
    }
  },
  {
    $group: {
      _id: {
        diaSemana: "$diaSemana",
        hora: "$hora"
      },
      totalVendas: { $sum: "$valorTotal" },
      quantidadePedidos: { $sum: 1 }
    }
  },
  {
    $project: {
      _id: 0,
      diaSemana: {
        $switch: {
          branches: [
            { case: { $eq: ["$_id.diaSemana", 1] }, then: "Domingo" },
            { case: { $eq: ["$_id.diaSemana", 2] }, then: "Segunda-feira" },
            { case: { $eq: ["$_id.diaSemana", 3] }, then: "Terça-feira" },
            { case: { $eq: ["$_id.diaSemana", 4] }, then: "Quarta-feira" },
            { case: { $eq: ["$_id.diaSemana", 5] }, then: "Quinta-feira" },
            { case: { $eq: ["$_id.diaSemana", 6] }, then: "Sexta-feira" },
            { case: { $eq: ["$_id.diaSemana", 7] }, then: "Sábado" }
          ],
          default: "Desconhecido"
        }
      },
      hora: "$_id.hora",
      totalVendas: 1,
      quantidadePedidos: 1
    }
  },
  { $sort: { diaSemana: 1, hora: 1 } }
])
```

### 3. Tempo médio entre a criação do pedido e sua entrega

```javascript
// Primeiro, vamos adicionar uma data de entrega para os pedidos entregues
// (Na prática, isso seria feito ao atualizar o status do pedido)

// Atualizar os pedidos com status "Entregue" para adicionar data de entrega
db.pedidos.updateMany(
  { status: "Entregue" },
  [
    {
      $set: {
        dataEntrega: {
          $dateAdd: {
            startDate: "$data",
            unit: "day",
            amount: { $floor: { $multiply: [{ $random: {} }, 5] } } // 0-4 dias após o pedido
          }
        }
      }
    }
  ]
);

// Agora calcular o tempo médio de entrega
db.pedidos.aggregate([
  { $match: { status: "Entregue", dataEntrega: { $exists: true } } },
  {
    $project: {
      _id: 0,
      cliente: 1,
      tempoEntregaHoras: {
        $divide: [
          { $subtract: ["$dataEntrega", "$data"] },
          3600000 // Converter de milissegundos para horas
        ]
      }
    }
  },
  {
    $group: {
      _id: null,
      tempoMedioHoras: { $avg: "$tempoEntregaHoras" },
      pedidoMaisRapido: { $min: "$tempoEntregaHoras" },
      pedidoMaisLento: { $max: "$tempoEntregaHoras" }
    }
  },
  {
    $project: {
      _id: 0,
      tempoMedioDias: { $round: [{ $divide: ["$tempoMedioHoras", 24] }, 1] },
      tempoMedioHoras: { $round: ["$tempoMedioHoras", 1] },
      pedidoMaisRapidoHoras: { $round: ["$pedidoMaisRapido", 1] },
      pedidoMaisLentoHoras: { $round: ["$pedidoMaisLento", 1] }
    }
  }
])
```

### 4. Produtos frequentemente comprados juntos

```javascript
db.pedidos.aggregate([
  // Filtra apenas pedidos com mais de um item
  { $match: { $expr: { $gt: [{ $size: "$itens" }, 1] } } },
  // Desmembra os itens do pedido
  { $unwind: "$itens" },
  // Agrupa por produto e guarda os outros produtos no mesmo pedido
  {
    $group: {
      _id: "$itens.produto",
      outrosProdutos: {
        $push: {
          $filter: {
            input: "$itens",
            as: "item",
            cond: { $ne: ["$$item.produto", "$itens.produto"] }
          }
        }
      },
      nomeProduto: { $first: "$itens.nome" }
    }
  },
  // Desmembra os arrays de outros produtos
  { $unwind: "$outrosProdutos" },
  { $unwind: "$outrosProdutos" },
  // Agrupa para contar a frequência de cada par
  {
    $group: {
      _id: {
        produto1: "$_id",
        produto2: "$outrosProdutos.produto"
      },
      produto1Nome: { $first: "$nomeProduto" },
      produto2Nome: { $first: "$outrosProdutos.nome" },
      frequencia: { $sum: 1 }
    }
  },
  // Filtra para eliminar duplicações (A,B e B,A)
  {
    $match: {
      $expr: {
        $gt: ["$_id.produto1", "$_id.produto2"]
      }
    }
  },
  // Ordena por frequência
  { $sort: { frequencia: -1 } },
  // Projeta o resultado final
  {
    $project: {
      _id: 0,
      produto1: "$produto1Nome",
      produto2: "$produto2Nome",
      frequencia: 1
    }
  }
])
```

### 5. Dashboard com indicadores de desempenho

```javascript
db.pedidos.aggregate([
  // Estágio 1: Calcular vendas totais
  {
    $facet: {
      // Total de vendas geral
      "vendasTotais": [
        {
          $group: {
            _id: null,
            valor: { $sum: "$valorTotal" },
            quantidade: { $sum: 1 }
          }
        }
      ],
      // Vendas por mês
      "vendasPorMes": [
        {
          $group: {
            _id: {
              ano: { $year: "$data" },
              mes: { $month: "$data" }
            },
            valor: { $sum: "$valorTotal" },
            quantidade: { $sum: 1 }
          }
        },
        { $sort: { "_id.ano": 1, "_id.mes": 1 } }
      ],
      // Ticket médio
      "ticketMedio": [
        {
          $group: {
            _id: null,
            valor: { $avg: "$valorTotal" }
          }
        }
      ],
      // Distribuição por status
      "statusPedidos": [
        {
          $group: {
            _id: "$status",
            quantidade: { $sum: 1 },
            valorTotal: { $sum: "$valorTotal" }
          }
        },
        { $sort: { quantidade: -1 } }
      ],
      // Produtos mais vendidos
      "produtosMaisVendidos": [
        { $unwind: "$itens" },
        {
          $group: {
            _id: "$itens.produto",
            nome: { $first: "$itens.nome" },
            quantidade: { $sum: "$itens.quantidade" },
            valorTotal: { $sum: { $multiply: ["$itens.preco", "$itens.quantidade"] } }
          }
        },
        { $sort: { quantidade: -1 } },
        { $limit: 5 }
      ]
    }
  },
  // Estágio 2: Formatar resultados
  {
    $project: {
      metricas: {
        vendasTotais: { $arrayElemAt: ["$vendasTotais.valor", 0] },
        quantidadePedidos: { $arrayElemAt: ["$vendasTotais.quantidade", 0] },
        ticketMedio: { $round: [{ $arrayElemAt: ["$ticketMedio.valor", 0] }, 2] }
      },
      vendasPorMes: "$vendasPorMes",
      statusPedidos: "$statusPedidos",
      produtosMaisVendidos: "$produtosMaisVendidos"
    }
  }
])
```

---

## Tarefa 9: Exportação e Backup

### 1. Exportação da coleção de produtos para CSV

```bash
# Usando o mongoexport
mongoexport --db ecommerce_db --collection produtos --type=csv --fields="_id,nome,descricao,preco,estoque,categoria,tags" --out=produtos.csv

# Com autenticação
mongoexport --db=ecommerce_db --collection=produtos --type=csv --fields=nome,categoria,preco,estoque --out=produtos.csv --username usuario --password senha --authenticationDatabase admin
```

### 2. Script de backup automático diário

**`backup_script.sh`**

```bash
#!/bin/bash

# Configurações
MONGO_HOST="localhost"
MONGO_PORT="27017"
DB_NAME="ecommerce_db"
BACKUP_DIR="/backups/mongodb"
DATE=$(date +"%Y%m%d")
BACKUP_NAME="backup_${DB_NAME}_${DATE}"

# Criar diretório de backup se não existir
mkdir -p $BACKUP_DIR

# Criar backup usando mongodump
mongodump --host $MONGO_HOST --port $MONGO_PORT --db $DB_NAME --out $BACKUP_DIR/$BACKUP_NAME

# Comprimir o backup
cd $BACKUP_DIR
tar -czf ${BACKUP_NAME}.tar.gz $BACKUP_NAME

# Remover diretório descompactado
rm -rf $BACKUP_NAME

# Manter apenas os últimos 7 backups
ls -tp $BACKUP_DIR/*.tar.gz | grep -v '/$' | tail -n +8 | xargs -I {} rm -- {}

echo "Backup concluído: $BACKUP_DIR/${BACKUP_NAME}.tar.gz"
```

**Configuração do cron para executar diariamente**

- Adicionar no crontab (`crontab -e`)

```bash
0 2 * * * /caminho/para/backup_script.sh >> /var/log/mongodb_backup.log 2>&1
```

### 3. Restauração de backup em nova instância

```bash
# Descomprimir o backup
tar -xzf backup_ecommerce_db_20240330.tar.gz -C /tmp/

# Restaurar usando mongorestore
mongorestore --host nova-instancia --port 27017 --db ecommerce_db_novo /tmp/backup_ecommerce_db
```

### 4. Script para exportar relatórios de vendas em PDF

**Usando Node.js e PDFKit**

Primeiro, instalar dependências:

```bash
npm install mongodb pdfkit moment fs
```

**`report_generator.js`**

```javascript
const { MongoClient } = require('mongodb');
const PDFDocument = require('pdfkit');
const fs = require('fs');
const moment = require('moment');

// Configurações MongoDB
const uri = 'mongodb://localhost:27017';
const client = new MongoClient(uri);
const dbName = 'ecommerce_db';

async function generateSalesReport() {
  try {
    await client.connect();
    console.log('Conectado ao MongoDB');

    const db = client.db(dbName);
    const pedidosCollection = db.collection('pedidos');

    // Data do relatório
    const today = moment().format('YYYY-MM-DD');
    const fileName = `relatorio_vendas_${today}.pdf`;

    // Criar documento PDF
    const doc = new PDFDocument();
    doc.pipe(fs.createWriteStream(fileName));

    // Título e cabeçalho
    doc.fontSize(20).text('Relatório de Vendas', { align: 'center' });
    doc.fontSize(12).text(`Gerado em: ${moment().format('DD/MM/YYYY HH:mm')}`, { align: 'center' });
    doc.moveDown(2);

    // Resumo de vendas
    const resumoVendas = await pedidosCollection.aggregate([
      {
        $group: {
          _id: null,
          totalVendas: { $sum: '$valorTotal' },
          quantidadePedidos: { $sum: 1 },
          ticketMedio: { $avg: '$valorTotal' }
        }
      }
    ]).toArray();

    if (resumoVendas.length > 0) {
      const resumo = resumoVendas[0];
      doc.fontSize(14).text('Resumo de Vendas');
      doc.fontSize(10);
      doc.text(`Total de Vendas: R$ ${resumo.totalVendas.toFixed(2)}`);
      doc.text(`Quantidade de Pedidos: ${resumo.quantidadePedidos}`);
      doc.text(`Ticket Médio: R$ ${resumo.ticketMedio.toFixed(2)}`);
      doc.moveDown(1);
    }

    // Vendas por mês
    doc.fontSize(14).text('Vendas por Mês');
    doc.moveDown(0.5);

    const vendasPorMes = await pedidosCollection.aggregate([
      {
        $group: {
          _id: {
            ano: { $year: '$data' },
            mes: { $month: '$data' }
          },
          totalVendas: { $sum: '$valorTotal' },
          quantidadePedidos: { $sum: 1 }
        }
      },
      { $sort: { '_id.ano': 1, '_id.mes': 1 } }
    ]).toArray();

    // Tabela de vendas por mês
    doc.fontSize(10);
    doc.text('Período', 50, doc.y, { width: 100 });
    doc.text('Total Vendas', 150, doc.y - 10, { width: 150 });
    doc.text('Quantidade', 300, doc.y - 10, { width: 100 });
    doc.moveDown(0.5);

    vendasPorMes.forEach(item => {
      const periodo = `${item._id.ano}/${item._id.mes.toString().padStart(2, '0')}`;
      doc.text(periodo, 50, doc.y, { width: 100 });
      doc.text(`R$ ${item.totalVendas.toFixed(2)}`, 150, doc.y - 10, { width: 150 });
      doc.text(`${item.quantidadePedidos}`, 300, doc.y - 10, { width: 100 });
      doc.moveDown(0.5);
    });

    doc.moveDown(1);

    // Produtos mais vendidos
    doc.fontSize(14).text('Produtos Mais Vendidos');
    doc.moveDown(0.5);

    const produtosMaisVendidos = await pedidosCollection.aggregate([
      { $unwind: '$itens' },
      {
        $group: {
          _id: '$itens.produto',
          nome: { $first: '$itens.nome' },
          quantidade: { $sum: '$itens.quantidade' },
          valorTotal: { $sum: { $multiply: ['$itens.preco', '$itens.quantidade'] } }
        }
      },
      { $sort: { quantidade: -1 } },
      { $limit: 10 }
    ]).toArray();

    // Tabela de produtos mais vendidos
    doc.fontSize(10);
    doc.text('Produto', 50, doc.y, { width: 200 });
    doc.text('Quantidade', 250, doc.y - 10, { width: 100 });
    doc.text('Valor Total', 350, doc.y - 10, { width: 100 });
    doc.moveDown(0.5);

    produtosMaisVendidos.forEach(item => {
      doc.text(item.nome, 50, doc.y, { width: 200 });
      doc.text(`${item.quantidade}`, 250, doc.y - 10, { width: 100 });
      doc.text(`R$ ${item.valorTotal.toFixed(2)}`, 350, doc.y - 10, { width: 100 });
      doc.moveDown(0.5);
    });

    // Finalizar documento
    doc.end();
    console.log(`Relatório de vendas gerado: ${fileName}`);
  } finally {
    await client.close();
  }
}

// Executar geração do relatório
generateSalesReport().catch(console.error);
```

### 5. Sistema de logs para operações importantes

**Implementação do sistema de logs usando MongoDB Change Streams**

`logger.js`

```javascript
const { MongoClient } = require('mongodb');
const fs = require('fs');
const moment = require('moment');

// Configurações MongoDB
const uri = 'mongodb://localhost:27017';
const client = new MongoClient(uri);
const dbName = 'ecommerce_db';

async function startLogger() {
  try {
    await client.connect();
    console.log('Logger conectado ao MongoDB');

    const db = client.db(dbName);

    // Arquivo de log
    const logFile = fs.createWriteStream('mongodb_operations.log', { flags: 'a' });

    // Monitorar operações na coleção de produtos
    const produtosChangeStream = db.collection('produtos').watch();
    produtosChangeStream.on('change', (change) => {
      const timestamp = moment().format('YYYY-MM-DD HH:mm:ss');
      const operation = change.operationType;
      let details = '';

      if (operation === 'insert') {
        details = `Novo produto inserido: ${change.fullDocument.nome}`;
      } else if (operation === 'update') {
        details = `Produto atualizado: ID ${change.documentKey._id}`;
        if (change.updateDescription && change.updateDescription.updatedFields) {
          details += `, Campos: ${Object.keys(change.updateDescription.updatedFields).join(', ')}`;
        }
      } else if (operation === 'delete') {
        details = `Produto removido: ID ${change.documentKey._id}`;
      }
      const logMessage = `[${timestamp}] [PRODUTOS] ${operation.toUpperCase()}: ${details}\n`;
      logFile.write(logMessage);
      console.log(logMessage);
    });

    // Monitorar operações na coleção de pedidos
    const pedidosChangeStream = db.collection('pedidos').watch();
    pedidosChangeStream.on('change', (change) => {
      const timestamp = moment().format('YYYY-MM-DD HH:mm:ss');
      const operation = change.operationType;
      let details = '';

      if (operation === 'insert') {
        details = `Novo pedido registrado: Cliente ID ${change.fullDocument.cliente}, Valor: R$ ${change.fullDocument.valorTotal}`;
      } else if (operation === 'update') {
        details = `Pedido atualizado: ID ${change.documentKey._id}`;
        if (change.updateDescription && change.updateDescription.updatedFields) {
          if (change.updateDescription.updatedFields.status) {
            details += `, Novo status: ${change.updateDescription.updatedFields.status}`;
          }
        }
      } else if (operation === 'delete') {
        details = `Pedido removido: ID ${change.documentKey._id}`;
      }
      const logMessage = `[${timestamp}] [PEDIDOS] ${operation.toUpperCase()}: ${details}\n`;
      logFile.write(logMessage);
      console.log(logMessage);
    });

    console.log('Logger iniciado e monitorando operações do MongoDB');

    // Manter a conexão aberta
    process.on('SIGINT', async () => {
      console.log('Encerrando logger...');
      await client.close();
    });
  } catch (error) {
    console.error('Erro ao iniciar logger:', error);
  }
}

// Iniciar o logger
startLogger().catch(console.error);
```

---

## Tarefa 10: Operações com Docker

### 1. Crie um `docker-compose.yml` para MongoDB e Mongo Express

```yaml
version: '3'
services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: senha123
    volumes:
      - mongo_data:/data/db
    networks:
      - mongo_network
  mongo-express:
    image: mongo-express:latest
    container_name: mongo-express
    depends_on:
      - mongodb
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: senha123
      ME_CONFIG_MONGODB_SERVER: mongodb
      ME_CONFIG_BASICAUTH_USERNAME: admin
      ME_CONFIG_BASICAUTH_PASSWORD: senha123
    networks:
      - mongo_network
volumes:
  mongo_data:
networks:
  mongo_network:
    driver: bridge
```

### 2. Implemente um sistema de replicação com 3 instâncias MongoDB

```bash
docker run -d --name mongo1 -p 27017:27017 mongo --replSet rs0
docker run -d --name mongo2 -p 27018:27017 mongo --replSet rs0
docker run -d --name mongo3 -p 27019:27017 mongo --replSet rs0
```

- Configurar o conjunto de réplicas

```bash
docker exec -it mongo1 mongosh --eval "rs.initiate({
  _id: 'rs0',
  members: [
    { _id: 0, host: 'localhost:27017' },
    { _id: 1, host: 'localhost:27018' },
    { _id: 2, host: 'localhost:27019' }
  ]
})"
```

---

## Tarefa 11: Preparação do Projeto

### 1. Crie um diagrama de relacionamento entre as coleções

(Diagrama seria inserido aqui, se fosse um formato visual)

### 2. Configure índices otimizados para todos os casos de uso

(Já abordado na Tarefa 6)

### 3. Implemente validação de esquema para todas as coleções

(Exemplo de validação de esquema para a coleção `produtos`)

```javascript
db.createCollection("produtos", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nome", "preco", "estoque", "categoria"],
      properties: {
        nome: {
          bsonType: "string",
          description: "deve ser uma string e é obrigatório"
        },
        preco: {
          bsonType: "double",
          minimum: 0,
          description: "deve ser um double maior ou igual a 0 e é obrigatório"
        },
        estoque: {
          bsonType: "int",
          minimum: 0,
          description: "deve ser um inteiro maior ou igual a 0 e é obrigatório"
        },
        categoria: {
          bsonType: "string",
          description: "deve ser uma string e é obrigatório"
        },
        descricao: {
          bsonType: "string",
          description: "deve ser uma string"
        },
        tags: {
          bsonType: "array",
          items: {
            bsonType: "string"
          },
          description: "deve ser um array de strings"
        }
      }
    }
  }
})
```

---

## Tarefa 12: Relatórios e Analytics

### 1. Desenvolva relatórios de vendas por período

(Exemplo: Vendas diárias)

```javascript
db.pedidos.aggregate([
  {
    $group: {
      _id: { $dateToString: { format: "%Y-%m-%d", date: "$data" } },
      totalVendas: { $sum: "$valorTotal" },
      quantidadePedidos: { $sum: 1 }
    }
  },
  { $sort: { _id: 1 } }
])
```

### 2. Crie um sistema de recomendação baseado em compras anteriores

(Exemplo: Produtos comprados juntos)

```javascript
db.pedidos.aggregate([
  { $unwind: "$itens" },
  {
    $group: {
      _id: "$cliente",
      produtosComprados: { $addToSet: "$itens.produto" }
    }
  },
  { $unwind: "$produtosComprados" },
  {
    $group: {
      _id: "$produtosComprados",
      clientes: { $addToSet: "$_id" }
    }
  },
  {
    $project: {
      _id: 0,
      produtoId: "$_id",
      clientesQueCompraram: "$clientes",
      count: { $size: "$clientes" }
    }
  },
  { $sort: { count: -1 } }
])
```

### 3. Implemente dashboards de métricas para administradores

(Já abordado na Tarefa 8, item 5)

### 4. Crie um sistema de alertas para produtos com estoque baixo

```javascript
db.produtos.find({
  estoque: { $lt: 10 }
})
```

### 5. Desenvolva relatórios de comportamento de usuários

(Exemplo: Clientes que mais gastaram)

```javascript
db.pedidos.aggregate([
  {
    $group: {
      _id: "$cliente",
      totalGasto: { $sum: "$valorTotal" },
      totalPedidos: { $sum: 1 }
    }
  },
  {
    $lookup: {
      from: "clientes",
      localField: "_id",
      foreignField: "_id",
      as: "clienteInfo"
    }
  },
  { $unwind: "$clienteInfo" },
  {
    $project: {
      _id: 0,
      nomeCliente: "$clienteInfo.nome",
      emailCliente: "$clienteInfo.email",
      totalGasto: 1,
      totalPedidos: 1
    }
  },
  { $sort: { totalGasto: -1 } },
  { $limit: 5 }
])
```

---

## Tarefa 13: Entrega e Documentação

### 1. Documente todas as funcionalidades implementadas

(Este documento serve como parte da documentação)

### 2. Crie guias de uso para administradores e clientes

(Exemplo de guia para administradores: Como verificar o status do MongoDB)

```bash
sudo systemctl status mongod
```