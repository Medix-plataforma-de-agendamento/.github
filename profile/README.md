# Medix – Documento de Onboarding e Organização do Projeto

## 1. Visão Geral

O **Medix** é uma plataforma de agendamento de consultas médicas voltada ao contexto **brasileiro**, concebida com foco em **qualidade técnica, governança de software, segurança da informação e escalabilidade**.

O sistema atende médicos autônomos, pacientes e administradores, permitindo o gerenciamento estruturado de agendas, consultas e dados sensíveis de saúde, em conformidade com a **LGPD (Lei Geral de Proteção de Dados)**.

Este documento tem caráter **institucional e técnico**, servindo como material oficial de onboarding para novos colaboradores, descrevendo visão, escopo, padrões, processos e decisões estruturais do Medix.

---

## 2. Objetivos do Projeto

### 2.1 Colaboradores do Projeto

O Medix é desenvolvido em regime de **evolução contínua**, seguindo padrões técnicos e organizacionais compatíveis com ambientes corporativos.

A participação dos colaboradores é registrada de forma transparente, incluindo o papel exercido e a dedicação média estimada ao projeto.

| Nome / Identificação    | Papel no Projeto                                                  | Dedicação Média (h/semana) |
| ----------------------- | ----------------------------------------------------------------- | -------------------------- |
| Desenvolvedor Principal | Desenvolvimento fullstack, arquitetura, documentação e governança | 8–10                       |

### 2.2 Objetivo Principal

Criar uma plataforma web moderna e segura para **agendamento e gerenciamento de consultas médicas**, servindo também como laboratório prático para:

* Arquitetura backend e frontend
* Segurança e autenticação
* Organização de tarefas e versionamento
* Documentação técnica e funcional
* CI/CD e deploy contínuo

### 2.3 Objetivos Secundários

* Aplicar padrões reais de mercado
* Evoluir o projeto de forma incremental e sustentável
* Manter rastreabilidade de decisões técnicas e funcionais

---

## 3. Usuários do Sistema

O Medix adota um modelo baseado em **papéis e contexto de autenticação**, onde um mesmo usuário pode possuir múltiplos papéis, mas as permissões são definidas pelo **tipo de login ativo**.

### 3.1 Pacientes (login via CPF + senha)

#### 3.1.1 Paciente comum

* Cadastro com informações pessoais
* Busca por consultas e horários disponíveis
* Agendamento de consultas
* Confirmação de comparecimento próximo à data da consulta
* Cancelamento de consultas
* Visualização de consultas agendadas
* Edição do perfil pessoal

#### 3.1.2 Paciente menor de idade

* Cadastro com informações pessoais
* Busca por consultas e horários disponíveis
* Agendamento de consultas vinculado a um responsável maior de idade
* Confirmação de comparecimento próximo à data da consulta
* Cancelamento de consultas
* Visualização de consultas agendadas
* Edição do perfil pessoal

> **Observação:** o agendamento de consultas para menores de idade exige confirmação obrigatória de um responsável maior de 18 anos. Caso a confirmação não ocorra em até 24 horas, a solicitação é automaticamente cancelada.

### 3.2 Médicos (login via CRM + senha)

* Cadastro com informações profissionais e pessoais (caso ainda não possua cadastro como paciente)
* Criação e gerenciamento de agenda
* Visualização da agenda
* Cancelamento de consultas futuras mediante justificativa obrigatória
* Visualização de informações básicas e relevantes do paciente para o atendimento
* Edição do perfil profissional

> **Restrição importante:** o perfil médico não possui acesso às funcionalidades de agendamento. Para agendar consultas, o usuário deve estar autenticado como paciente (CPF).

### 3.3 Administradores (login via código + senha)

* Acesso aos perfis de todos os usuários
* Visualização de movimentações e auditoria
* Bloqueio e desbloqueio de usuários
* Agendamento e cancelamento de consultas em nome de pacientes
* Cadastro de novos usuários (pacientes, médicos e administradores)
* Redefinição de senha com geração de senha temporária

---

## 4. Escopo do MVP (Produto Mínimo Viável)

O MVP do Medix contempla as funcionalidades essenciais para operação segura e controlada do sistema:

* Cadastro de pacientes

  * Suporte a pacientes menores de idade
  * Vinculação de acompanhante exclusivamente à consulta
  * Fluxo de confirmação obrigatória da consulta por usuário maior de 18 anos
* Cadastro de médicos

  * Associação a múltiplas especialidades
  * Configuração de agenda por especialidade
  * Definição de duração padrão das consultas por especialidade
* Agendamento de consultas

  * Proibição de sobreposição de horários
  * Cancelamento livre por paciente ou médico
* Segurança e conformidade

  * Autenticação baseada em múltiplos identificadores:

    * CPF (pacientes)
    * CRM (médicos)
    * Código interno (administradores)
  * Autorização baseada em papel e contexto de autenticação
  * Adequação à LGPD desde o MVP
* Perfil administrativo

  * Gestão básica de usuários
  * Visualização de logs e auditoria
* Envio de e-mails

  * Confirmação de agendamento
  * Cancelamento de consultas

---

## 5. Funcionalidades Futuras (Roadmap)

