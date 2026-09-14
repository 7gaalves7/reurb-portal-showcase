# 🏘️ REURB Portal

Portal web desenvolvido para **automatizar o fluxo de geração, envio e assinatura de contratos de REURB**.

O projeto centraliza dados de clientes, organiza remessas, gera documentos personalizados e integra todo o processo de assinatura eletrônica com a **Clicksign**.

> 🔒 Projeto desenvolvido para uso interno. Este repositório serve como demonstração técnica e de portfólio. Credenciais, dados reais, documentos jurídicos, tokens, banco de dados e URLs de produção não são disponibilizados.

---

## 📌 Visão geral

O sistema foi desenvolvido para reduzir tarefas manuais em um processo que anteriormente dependia de planilhas, geração individual de documentos, envio de links e acompanhamento separado das assinaturas.

A aplicação permite:

- 📊 Importar clientes através de planilhas `.xlsx`
- 📁 Organizar clientes em diferentes remessas
- 🔗 Gerar links únicos de confirmação
- 👤 Exibir ao cliente os dados já cadastrados
- 💳 Permitir a escolha de parcelas e dia de vencimento
- 📄 Gerar contratos `.docx` automaticamente
- ✍️ Criar processos de assinatura através da Clicksign
- 🔄 Controlar a ordem das assinaturas
- 📧 Enviar links iniciais por e-mail
- 📋 Acompanhar o status de cada processo
- 💾 Persistir os dados da aplicação
- 🔐 Proteger a área administrativa com autenticação

---

## 🔄 Fluxo da aplicação

```text
            📊 Planilha de clientes
                      │
                      ▼
             📁 Nova remessa
                      │
                      ▼
          🔗 Links individuais
                      │
                      ▼
        👤 Cliente confere os dados
                      │
                      ▼
      💳 Escolhe parcelas e vencimento
                      │
                      ▼
        📄 Contrato gerado automaticamente
                      │
                      ▼
          ☁️ Envio para Clicksign
                      │
                      ▼
            ✍️ Cliente assina
                      │
                      ▼
      ✍️ Responsável realiza assinatura final
                      │
                      ▼
             ✅ FINALIZADO
```

---

## 🚀 Principais funcionalidades

### 📁 Gestão de remessas

Cada nova planilha pode ser importada como uma **remessa independente**.

Isso permite trabalhar com diferentes grupos de clientes sem perder o histórico dos processos anteriores.

---

### 🔗 Links individuais

O sistema gera um **link exclusivo para cada cliente**.

Ao acessar o link, o cliente encontra seus dados previamente cadastrados e não precisa preencher todas as informações novamente.

---

### 💳 Escolha da forma de pagamento

Na página de confirmação, o cliente pode selecionar:

- quantidade de parcelas;
- dia de vencimento;
- confirmação dos dados cadastrados.

Após a confirmação, o processo de geração do contrato começa automaticamente.

---

### 📄 Geração automática de contratos

A aplicação utiliza um modelo de documento Word e substitui automaticamente os campos pelos dados correspondentes ao cliente.

São inseridas informações como:

- nome;
- CPF;
- RG;
- endereço;
- profissão;
- estado civil;
- contato;
- e-mail;
- quantidade de parcelas;
- vencimento;
- data.

O sistema também trata situações em que o Microsoft Word divide placeholders entre diferentes `runs` do documento.

---

## ✍️ Integração com Clicksign

A aplicação utiliza a **API v3 da Clicksign** para automatizar o processo de assinatura eletrônica.

O sistema é responsável por:

- criar o envelope;
- enviar o documento;
- cadastrar os signatários;
- configurar requisitos de assinatura;
- definir a ordem das assinaturas;
- ativar o envelope;
- acompanhar o andamento;
- identificar a conclusão do processo.

---

## 🔢 Ordem das assinaturas

Os signatários são separados em grupos:

```text
Grupo 1
   │
   └── Cliente
          │
          ▼
Grupo 2
   │
   └── Responsável pela assinatura final
```

Dessa forma, a segunda etapa ocorre somente após a conclusão da assinatura do cliente.

---

## 📊 Painel administrativo

O painel permite acompanhar os clientes de cada remessa e visualizar informações como:

