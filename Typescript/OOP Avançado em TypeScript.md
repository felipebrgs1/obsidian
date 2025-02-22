###### Este resumo aborda técnicas e práticas avançadas de Programação Orientada a Objetos em TypeScript, explorando desde os recursos mais recentes do ECMAScript até padrões de design que promovem código modular, escalável e fortemente tipado.

---

## 1. Encapsulamento Avançado

Além dos modificadores de acesso tradicionais (`public`, `protected` e `private`), TypeScript (alinhado com as novas especificações ECMAScript) permite o uso de campos privados reais com a sintaxe `#`. Esses campos não são acessíveis fora da classe, mesmo via indexação, aumentando a segurança do estado interno.

### Exemplo com Campos Privados Reais

```ts
class AdvancedCounter {
  #count: number = 0;

  increment(): void {
    this.#count++;
  }

  decrement(): void {
    if (this.#count > 0) {
      this.#count--;
    }
  }

  getCount(): number {
    return this.#count;
  }
}

const counter = new AdvancedCounter();
counter.increment();
console.log(`Contagem: ${counter.getCount()}`); // Contagem: 1
```

**Dicas Avançadas:**

- Use getters e setters para aplicar lógica de validação ou transformação.
- Combine campos privados com métodos auxiliares internos para manter invariantes do objeto.

---

## 2. Herança e Composição Avançada

### Herança com Mixins

TypeScript permite simular múltiplas heranças por meio de **mixins**, que são funções que estendem uma classe base com funcionalidades adicionais.

### Exemplo de Mixin

```ts
type Constructor<T = {}> = new (...args: any[]) => T;

function Timestamped<TBase extends Constructor>(Base: TBase) {
  return class extends Base {
    timestamp: Date = new Date();
  };
}

class Message {
  constructor(public content: string) {}
}

const TimestampedMessage = Timestamped(Message);
const msg = new TimestampedMessage("Olá, mundo!");
console.log(`Mensagem: ${msg.content} | Timestamp: ${msg.timestamp}`);
```

### Composição vs. Herança

Enquanto a herança estabelece uma relação "é um", a composição cria relações "tem um". Prefira composição para promover baixo acoplamento e reutilização flexível.

### Exemplo de Composição

```ts
class Logger {
  log(message: string): void {
    console.log(`[LOG]: ${message}`);
  }
}

class Service {
  private logger: Logger;

  constructor() {
    this.logger = new Logger();
  }

  executeTask(taskName: string): void {
    this.logger.log(`Executando tarefa: ${taskName}`);
    // Lógica da tarefa...
  }
}

const service = new Service();
service.executeTask("Importação de Dados");
```

---

## 3. Polimorfismo e Genéricos Avançados

Utilize **genéricos** para criar funções e classes polimórficas que operam com múltiplos tipos, mantendo a segurança de tipo e permitindo comportamentos customizados.

### Exemplo com Interface Genérica e Função Polimórfica

```ts
interface Comparable<T> {
  compareTo(other: T): number;
}

class NumberWrapper implements Comparable<NumberWrapper> {
  constructor(public value: number) {}

  compareTo(other: NumberWrapper): number {
    return this.value - other.value;
  }
}

function max<T extends Comparable<T>>(a: T, b: T): T {
  return a.compareTo(b) > 0 ? a : b;
}

const num1 = new NumberWrapper(10);
const num2 = new NumberWrapper(20);
const maxNumber = max(num1, num2);
console.log(`Número máximo: ${maxNumber.value}`); // Número máximo: 20
```

**Dicas Avançadas:**

- Explore **constraints** em genéricos para garantir que os tipos passem a implementar determinadas interfaces ou propriedades.
- Utilize tipos condicionais e inferência de tipos para escrever APIs ainda mais flexíveis e seguras.

---

## 4. Abstração e Padrões de Projeto

A abstração avançada envolve o uso de **interfaces** e **classes abstratas** para definir contratos que podem ser implementados de várias maneiras. Combine esses conceitos com padrões de projeto para resolver problemas recorrentes de forma elegante.

### Exemplo: Padrão Strategy com Abstração

```ts
interface Strategy {
  execute(data: number[]): number;
}

class SumStrategy implements Strategy {
  execute(data: number[]): number {
    return data.reduce((acc, cur) => acc + cur, 0);
  }
}

class AverageStrategy implements Strategy {
  execute(data: number[]): number {
    return data.reduce((acc, cur) => acc + cur, 0) / data.length;
  }
}

class DataProcessor {
  constructor(private strategy: Strategy) {}

  process(data: number[]): number {
    return this.strategy.execute(data);
  }
}

const sumProcessor = new DataProcessor(new SumStrategy());
console.log(`Soma: ${sumProcessor.process([1, 2, 3, 4])}`); // Soma: 10

const avgProcessor = new DataProcessor(new AverageStrategy());
console.log(`Média: ${avgProcessor.process([1, 2, 3, 4])}`); // Média: 2.5
```

**Dicas Avançadas:**

- Combine injeção de dependências com interfaces para criar sistemas facilmente testáveis e extensíveis.
- Explore outros padrões como Observer, Decorator e Factory para modularizar e dinamizar seu código.

---

## 5. Decorators e Metaprogramação

TypeScript suporta **decorators**, que permitem aplicar metaprogramação para modificar o comportamento de classes, métodos ou propriedades em tempo de compilação. Essa técnica é poderosa para adicionar funcionalidades transversais (como logging, validação ou injeção de dependências) sem poluir a lógica de negócio.

### Exemplo de Decorator de Método

```ts
function Log(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
): PropertyDescriptor {
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Chamando ${propertyKey} com argumentos: ${JSON.stringify(args)}`);
    const result = originalMethod.apply(this, args);
    console.log(`Resultado de ${propertyKey}: ${result}`);
    return result;
  };
  return descriptor;
}

class Calculator {
  @Log
  add(a: number, b: number): number {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(5, 3);
```

**Dicas Avançadas:**

- Utilize decorators para centralizar funcionalidades comuns, mantendo as classes focadas em suas responsabilidades principais.
- Combine decorators com metadados (por exemplo, usando a biblioteca `reflect-metadata`) para criar sistemas mais dinâmicos e configuráveis.

---

## Considerações Finais

Os conceitos avançados de OOP em TypeScript permitem a criação de aplicações robustas e escaláveis, explorando:

- **Encapsulamento avançado:** com campos privados reais e acesso controlado.
- **Herança e composição avançada:** utilizando mixins e padrões de composição para evitar hierarquias rígidas.
- **Polimorfismo com genéricos:** para criar APIs flexíveis e seguras.
- **Abstração combinada com padrões de projeto:** que facilitam a manutenção e a evolução do código.
- **Decorators e metaprogramação:** para aplicar funcionalidades transversais sem comprometer a clareza da lógica de negócio.

Ao dominar essas técnicas, você elevará o design do seu software, garantindo que ele seja modular, testável e preparado para evoluir junto com as necessidades do projeto.