As funcionalidades a seguir estão mapeadas para evolução futura do Medix, não fazendo parte do escopo do MVP:

* Gerenciamento de prontuários médicos
* Modelos assistidos para:

  * Receituários
  * Guias médicas
  * ATAs
* Cadastro e gestão de clínicas com múltiplos profissionais
* Compartilhamento e versionamento de documentos entre médico e paciente
* Impressão de documentos
* Autenticação via provedores externos (ex.: Google)
* Uso de Inteligência Artificial para:

  * Simplificação da linguagem médica para pacientes
  * Agendamento inteligente via chatbot
* Avaliação de profissionais
* Envio de lembretes e confirmações via WhatsApp
* Controle de sigilo de documentos médicos

  * Documentos sigilosos visíveis apenas ao médico criador e administradores
  * Documentos não sigilosos acessíveis a outros profissionais conforme regra de acesso

---

## 6. Metodologia de Trabalho

### 6.1 Metodologia

* **Kanban**, com foco em fluxo contínuo e melhoria incremental

### 6.2 Colunas do Board

| Coluna                        | Descrição                                         |
| ----------------------------- | ------------------------------------------------- |
| Open                          | Funcionalidades mapeadas, sem definição detalhada |
| Refinamento                   | Análise de regras e critérios                     |
| A Fazer                       | Pronto para desenvolvimento                       |
| Fazendo                       | Em desenvolvimento                                |
| Revisando PR                  | Revisão de código                                 |
| Aguardando testes             | Pronto para validação                             |
| Realizando testes             | Testes em execução                                |
| Aguardando deploy em produção | Aprovado para release                             |
| Pronto                        | Entregue e finalizado                             |

### 6.3 Estimativa de Esforço

* Cards estimados por **peso (Sequência de Fibonacci)**

---

## 7. Labels do Projeto

### 7.1 Labels de Tipo

* **Bug** – Erro identificado em ambiente de desenvolvimento
* **Defeito** – Erro identificado em produção
* **Melhoria** – Otimização de algo funcional
* **Desenvolvimento** – Nova funcionalidade
* **Impedimento** – Dependência externa
* **Block** – Bloqueio interno

### 7.2 Labels Técnicas

* **Frontend**
* **Backend**
* **Banco de dados**

### 7.3 Labels de Escopo

* **MVP** – Funcionalidade pertencente ao MVP
* **Nome da funcionalidade** – Label específica para agrupar cards relacionados

---

## 8. Stack Tecnológica

### Backend

* Java + **Spring Boot** (arquitetura monolítica modular)

### Frontend

* **Angular**
* Angular Material
* Design System próprio do Medix

### Persistência

* Banco relacional: **PostgreSQL**
* Cache: **Redis**

### Infraestrutura e DevOps

* CI/CD: **Render**
* Versionamento e colaboração: **GitHub**

---

## 9. Padrões de Desenvolvimento

### 9.1 Padrão de Branches

* `feature/` – Novas funcionalidades
* `bugfix/` – Correções de bugs
* `hotfix/` – Correções críticas em produção

### 9.2 Padrão de Commits (Conventional Commits)

* `feat(escopo):` nova funcionalidade
* `fix(escopo):` correção de bug
* `docs(escopo):` documentação
* `refactor(escopo):` refatoração sem mudança de comportamento
* `perf(escopo):` melhoria de performance
* `test(escopo):` testes
* `build:` dependências e configurações
* `ci:` alterações de CI/CD
* `revert:` reversão de commit
* `chore:` tarefas que não afetam código-fonte

---

## 10. Wiki de Funcionalidades

A Wiki do projeto deve documentar **cada funcionalidade relevante**, contendo:

Para cada página/fluxo:

* Objetivo da funcionalidade

* Usuário principal

* Fluxo feliz (happy path)

* Fluxos infelizes (erros, exceções, validações)

* Dependências com outros fluxos

* Decisões importantes e motivações

* Quem inicia o fluxo

* Diferença entre paciente adulto e menor de idade

* Regras de validação

* Dependência com autenticação e permissões

---

## 11. Governança, Segurança e Qualidade

### 11.1 Auditoria

* Todas as ações sensíveis do sistema devem ser auditadas
* Cada registro de auditoria deve conter:

  * Código único do usuário responsável (inclusive administradores)
  * Tipo da ação executada
  * Entidade afetada
  * Data e hora

### 11.2 Segurança

* Senhas nunca são visualizadas por nenhum perfil
* Administradores podem redefinir senhas por meio de senhas temporárias
* Acesso a dados sensíveis é restrito conforme papel e contexto de autenticação

### 11.3 Definition of Done (DoD)

Uma demanda só pode ser considerada **Pronto** quando:

* Código implementado e revisado via Pull Request
* Testes unitários e de integração executados
* Testes manuais realizados
* Documentação (Wiki) atualizada

---

## 12. Considerações Finais

O Medix é um projeto vivo, que deve evoluir de forma organizada, incremental e documentada. Decisões técnicas e funcionais devem ser registradas sempre que impactarem o futuro do sistema.

Este documento deve ser mantido atualizado e é o **ponto de entrada oficial** para qualquer pessoa que venha a colaborar com o projeto.