| Informação | Descrição |
|---|---|
| Cliente | Nome cadastrado |
| E-mail | Endereço utilizado no processo |
| WhatsApp | Contato cadastrado |
| Processo | Situação atual |
| E-mail inicial | Status do envio |
| WhatsApp | Status da comunicação |
| Ações | Operações disponíveis |

Também é possível realizar ações individuais ou preparar uma remessa inteira.

---

## 🔐 Segurança

Algumas medidas utilizadas no projeto:

- 🔑 Credenciais armazenadas em variáveis de ambiente
- 🔐 Autenticação da área administrativa
- 🔗 Links individuais para cada cliente
- 🗄️ Banco de produção separado do repositório
- 🔑 Tokens de API fora do código-fonte
- 🚫 Dados pessoais não expostos no repositório público
- 🧪 Separação entre ambiente de testes e produção

---

## 💾 Persistência de dados

A aplicação utiliza **SQLite** para armazenar informações sobre:

- remessas;
- clientes;
- links gerados;
- processos;
- status dos envios;
- identificadores dos envelopes;
- andamento das assinaturas.

Em produção, o banco utiliza armazenamento persistente para que os dados sejam preservados entre novos deploys.

---

## 🛠️ Tecnologias utilizadas

### Backend

- **Python**
- **SQLite**
- **Requests**
- **OpenPyXL**
- **python-docx**

### Integrações

- **Clicksign API v3**
- **SMTP**
- **WhatsApp Business Cloud API** *(suporte previsto/configurável)*

### Infraestrutura

- **Render**
- **GitHub**
- **Persistent Disk**
- **Environment Variables**

---

## ☁️ Deploy

A aplicação foi implantada em ambiente cloud utilizando **Render**.

A infraestrutura utiliza:

```text
GitHub
   │
   ▼
Render
   │
   ├── Aplicação Python
   ├── Environment Variables
   ├── Persistent Disk
   │
   ├──── Clicksign API
   │
   └──── SMTP
```

Isso permite que o sistema continue funcionando independentemente do computador utilizado para desenvolvê-lo.

---

## 🧠 Desafios técnicos

Alguns dos principais desafios trabalhados durante o desenvolvimento:

- integração com API externa de assinatura eletrônica;
- controle da ordem entre múltiplos signatários;
- geração dinâmica de documentos Word;
- manipulação de placeholders divididos em múltiplos `runs`;
- importação e normalização de planilhas Excel;
- persistência dos dados entre deploys;
- acompanhamento automático do status das assinaturas;
- separação entre ambientes de Sandbox e Produção;
- armazenamento seguro de credenciais;
- autenticação da área administrativa;
- organização dos clientes por remessas.

---

## 👨‍💻 O que desenvolvi neste projeto

Durante o desenvolvimento foram implementados:

- arquitetura do fluxo da aplicação;
- importação e processamento de planilhas;
- estruturação do banco SQLite;
- gerenciamento de remessas;
- geração de links individuais;
- interface de confirmação do cliente;
- geração automática de documentos;
- integração com a Clicksign;
- controle sequencial das assinaturas;
- automação de e-mails;
- sincronização dos status;
- autenticação administrativa;
- configuração do ambiente de produção;
- deploy e persistência em cloud.

---

## 📸 Screenshots

### Painel administrativo

> Screenshot demonstrativo do painel será adicionado aqui.

### Confirmação do cliente

> Screenshot demonstrativo da página do cliente será adicionado aqui.

### Processo de assinatura

> Screenshot demonstrativo do fluxo de assinatura será adicionado aqui.

---

## 🔒 Privacidade

Este repositório é destinado à **demonstração técnica do projeto**.

Por questões de segurança e privacidade, não são disponibilizados:

- dados de clientes;
- banco de dados de produção;
- credenciais;
- tokens de API;
- senhas;
- documentos jurídicos reais;
- URLs administrativas;
- informações pessoais utilizadas em produção.

---

## 📈 Resultado

O projeto transforma um fluxo composto por diversas tarefas manuais em um processo integrado:

**Planilha → confirmação do cliente → geração do contrato → assinatura eletrônica → acompanhamento → finalização.**

O objetivo foi criar uma solução simples para o usuário final, mantendo a automação e as integrações concentradas no backend.

---

## 👤 Autor

**Gabriel Alves**

Projeto desenvolvido como solução de automação de processos e integração de sistemas.
