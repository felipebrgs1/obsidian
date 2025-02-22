
O **Liskov Substitution Principle (LSP)** é o terceiro princípio do SOLID e estabelece que **subclasses devem ser substituíveis por suas classes base sem alterar o comportamento esperado do sistema**. Em outras palavras, se uma classe S é derivada de uma classe T, então objetos do tipo T devem poder ser substituídos por objetos do tipo S sem que o funcionamento do programa seja comprometido.

---

## 📌 **Definição Formal**

Formulado por Barbara Liskov, o princípio pode ser resumido assim:

> "Se S é uma subclasse de T, então objetos de T devem ser substituíveis por objetos de S sem alterar as propriedades desejáveis do programa (correção, desempenho, etc.)."

Isso significa que as subclasses devem respeitar o contrato estabelecido pela classe base, garantindo que a interface e o comportamento esperado sejam mantidos.

---

## 🎯 **Por que o LSP é Importante?**

- **Consistência de Contrato:** Garante que as subclasses implementem ou extendam corretamente os comportamentos da classe base, mantendo as expectativas de quem utiliza a interface.
- **Polimorfismo Seguro:** Permite que métodos que operam sobre a classe base possam trabalhar com qualquer subclasse sem a necessidade de ajustes ou verificações adicionais.
- **Manutenção e Evolução:** Facilita a refatoração e a evolução do sistema, já que alterações em uma subclasse não impactam negativamente as funcionalidades que dependem da classe base.
- **Redução de Erros:** Ao garantir que a substituição de objetos seja transparente, minimiza a introdução de bugs decorrentes de implementações inadequadas nas subclasses.

---

## 🛠 **Exemplo Prático**

### ❌ **Violação do LSP**

Imagine uma hierarquia de classes onde a classe base `Bird` define um método `fly()`. Contudo, algumas aves, como o pinguim, não conseguem voar. Veja o exemplo:

```ts
class Bird {
  fly(): void {
    console.log("A ave está voando.");
  }
}

class Sparrow extends Bird {
  // Implementação herdada de fly(), adequada para pardais.
}

class Penguin extends Bird {
  fly(): void {
    throw new Error("Pinguins não podem voar.");
  }
}

function makeBirdFly(bird: Bird): void {
  bird.fly();
}

const penguin = new Penguin();
// Ao tentar executar makeBirdFly com um Pinguim, o comportamento esperado é quebrado:
makeBirdFly(penguin); // Lança um erro.
```

Nesse caso, a substituição de um objeto do tipo `Bird` por um objeto do tipo `Penguin` quebra o contrato esperado, pois o método `fly()` não se comporta como os demais.

---

### ✅ **Aplicando o LSP Corretamente**

Para respeitar o LSP, é preciso repensar a hierarquia. Uma abordagem é separar os comportamentos de voo da abstração geral de aves, criando uma distinção clara entre aves que voam e aves que não voam:

```ts
// Classe base para todas as aves, contendo atributos e métodos comuns.
class Bird {
  // Propriedades e métodos comuns a todas as aves.
}

// Classe específica para aves que podem voar.
class FlyingBird extends Bird {
  fly(): void {
    console.log("A ave está voando.");
  }
}

class Sparrow extends FlyingBird {
  // Pardal herda o comportamento de voar.
}

class Penguin extends Bird {
  // Pinguins não implementam o método fly, evitando comportamento inadequado.
}

// Função que opera somente com aves que podem voar.
function makeFlyingBirdFly(bird: FlyingBird): void {
  bird.fly();
}

const sparrow = new Sparrow();
makeFlyingBirdFly(sparrow); // Funciona corretamente.
```

Ao separar a responsabilidade de voo em uma subclasse específica, evitamos que classes que não podem voar (como `Penguin`) sejam forçadas a implementar um método inadequado, garantindo a substituibilidade correta.

---

## 📖 **Resumo**

|🚀 **Benefício**|✅ **Impacto**|
|---|---|
|**Consistência de Interface**|Garante que todas as subclasses respeitem o contrato da classe base.|
|**Polimorfismo Seguro**|Permite a substituição de objetos sem surpresas ou erros inesperados.|
|**Manutenção Facilitada**|Simplifica a evolução do sistema, isolando comportamentos específicos.|
|**Redução de Erros**|Evita bugs causados por implementações inadequadas em subclasses.|

> **Regra de Ouro:** Ao estender uma classe, certifique-se de que a subclasse possa substituir a classe base sem alterar o comportamento esperado do programa. Isso mantém o sistema robusto, previsível e fácil de manter.

Aplicando o Liskov Substitution Principle, garantimos que nossas hierarquias de classes sejam coerentes e que a substituição de objetos seja transparente e segura, contribuindo para um código mais confiável e de fácil evolução.