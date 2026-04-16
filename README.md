# BioPoint — Sistema de Ponto Eletrônico Biométrico

> Sistema de controle de frequência com autenticação biométrica, desenvolvido para pequenas e médias empresas como alternativa segura e eficiente ao registro manual de ponto.

![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![Banco de Dados](https://img.shields.io/badge/banco%20de%20dados-PostgreSQL-blue)
![Metodologia](https://img.shields.io/badge/metodologia-cascata-purple)
![Instituição](https://img.shields.io/badge/instituição-UNIPLAN-green)

---

## Sobre o projeto

O **BioPoint** surgiu para resolver um problema real enfrentado por pequenas empresas: o controle de jornada de trabalho feito de forma manual ou informal, sujeito a erros, fraudes e riscos trabalhistas.

O sistema utiliza leitura biométrica (impressão digital) para registrar entradas e saídas dos funcionários com segurança, eliminando fraudes e automatizando o cálculo de horas extras, banco de horas e relatórios de frequência.

Projeto acadêmico desenvolvido no curso de **CST em Análise e Desenvolvimento de Sistemas — UNIPLAN**, aplicando o modelo de desenvolvimento em cascata.

---

## Protótipo de interface

Acesse o protótipo navegável completo desenvolvido no Figma:

🔗 [Visualizar protótipo no Figma](https://www.figma.com/proto/7bmX1jFmXqqSg8YW5oY19F/Sem%C3%ADtulo?node-id=2-2&t=PYMnhId0kpyCvGaw-1&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1&starting-point-node-id=2%3A2)

---

## Funcionalidades

- Cadastro de funcionários com dados pessoais e biometria (armazenada em formato `BYTEA`)
- Registro de ponto por leitura biométrica com validação de identidade
- Painel administrativo com controle de acesso (perfil admin e usuário comum)
- Cálculo automático de horas extras, atrasos e banco de horas
- Geração de relatórios de frequência (diário, semanal e mensal)
- Exportação de dados para integração com folha de pagamento
- Auditoria e histórico completo de ações no sistema
- Senhas criptografadas e controle de acesso por perfil

---

## Tecnologias utilizadas

| Camada | Tecnologia |
|---|---|
| Banco de dados | PostgreSQL |
| Modelagem | DER + Diagrama de Casos de Uso |
| Prototipação | Figma |
| Metodologia | Modelo Cascata |

---

## Modelagem do banco de dados

O banco relacional é composto pelas seguintes tabelas:

| Tabela | Descrição |
|---|---|
| `funcionario` | Dados cadastrais e biométricos dos funcionários |
| `usuario` | Controle de acesso ao sistema (admin / usuário comum) |
| `jornada` | Carga horária diária contratual |
| `funcionario_jornada` | Relacionamento entre funcionário e jornada (N:M) |
| `registro_ponto` | Eventos de entrada e saída |
| `intervalo` | Controle de pausas (ex: almoço) |
| `relatorio` | Metadados dos relatórios gerados |
| `auditoria` | Registro de todas as ações realizadas no sistema |
| `historico` | Histórico detalhado de alterações por tabela |

### Principais relacionamentos

- `funcionario` → `registro_ponto`: **1:N** — um funcionário possui múltiplos registros de ponto
- `funcionario` → `intervalo`: **1:N** — um funcionário pode ter múltiplos intervalos
- `funcionario` → `jornada`: via tabela `funcionario_jornada`
- `usuario` → `auditoria`: **1:N** — um usuário gera múltiplas entradas de auditoria

### Exemplo de criação do banco

```sql
CREATE DATABASE biopoint;

CREATE TABLE funcionario (
  id_funcionario SERIAL PRIMARY KEY,
  nome           VARCHAR(100) NOT NULL,
  cpf            VARCHAR(14) UNIQUE NOT NULL,
  data_nascimento DATE,
  telefone       VARCHAR(20),
  email          VARCHAR(100),
  biometria      BYTEA NOT NULL
);

CREATE TABLE registro_ponto (
  id_registro    SERIAL PRIMARY KEY,
  id_funcionario INT NOT NULL,
  data_hora      TIMESTAMP NOT NULL,
  tipo           VARCHAR(10) NOT NULL, -- 'entrada' ou 'saída'
  FOREIGN KEY (id_funcionario) REFERENCES funcionario(id_funcionario)
);
```

---

## Como executar o projeto

### Pré-requisitos

- [PostgreSQL 14+](https://www.postgresql.org/download/)
- pgAdmin 4 (opcional, para gerenciar o banco visualmente)

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/biopoint.git
cd biopoint

# 2. Crie o banco de dados
psql -U postgres -c "CREATE DATABASE biopoint;"

# 3. Execute o script de criação das tabelas
psql -U postgres -d biopoint -f database/schema.sql
```

---

## Requisitos do sistema

### Funcionais
| Código | Descrição |
|---|---|
| RF01 | Cadastrar funcionários com dados pessoais e biométricos |
| RF02 | Registrar ponto por leitura biométrica |
| RF03 | Validar identidade antes de registrar o ponto |
| RF04 | Consultar registros de entrada e saída |
| RF05 | Gerar relatórios de frequência (diário, semanal, mensal) |
| RF06 | Editar dados cadastrais dos funcionários |
| RF07 | Controlar usuários e permissões (admin) |
| RF08 | Exportar dados para folha de pagamento |

### Não funcionais
| Código | Descrição |
|---|---|
| RNF01 | Dados biométricos armazenados com criptografia |
| RNF02 | Registro de ponto em até 2 segundos |
| RNF03 | Disponibilidade mínima de 99% |
| RNF04 | Interface simples e intuitiva |
| RNF05 | Integridade garantida dos registros |

---

## Testes realizados

| Teste | Objetivo |
|---|---|
| Cadastro de funcionários | Verificar armazenamento correto de dados e biometria |
| Leitura biométrica | Validar reconhecimento e autenticação por digital |
| Registro de ponto | Confirmar registro preciso de entradas e saídas |
| Geração de relatórios | Verificar relatórios diários, semanais e mensais |
| Segurança | Garantir bloqueio de acessos não autorizados |

---

## Equipe

| Nome | GitHub |
|---|---|
| Mikael Carvalho Mendes | [@seu-usuario](https://github.com/seu-usuario) |
| Raíssa Adjuto Sanders Yoshyaky | [@seu-usuario](https://github.com/seu-usuario) |
| Tatiane da Silva Domiense | [@seu-usuario](https://github.com/seu-usuario) |

---

## Instituição

Desenvolvido no **Centro Universitário Planalto do Distrito Federal (UNIPLAN)**  
Curso: CST em Análise e Desenvolvimento de Sistemas — 2026

---

## Contato

📧 biopoint.tec@gmail.com
