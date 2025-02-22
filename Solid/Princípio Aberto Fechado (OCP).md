## Definição Formal

O OCP foi originalmente formulado por Bertrand Meyer e, posteriormente, popularizado por Robert C. Martin (Uncle Bob). Sua premissa central é:

> "Software entities should be open for extension, but closed for modification."

Isso significa que o comportamento de uma classe ou módulo pode ser estendido por meio de herança, composição ou outros mecanismos sem que seja necessário modificar o código original.

## Por que o OCP é Importante?

- **Segurança e Confiabilidade:** Ao evitar modificações em código já funcional, reduz-se o risco de regressões.
- **Flexibilidade e Extensibilidade:** Permite adicionar novas funcionalidades sem impactar as implementações existentes.
- **Manutenção Facilitada:** O código torna-se mais robusto a mudanças, pois cada alteração nova se dá através de extensões específicas.
- **Isolamento de Mudanças:** Mudanças em requisitos ou comportamentos específicos não forçam alterações em partes estáveis do sistema.

## Exemplo Prático

### Violação do OCP

Imagine uma classe que calcula a área de diferentes formas geométricas utilizando condicionais para identificar o tipo de forma. Se uma nova forma for adicionada, será necessário alterar o método de cálculo:

```ts
class AreaCalculator {
  calculateArea(shapes: any[]): number {
    let total = 0;
    shapes.forEach(shape => {
      if (shape.type === 'circle') {
        total += Math.PI * shape.radius * shape.radius;
      } else if (shape.type === 'square') {
        total += shape.side * shape.side;
      }
      // Para cada nova forma, o método precisará ser modificado!
    });
    return total;
  }
}
````

**Problema:** Toda vez que uma nova forma for implementada, o código do `AreaCalculator` precisará ser alterado, violando o princípio de estar "fechado para modificação".

### Aplicando o OCP

Utilizando o polimorfismo, cada forma define seu próprio método para calcular a área. Assim, o `AreaCalculator` não precisa conhecer os detalhes de cada forma e permanece inalterado quando novas formas são adicionadas:

TypeScript

```ts
// Definindo uma classe abstrata para representar uma forma
abstract class Shape {
  abstract area(): number;
}

class Circle extends Shape {
  constructor(public radius: number) {
    super();
  }

  area(): number {
    return Math.PI * this.radius * this.radius;
  }
}

class Square extends Shape {
  constructor(public side: number) {
    super();
  }

  area(): number {
    return this.side * this.side;
  }
}

class AreaCalculator {
  calculateArea(shapes: Shape[]): number {
    let total = 0;
    shapes.forEach(shape => {
      total += shape.area();
    });
    return total;
  }
}

// Uso
const shapes: Shape[] = [
  new Circle(5),
  new Square(10)
];

const calculator = new AreaCalculator();
console.log(`Área Total: ${calculator.calculateArea(shapes)}`);
```

✔ **Benefícios:**

- **Extensibilidade:** Para adicionar uma nova forma, basta criar uma nova classe que estenda `Shape` e implementar o método `area()`, sem alterar o `AreaCalculator`.
- **Isolamento:** Cada forma encapsula sua própria lógica de cálculo, facilitando testes e manutenção.
- **Código Limpo:** O `AreaCalculator` permanece simples e focado, sem depender de condições ou lógica específica de cada tipo de forma.

## Aplicação do OCP em Arquitetura de Software

O OCP pode ser aplicado não apenas em classes, mas em módulos, serviços e até mesmo na estrutura geral do sistema. Ao adotar práticas como injeção de dependências e interfaces, desenvolvedores garantem que partes do sistema possam ser estendidas com mínimo impacto nas demais.

### Exemplo Prático em um Contexto de Pagamentos:

Imagine um sistema que processa pagamentos e precisa oferecer suporte a diferentes métodos (cartão, boleto, criptomoedas, etc.). Ao aplicar o OCP, você define uma interface ou classe abstrata para pagamentos, e cada método de pagamento implementa seu comportamento específico:

TypeScript

```ts
// Interface para o método de pagamento
interface PaymentMethod {
  pay(amount: number): void;
}

class CreditCardPayment implements PaymentMethod {
  pay(amount: number): void {
    console.log(`Processando pagamento de R$${amount} com Cartão de Crédito`);
  }
}

class BoletoPayment implements PaymentMethod {
  pay(amount: number): void {
    console.log(`Processando pagamento de R$${amount} com Boleto`);
  }
}

class PaymentProcessor {
  // Aceita qualquer implementação de PaymentMethod
  processPayment(method: PaymentMethod, amount: number): void {
    method.pay(amount);
  }
}

// Uso
const processor = new PaymentProcessor();
processor.processPayment(new CreditCardPayment(), 100);
processor.processPayment(new BoletoPayment(), 200);
```

✔ **Benefícios nessa abordagem:**

- **Flexibilidade:** Novos métodos de pagamento podem ser adicionados sem alterar o `PaymentProcessor`.
- **Isolamento de Mudanças:** Cada implementação de `PaymentMethod` lida com suas particularidades sem afetar o processamento geral.

## Resumo

|   |   |
|---|---|
|**Benefício**|**✅ Impacto**|
|Extensibilidade|Novas funcionalidades podem ser adicionadas sem alterar o código existente.|
|Manutenção Facilitada|Reduz o risco de introduzir bugs em partes já testadas.|
|Isolamento de Responsabilidades|Cada componente lida com sua própria lógica, promovendo um sistema mais robusto.|
|Código Limpo e Modular|Menos condicionais e complexidade centralizada em um único módulo.|

> **Regra de Ouro:** Estruture seu código de forma que novas funcionalidades sejam implementadas através de extensões e não através de alterações no código existente.

Ao aplicar o Open/Closed Principle, você garante que o sistema evolua de maneira segura e controlada, mantendo a integridade do código original e facilitando a manutenção e a escalabilidade do software.