# REURB Portal

Portal web desenvolvido para automatizar o fluxo de geração, envio e assinatura de contratos de REURB.

O projeto centraliza dados de clientes, organiza remessas, gera documentos personalizados e integra o processo de assinatura eletrônica com a Clicksign.

> Projeto desenvolvido para uso interno. Este repositório público serve apenas como demonstração técnica e de portfólio. Credenciais, dados reais, documentos jurídicos, tokens, banco de dados e URLs de produção não são disponibilizados.

---

## Visão geral

O sistema foi criado para reduzir tarefas manuais em um fluxo que antes dependia de planilhas, geração individual de documentos, envio de links e acompanhamento separado das assinaturas.

A aplicação permite:

- importar clientes por planilha `.xlsx`;
- organizar clientes por remessas;
- gerar links únicos de confirmação;
- exibir ao cliente os dados já cadastrados;
- permitir escolha de parcelas e dia de vencimento;
- gerar contratos `.docx` personalizados;
- criar envelopes de assinatura na Clicksign;
- controlar a ordem de assinatura entre cliente e responsável final;
- acompanhar o status dos processos;
- enviar o link inicial por e-mail;
- manter os dados persistidos em banco SQLite;
- proteger a área administrativa com autenticação.

---

## Fluxo da aplicação

```text
Planilha de clientes
        ↓
Importação da remessa
        ↓
Geração dos links individuais
        ↓
Cliente confere os dados cadastrados
        ↓
Escolha de parcelas e vencimento
        ↓
Geração automática do contrato
        ↓
Criação do envelope na Clicksign
        ↓
Assinatura do cliente
        ↓
Assinatura do responsável final
        ↓
Processo finalizado
```

---

## Principais funcionalidades

### Gestão de remessas

Cada nova planilha pode ser importada como uma remessa independente, permitindo separar diferentes grupos de clientes e preservar o histórico dos processos anteriores.

### Links individuais

O sistema gera um link exclusivo para cada cliente. O cliente não precisa preencher novamente os dados já cadastrados.

### Geração automática de contratos

Os dados da planilha são inseridos automaticamente em um modelo `.docx`, incluindo informações do cliente, quantidade de parcelas, vencimento e data.

### Integração com Clicksign

A aplicação utiliza a API da Clicksign para:

- criar envelopes;
- enviar documentos;
- cadastrar signatários;
- definir requisitos de assinatura;
- controlar a ordem de assinatura;
- consultar o andamento do processo;
- identificar quando o envelope foi finalizado.

### Ordem de assinatura

O fluxo utiliza grupos de assinatura:

```text
Grupo 1 → Cliente
Grupo 2 → Responsável final
```

O segundo signatário só entra na etapa de assinatura após a conclusão da primeira.

### Persistência

Os dados operacionais ficam armazenados em SQLite em armazenamento persistente do ambiente de produção.

### Área administrativa protegida

As rotas administrativas possuem autenticação e as credenciais são lidas por variáveis de ambiente.

---

## Tecnologias utilizadas

- Python
- HTTP Server nativo do Python
- SQLite
- OpenPyXL
- python-docx
- Requests
- Clicksign API v3
- SMTP
- Render
- GitHub

---

## Estrutura simplificada

```text
reurb-portal/
├── reurb_portal_FINAL.py
├── requirements.txt
├── render.yaml
├── .env.example
├── README.md
└── assets/
    └── screenshots/
```

Arquivos sensíveis e dados reais não fazem parte da versão pública.

---

## Configuração por variáveis de ambiente

A aplicação utiliza variáveis de ambiente para separar configuração e código.

Exemplos:

```env
CLICKSIGN_BASE_URL=
CLICKSIGN_API_TOKEN=

REURB_PUBLIC_URL=
REURB_DATA_DIR=

REURB_ADMIN_USER=
REURB_ADMIN_PASSWORD=

REURB_SMTP_HOST=
REURB_SMTP_PORT=
REURB_SMTP_USER=
REURB_SMTP_PASSWORD=
REURB_SMTP_FROM=
```

Nenhum valor real deve ser versionado.

---

## Segurança

Algumas medidas aplicadas no projeto:

- credenciais fora do código;
- autenticação da área administrativa;
- links individuais para cada cliente;
- separação entre área administrativa e confirmação do cliente;
- armazenamento persistente fora do repositório;
- tokens de API mantidos em variáveis de ambiente;
- repositório de demonstração sem dados pessoais reais.

---

## Deploy

A aplicação foi implantada em ambiente cloud utilizando Render.

O ambiente de produção utiliza:

- variáveis de ambiente;
- armazenamento persistente;
- processo web Python;
- integração com serviços externos via API.

A URL de produção não é disponibilizada neste repositório.

---

## Desafios técnicos

Durante o desenvolvimento foram trabalhados pontos como:

- integração com uma API externa de assinatura eletrônica;
- controle de ordem entre múltiplos signatários;
- persistência de dados entre deploys;
- geração dinâmica de documentos Word;
- substituição de placeholders mesmo quando o Word divide o texto em múltiplos `runs`;
- acompanhamento automático do status dos envelopes;
- separação entre ambiente de testes e produção;
- proteção de credenciais e rotas administrativas.

---

## O que eu desenvolvi neste projeto

- arquitetura do fluxo;
- importação e tratamento de planilhas;
- banco SQLite;
- geração de documentos;
- integração com Clicksign;
- automação de e-mails;
- acompanhamento de status;
- organização por remessas;
- autenticação administrativa;
- configuração e deploy em produção.

---

## Screenshots

Adicione aqui imagens com dados fictícios ou censurados.

### Painel administrativo

```text
assets/screenshots/admin.png
```

### Confirmação do cliente

```text
assets/screenshots/confirmacao.png
```

### Fluxo de assinatura

```text
assets/screenshots/assinatura.png
```

---

## Observação

Este projeto está apresentado como estudo de caso e portfólio.

Por segurança e privacidade, a versão pública não inclui:

- base de clientes;
- banco de produção;
- credenciais;
- tokens;
- documentos jurídicos reais;
- links de produção;
- e-mails reais;
- dados pessoais.

---

## Autor

Desenvolvido por **Gabriel Alves**.
