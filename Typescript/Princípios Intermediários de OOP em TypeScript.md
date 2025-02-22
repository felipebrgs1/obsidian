##### Este resumo explora os conceitos fundamentais da Programação Orientada a Objetos com uma abordagem intermediária, utilizando recursos avançados do TypeScript. Aqui, veremos como aplicar de forma robusta encapsulamento, herança (e composição), polimorfismo e abstração para construir aplicações escaláveis e de fácil manutenção.

---

## 1. Encapsulamento Avançado

O encapsulamento permite proteger o estado interno de uma classe, expondo apenas os métodos e propriedades necessários. Em TypeScript, usamos os modificadores de acesso `private`, `protected` e `public` e podemos recorrer a getters e setters para controlar a manipulação dos dados.

### Exemplo:

```ts
class ContaBancaria {
  private _saldo: number = 0;

  public depositar(valor: number): void {
    if (valor > 0) {
      this._saldo += valor;
    }
  }

  public sacar(valor: number): boolean {
    if (valor > 0 && this._saldo >= valor) {
      this._saldo -= valor;
      return true;
    }
    return false;
  }

  // Getter para acesso controlado
  public get saldo(): number {
    return this._saldo;
  }
}

const conta = new ContaBancaria();
conta.depositar(200);
if (conta.sacar(50)) {
  console.log(`Novo saldo: ${conta.saldo}`);
} else {
  console.log("Saldo insuficiente.");
}
```

**Dicas Intermediárias:**

- Use `protected` para permitir acesso em subclasses sem expor o membro publicamente.
- Implemente getters e setters para validar dados ou aplicar transformações ao acessar propriedades.

---

## 2. Herança e Composição

### Herança

A herança permite que uma classe derive de outra, reutilizando e estendendo funcionalidades. Ela facilita a criação de hierarquias, mas cuidado com hierarquias profundas que podem dificultar a manutenção.

### Exemplo com Herança:

```ts
class Veiculo {
  constructor(public marca: string) {}

  mover(): void {
    console.log("Veículo em movimento...");
  }
}

class Carro extends Veiculo {
  constructor(marca: string, public modelo: string) {
    super(marca);
  }

  mover(): void {
    console.log("Carro acelerando...");
  }
}
```

### Composição

A composição favorece a criação de classes complexas por meio da união de componentes menores, promovendo um design mais flexível.

### Exemplo com Composição:

```ts
class Motor {
  ligar(): void {
    console.log("Motor ligado");
  }
}

class CarroComposto {
  private motor: Motor;

  constructor(public marca: string, public modelo: string) {
    this.motor = new Motor();
  }

  acelerar(): void {
    this.motor.ligar();
    console.log("Carro acelerando com composição!");
  }
}

const carro = new CarroComposto("Ford", "Fusion");
carro.acelerar();
```

**Dicas Intermediárias:**

- Avalie se a relação "é um" (herança) ou "tem um" (composição) melhor modela seu problema.
- Prefira composição para evitar acoplamento excessivo e hierarquias profundas.

---

## 3. Polimorfismo

O polimorfismo permite tratar objetos de diferentes classes de maneira uniforme, utilizando métodos com o mesmo nome que se comportam de forma específica conforme a classe.

### Exemplo com Classes Abstratas:

```ts
abstract class Animal {
  abstract emitirSom(): void;
}

class Cachorro extends Animal {
  emitirSom(): void {
    console.log("Au Au");
  }
}

class Gato extends Animal {
  emitirSom(): void {
    console.log("Miau");
  }
}

function fazerAnimalEmitirSom(animal: Animal): void {
  animal.emitirSom();
}

const cachorro = new Cachorro();
const gato = new Gato();

fazerAnimalEmitirSom(cachorro);
fazerAnimalEmitirSom(gato);
```

**Dicas Intermediárias:**

- Use classes abstratas para definir um contrato de comportamento, obrigando as subclasses a implementar métodos essenciais.
- Considere o uso de interfaces para definir contratos quando precisar de múltiplas "heranças" de comportamento.

---

## 4. Abstração

A abstração consiste em simplificar a complexidade, ocultando detalhes irrelevantes e expondo apenas o essencial. Em TypeScript, podemos usar tanto interfaces quanto classes abstratas para criar essa camada de separação entre o "o que" e o "como".

### Exemplo com Interface:

```ts
interface IForma {
  calcularArea(): number;
}

class Quadrado implements IForma {
  constructor(private lado: number) {}

  calcularArea(): number {
    return this.lado ** 2;
  }
}

class Circulo implements IForma {
  constructor(private raio: number) {}

  calcularArea(): number {
    return Math.PI * this.raio ** 2;
  }
}

function exibirArea(forma: IForma): void {
  console.log(`Área: ${forma.calcularArea()}`);
}

exibirArea(new Quadrado(4));
exibirArea(new Circulo(3));
```

### Exemplo com Classe Abstrata:

```ts
abstract class Forma {
  abstract calcularArea(): number;

  exibirArea(): void {
    console.log(`Área calculada: ${this.calcularArea()}`);
  }
}

class Retangulo extends Forma {
  constructor(private largura: number, private altura: number) {
    super();
  }

  calcularArea(): number {
    return this.largura * this.altura;
  }
}

const retangulo = new Retangulo(5, 3);
retangulo.exibirArea();
```

**Dicas Intermediárias:**

- Combine interfaces e classes abstratas para aproveitar os benefícios de ambos: contratos flexíveis e implementação base.
- Use abstração para facilitar a troca de implementações sem alterar o código que depende desses contratos.

---

## Considerações Finais

Ao aprofundar o conhecimento nos princípios intermediários de OOP com TypeScript, você cria bases sólidas para aplicações robustas.

- **Encapsulamento** garante que os dados sejam manipulados de forma segura e controlada.
- **Herança e Composição** ajudam a organizar e reutilizar código, com a composição oferecendo uma alternativa para evitar hierarquias complexas.
- **Polimorfismo** permite escrever código flexível e expansível, tratando diferentes objetos de forma uniforme.
- **Abstração** simplifica a complexidade do sistema, facilitando a manutenção e evolução do código.

Utilize esses conceitos para estruturar projetos mais limpos e escaláveis, sempre avaliando o melhor padrão (herança ou composição) para cada situação e adotando boas práticas que promovam um design desacoplado e testável. Este conhecimento intermediário é fundamental para evoluir seu código e enfrentar desafios mais complexos no desenvolvimento de software.