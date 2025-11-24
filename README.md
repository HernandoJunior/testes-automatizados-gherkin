# Automação de Testes de API - Entregas (Cucumber & RestAssured)

Este projeto consiste em uma suíte de testes automatizados de API desenvolvida em **Java 21**, utilizando a abordagem **BDD (Behavior Driven Development)**. O objetivo é validar o comportamento, a integridade dos dados e os contratos de uma API de gerenciamento de entregas.

O projeto segue padrões de design robustos, separando as camadas de especificação (Gherkin), execução (Steps/Runner) e lógica de requisição (Services), garantindo escalabilidade e fácil manutenção.

## 🚀 Tecnologias Utilizadas

Este framework utiliza um conjunto de tecnologias modernas para garantir a qualidade do software:

**Core & Linguagem:**

  * **Java 21**: Versão LTS mais recente utilizada no projeto.
  * **Maven**: Gerenciamento de dependências e build.

**Testes & BDD:**

  * **Cucumber Java (7.18.1)**: Framework para escrita de testes em linguagem natural (Gherkin/PT-BR).
  * **Cucumber JUnit**: Executor dos testes integrado ao JUnit.
  * **Rest Assured (5.5.0)**: Biblioteca fluente para testar e validar serviços REST.

**Validação & Dados:**

  * **Json Schema Validator (NetworkNT)**: Para validação de contratos JSON (Contract Testing).
  * **Lombok**: Redução de código boilerplate (Getters/Setters/Constructors).
  * **Gson & org.json**: Manipulação e serialização de objetos JSON.

**CI/CD:**

  * **GitHub Actions**: Pipeline de integração contínua configurada.

## ✨ Principais Cenários de Teste

A automação cobre fluxos críticos da API de entregas, documentados nos arquivos `.feature`:

### 📦 Cadastro de Entregas (`CadastroEntrega.feature`)

  * **Caminho Feliz**: Validação do cadastro bem-sucedido de uma entrega (Status 201).
  * **Caminho de Exceção**: Validação de erro ao enviar dados inválidos (ex: `statusEntrega` incorreto), garantindo retorno 400 e mensagem de erro apropriada.

### 🗑️ Gestão de Entregas (`ExemploContexto.feature`)

  * **Fluxo de Deleção**: Utiliza a funcionalidade de `Contexto` do Gherkin para pré-cadastrar uma entrega, recuperar seu ID dinamicamente e realizar a exclusão (Status 204).

### 📜 Teste de Contrato (`ExemploContrato.feature`)

  * **Validação de Schema**: Garante que a resposta da API (JSON) respeite estritamente o contrato definido (tipagem de dados, campos obrigatórios como `numeroPedido`, `nomeEntregador`, etc.) utilizando o arquivo `cadastro-bem-sucedido-de-entrega.json`.

## 🏗️ Estrutura do Projeto

O projeto está organizado para facilitar a leitura e a manutenção:

  * **`src/test/resources/features`**: Arquivos `.feature` escritos em Gherkin (PT-BR).
  * **`src/test/resources/schemas`**: Arquivos `.json` utilizados para validação de contrato.
  * **`src/test/java/steps`**: Camada que traduz os passos do Gherkin para código Java (`CadastroEntregasSteps.java`).
  * **`src/test/java/services`**: Camada de serviço (`CadastroEntregasService.java`) responsável por montar as requisições Rest Assured, serializar objetos e interagir com a API.
  * **`src/test/java/model`**: POJOs que representam os dados de envio e resposta (`EntregaModel`, `ErrorMessageModel`).
  * **`src/test/java/hook`**: Configurações de `@Before` e `@After` para preparação e limpeza de ambiente.
  * **`src/test/java/runner`**: Classe `TestRunner.java` configurada para executar os testes com a tag `@regressivo` e gerar relatórios HTML.

## ⚙️ Configuração e Execução

### Pré-requisitos

Para rodar os testes, é necessário que a API alvo esteja rodando localmente (mock ou aplicação real), conforme definido na classe de serviço:

  * **Base URL**: `http://localhost:8080`

### Executando os Testes

Para executar a suíte de testes via linha de comando, utilize o Maven:

```bash
mvn clean test
```

Isso acionará o `TestRunner`, que buscará pelos cenários anotados com `@regressivo`.

### Relatórios

Após a execução, um relatório HTML detalhado é gerado automaticamente em:
`target/cucumber-reports.html`

## 🔄 CI - Integração Contínua

O projeto possui um workflow configurado no GitHub Actions (`ci.yaml`) para garantir a integridade do código a cada alteração.

**Gatilhos:**

  * Push em qualquer branch.
  * Pull Requests.

**Pipeline (`continuous-integration`):**

1.  **Checkout**: Baixa o código do repositório.
2.  **Setup Java 21**: Configura o ambiente com a distribuição Temurin do JDK 21.
3.  **Compile**: Executa a compilação do projeto e dos testes (`mvn clean compile test-compile`).
4.  **Deploy (Simulado)**: Job condicional que simula o deploy da aplicação caso a integração seja bem-sucedida.
