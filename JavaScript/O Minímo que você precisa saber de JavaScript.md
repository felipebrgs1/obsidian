Esta nota reúne os conceitos essenciais para iniciar em JavaScript. Aqui você encontrará o necessário para entender variáveis, funções (inclusive arrow functions), template literals, destructuring, módulos, e muito mais. Este material é perfeito para colar no Obsidian e consultar sempre que precisar.

---

## 1. Variáveis: `var`, `let` e `const`
### `var`
- **Escopo:** Função ou global (não respeita escopo de bloco)
- **Redeclaração:** Permitida
- **Reatribuição:** Permitida

```js
var x = 10;
console.log(x); // 10

if (true) {
  var x = 20;
  console.log(x); // 20 (mesma variável x)
}
console.log(x); // 20
```

### `let`

- **Escopo:** Bloco, função ou global
- **Redeclaração:** Não permitida no mesmo escopo
- **Reatribuição:** Permitida

```js
let y = 10;
console.log(y); // 10

if (true) {
  let y = 20; // y deste bloco é diferente
  console.log(y); // 20
}
console.log(y); // 10 (valor original)
```

### `const`

- **Escopo:** Bloco, função ou global
- **Redeclaração/Reatribuição:** Não permitida (a referência não pode mudar)
- **Observação:** Para objetos e arrays, as propriedades ou itens podem ser alterados, mas a referência não.

```js
const z = 10;
console.log(z); // 10
// z = 20; // Erro: Assignment to constant variable

const pessoa = { nome: "Ana", idade: 25 };
pessoa.idade = 26; // Modifica propriedade, mas não a referência
console.log(pessoa); // { nome: "Ana", idade: 26 }
```

---

## 2. Funções e Arrow Functions

### Declaração de Função

- **Sintaxe tradicional:** Permite hoisting.

```js
function soma(a, b) {
  return a + b;
}
console.log(soma(2, 3)); // 5
```

### Expressão de Função

- **Sintaxe:** Pode ser anônima ou nomeada.

```js
const subtrai = function(a, b) {
  return a - b;
};
console.log(subtrai(5, 3)); // 2
```

### Arrow Functions

- **Sintaxe curta e clara**
- **`this` léxico:** Não cria seu próprio contexto de `this`

```js
const multiplica = (a, b) => a * b;
console.log(multiplica(3, 4)); // 12

// Exemplo com múltiplas linhas:
const divide = (a, b) => {
  if (b === 0) return 'Divisão por zero não permitida';
  return a / b;
};
console.log(divide(10, 2)); // 5
```

---

## 3. Template Literals

- **Utilização:** Criação de strings com interpolação de variáveis e suporte a multilinha.

```js
const nome = "João";
const idade = 30;
const mensagem = `Olá, meu nome é ${nome} e tenho ${idade} anos.
Seja bem-vindo!`;
console.log(mensagem);
```

---

## 4. Destructuring

### Em Arrays

- **Extrai valores de arrays em variáveis individuais.**

```js
const numeros = [1, 2, 3];
const [um, dois, tres] = numeros;
console.log(um, dois, tres); // 1 2 3
```

### Em Objetos

- **Extrai propriedades de objetos em variáveis.**

```js
const pessoa = { nome: "Maria", idade: 25 };
const { nome, idade } = pessoa;
console.log(nome, idade); // Maria 25
```

---

## 5. Operadores Rest e Spread

### Rest Operator

- **Uso:** Agrupa os argumentos restantes em uma função ou elementos em array.

```js
function somaTudo(...numeros) {
  return numeros.reduce((acc, curr) => acc + curr, 0);
}
console.log(somaTudo(1, 2, 3, 4)); // 10
```

### Spread Operator

- **Uso:** Expande arrays ou objetos em elementos individuais.

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
console.log(arr2); // [1, 2, 3, 4, 5]

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 };
console.log(obj2); // { a: 1, b: 2, c: 3 }
```

---

## 6. Classes (ES6)

- **Utilização:** Facilita a criação de objetos e a implementação de herança.

```js
class Animal {
  constructor(nome) {
    this.nome = nome;
  }
  
  falar() {
    console.log(`${this.nome} faz um som.`);
  }
}

class Cachorro extends Animal {
  falar() {
    console.log(`${this.nome} late.`);
  }
}

const dog = new Cachorro("Rex");
dog.falar(); // Rex late.
```

---

## 7. Promises e Async/Await

### Promises

- **Uso:** Gerencia operações assíncronas.

```js
const promessa = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Operação concluída");
  }, 1000);
});

promessa
  .then(resultado => console.log(resultado))
  .catch(error => console.error(error));
```

### Async/Await

- **Uso:** Sintaxe mais simples e clara para trabalhar com Promises.

```js
async function executar() {
  try {
    const resultado = await promessa;
    console.log(resultado);
  } catch (error) {
    console.error(error);
  }
}
executar();
```

---

## 8. Módulos (Modules)

- **Uso:** Organiza o código em arquivos separados.

### Exportando

```js
// math.js
export function soma(a, b) {
  return a + b;
}
```

### Importando

```js
// app.js
import { soma } from './math.js';
console.log(soma(2, 3)); // 5
```

---

## Considerações Finais

Este note abrange o essencial de JavaScript:

- **Declaração de variáveis** com `var`, `let` e `const` e suas diferenças;
- **Funções tradicionais e arrow functions**;
- **Template literals** para strings dinâmicas;
- **Destructuring** para arrays e objetos;
- **Operadores rest e spread** para manipulação de dados;
- **Classes, Promises, Async/Await** e **Módulos** para estruturar aplicações modernas.

Estes são os fundamentos que você precisa dominar para começar a desenvolver com JavaScript. Conforme avançar, explore tópicos como closures, prototipagem, IIFE e padrões de design para aprofundar seu conhecimento.
