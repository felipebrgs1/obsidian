##### A Programação Orientada a Objetos (OOP) é um paradigma que organiza o código em objetos que representam entidades do mundo real. Em TypeScript, esses princípios podem ser implementados de forma clara e tipada. A seguir, um resumo dos principais conceitos:

---

## 1. Encapsulamento

**Conceito:**  
Agrupa dados (propriedades) e comportamentos (métodos) dentro de uma única entidade (classe), ocultando os detalhes internos e protegendo o estado do objeto.

**Benefícios:**
- Protege os dados contra acesso e modificação indevida.
- Facilita a manutenção, pois a lógica está concentrada na própria classe.

**Exemplo:**

```ts
class ContaBancaria {
  // Propriedade privada
  private saldo: number = 0;

  // Método público para depositar
  depositar(valor: number): void {
    if (valor > 0) {
      this.saldo += valor;
    }
  }

  // Método público para consultar saldo
  getSaldo(): number {
    return this.saldo;
  }
}

const conta = new ContaBancaria();
conta.depositar(100);
console.log(`Saldo: ${conta.getSaldo()}`); // Saldo: 100
```

---

## 2. Herança

**Conceito:**  
Permite que uma classe (subclasse) herde propriedades e métodos de outra classe (superclasse), promovendo reutilização de código e hierarquização de classes.

**Benefícios:**
- Reutilização de código: subclasses herdam comportamentos comuns.
- Organização: facilita a modelagem de relações "é um tipo de".

**Exemplo:**

```ts
// Superclasse
class Veiculo {
  constructor(public marca: string) {}

  acelerar(): void {
    console.log("Acelerando...");
  }
}

// Subclasse que herda de Veiculo
class Carro extends Veiculo {
  constructor(marca: string, public modelo: string) {
    super(marca);
  }

  exibirDetalhes(): void {
    console.log(`Marca: ${this.marca}, Modelo: ${this.modelo}`);
  }
}

const meuCarro = new Carro("Toyota", "Corolla");
meuCarro.acelerar();
meuCarro.exibirDetalhes();
```

---

## 3. Polimorfismo

**Conceito:**  
Permite que objetos de classes diferentes sejam tratados de forma uniforme através de uma interface comum. Métodos com o mesmo nome podem ter comportamentos diferentes conforme o objeto que os implementa.

**Benefícios:**
- Flexibilidade: permite o uso de uma mesma interface para diversas implementações.
- Extensibilidade: facilita a adição de novas classes sem alterar o código existente.

**Exemplo:**

```ts
class Animal {
  falar(): void {
    console.log("Animal faz um som.");
  }
}

class Cachorro extends Animal {
  falar(): void {
    console.log("O cachorro late.");
  }
}

class Gato extends Animal {
  falar(): void {
    console.log("O gato mia.");
  }
}

function fazerAnimalFalar(animal: Animal): void {
  animal.falar();
}

const dog = new Cachorro();
const cat = new Gato();

fazerAnimalFalar(dog); // O cachorro late.
fazerAnimalFalar(cat); // O gato mia.
```

---

## 4. Abstração

**Conceito:**  
Envolve a criação de modelos simplificados que representam entidades complexas do mundo real. Em TypeScript, pode ser implementada por meio de classes abstratas e interfaces, definindo contratos sem especificar os detalhes de implementação.

**Benefícios:**
- Simplificação: foca no que é relevante para a aplicação.
- Flexibilidade: permite diferentes implementações para a mesma abstração.

**Exemplo com Interface:**

```ts
interface Forma {
  calcularArea(): number;
}

class Retangulo implements Forma {
  constructor(private largura: number, private altura: number) {}

  calcularArea(): number {
    return this.largura * this.altura;
  }
}

class Circulo implements Forma {
  constructor(private raio: number) {}

  calcularArea(): number {
    return Math.PI * this.raio ** 2;
  }
}

function exibirArea(forma: Forma): void {
  console.log(`Área: ${forma.calcularArea()}`);
}

const retangulo = new Retangulo(10, 5);
const circulo = new Circulo(7);

exibirArea(retangulo);
exibirArea(circulo);
```

---

## Resumo Visual

| **Princípio**     | **Descrição**                                                                          | **Benefícios**                                        |
|-------------------|----------------------------------------------------------------------------------------|-------------------------------------------------------|
| **Encapsulamento**| Oculta detalhes internos e protege o estado dos objetos.                               | Segurança, manutenção e modularidade.                 |
| **Herança**       | Permite a criação de hierarquias, reutilizando código entre classes relacionadas.      | Reutilização, organização e hierarquização.           |
| **Polimorfismo**  | Objetos podem ser tratados de forma genérica, permitindo múltiplas implementações.       | Flexibilidade e facilidade na extensão do código.     |
| **Abstração**     | Cria modelos simplificados (interfaces ou classes abstratas) para representar entidades. | Foco no essencial e flexibilidade de implementação.   |

---

Esses princípios formam a base da Programação Orientada a Objetos, ajudando a criar sistemas mais organizados, manuteníveis e escaláveis em TypeScript. Cada um deles pode ser combinado para estruturar aplicações robustas e de fácil evolução, mantendo o código limpo e eficiente para o desenvolvimento contínuo.