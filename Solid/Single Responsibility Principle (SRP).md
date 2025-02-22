## Definição Formal

O SRP foi definido por **Robert C. Martin (Uncle Bob)** da seguinte forma:

> "Uma classe deve ter apenas um motivo para mudar."

Isso implica que uma classe deve lidar apenas com uma única **responsabilidade**, evitando que diferentes lógicas de negócio sejam misturadas.

## Por que o SRP é importante?

1. **Código mais modular e organizado:** Cada classe tem um papel bem definido, facilitando a leitura e a manutenção.
2. **Facilidade para testes:** Classes menores e focadas permitem testes unitários mais eficientes.
3. **Menos acoplamento:** O código fica menos dependente de diferentes partes do sistema.
4. **Maior reutilização:** Componentes isolados podem ser reutilizados sem depender de partes não relacionadas.
5. **Facilidade de manutenção:** Alterações afetam apenas uma parte específica do sistema.

## Sintomas da violação do SRP

- **Mudanças frequentes na classe por razões distintas:** Se um arquivo precisa ser modificado frequentemente para diferentes propósitos, pode ser um sinal de múltiplas responsabilidades.
- **Classes muito grandes (God Objects):** Classes enormes e com múltiplas funções indicam uma violação do SRP.
- **Dificuldade em reutilizar partes do código:** Se um trecho de código não pode ser facilmente reaproveitado porque está acoplado a outras funcionalidades, há uma violação do SRP.

## Exemplo Prático

### Violação do SRP

A classe abaixo lida tanto com **dados do usuário** quanto com **persistência no banco de dados**, o que são responsabilidades diferentes:

```ts
class User {
  constructor(public name: string, public email: string) {}

  saveToDatabase() {
    console.log(`Saving ${this.name} to the database`);
    // Código para salvar no banco...
  }
}
```

 **Problema:** Se mudarmos a forma como o banco de dados funciona, precisaremos alterar essa classe, que deveria se preocupar apenas com os dados do usuário.

### Aplicando o SRP

Aqui, dividimos as responsabilidades em classes separadas:

```ts
class User {
  constructor(public name: string, public email: string) {}
}

class UserRepository {
  save(user: User) {
    console.log(`Saving ${user.name} to the database`);
    // Código para salvar no banco...
  }
}
```

✔ **Benefícios:**
✅ A classe `User` agora lida apenas com os **dados do usuário**.
✅ A classe `UserRepository` lida exclusivamente com a **persistência no banco de dados**.

## Aplicação do SRP em Arquitetura de Software

O SRP não se aplica apenas a classes, mas também a **módulos, serviços e até mesmo camadas inteiras do sistema**.

### Camadas de Arquitetura seguindo o SRP

1️⃣ **Camada de Apresentação (Frontend/API Controller):** Responsável por lidar com a entrada do usuário.
2️⃣ **Camada de Aplicação (Serviços/Use Cases):** Contém as regras de negócio e a lógica principal do software.
3️⃣ **Camada de Infraestrutura (Banco de Dados/Adapters):** Responsável por persistência de dados, envio de e-mails, logs, etc.

 **Exemplo em TypeScript com Arquitetura Limpa:**

```ts
// Camada de Entidade (Somente Dados)
class User {
  constructor(public name: string, public email: string) {}
}

// Camada de Aplicação (Regra de Negócio)
class UserService {
  constructor(private userRepository: UserRepository) {}

  registerUser(name: string, email: string) {
    const user = new User(name, email);
    this.userRepository.save(user);
  }
}

// Camada de Infraestrutura (Persistência)
class UserRepository {
  save(user: User) {
    console.log(`Saving ${user.name} to the database`);
  }
}

// Uso
const userRepository = new UserRepository();
const userService = new UserService(userRepository);
userService.registerUser("Alice", "alice@email.com");
```

✔ **Vantagens dessa abordagem:**
- Separação clara entre **dados**, **regra de negócio** e **infraestrutura**.
- Cada classe tem **apenas uma responsabilidade** e **um motivo para mudar**.
- **Facilidade para testes e manutenção**.

## Resumo

|  Benefício | ✅ Impacto |
|-------------|-----------|
| Código modular | Facilita a manutenção e a compreensão |
| Facilita testes unitários | Reduz dependências desnecessárias |
| Redução do acoplamento | Evita que mudanças em uma parte afetem todo o sistema |
| Reutilização de código | Partes independentes podem ser usadas em outros contextos |

> **Regra de Ouro:** Se uma classe parece ter múltiplas responsabilidades, divida-a em partes menores, cada uma focada em um único objetivo.

Com esse princípio bem aplicado, o código se torna mais escalável, fácil de manter e menos propenso a bugs. 