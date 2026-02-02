# Wiki – Atendimento Médico (MVP)

## 1. Visão Geral

Esta wiki descreve **exclusivamente as funcionalidades de Atendimento Médico incluídas no MVP do Medix**.

O objetivo é permitir que o médico:

* Visualize sua agenda
* Realize o atendimento
* Registre informações clínicas básicas
* Anexe prescrições e documentos

---

## 2. Acesso ao Atendimento Médico

* Apenas usuários com **perfil Médico** podem acessar esta funcionalidade
* O médico pode visualizar **somente a própria agenda**
* O administrador **não acessa conteúdo clínico**

---

## 3. Visualização da Agenda do Médico

A agenda do médico deve exibir:

* Data da consulta
* Horário de início e fim
* Nome do paciente
* Status da consulta

Consultas canceladas ou reagendadas **não devem ser exibidas como atendíveis**.

---

## 4. Início do Atendimento

* O médico seleciona uma consulta com status **Agendada** ou **Confirmada**
* A ação **Iniciar Atendimento**:

  * Registra data e hora de início
  * Altera o status da consulta para **Em atendimento**

### Regras

* Apenas uma consulta pode estar em atendimento por médico
* Consultas fora do horário agendado não podem ser iniciadas

---

## 5. Registro de Anotações Clínicas

Durante o atendimento, o médico pode registrar:

* Anotações clínicas em **texto livre**

### Regras

* As anotações:

  * São **obrigatórias** para encerrar o atendimento
  * Não podem ser editadas após o encerramento
  * Não são visíveis para o paciente

---

## 6. Prescrições e Documentos

O médico pode anexar documentos relacionados à consulta:

* Receitas médicas
* Guias ou outros documentos clínicos

### Regras

* Os documentos devem:

  * Estar vinculados à consulta
  * Ser anexados em formato PDF
* Após o encerramento:

  * Todos podem visualizar os documentos

---

## 7. Encerramento do Atendimento

Para encerrar uma consulta:

* As anotações clínicas devem estar preenchidas
* O médico seleciona **Encerrar Atendimento**

A ação:

* Registra data e hora de encerramento
* Altera o status da consulta para **Realizada**

Após o encerramento:

* A consulta não pode ser reaberta
* As anotações não podem ser alteradas

---

## 8. Auditoria (Obrigatória no MVP)

As seguintes ações devem gerar auditoria:

* Início do atendimento
* Encerramento do atendimento
* Upload de documentos

Cada evento deve registrar:

* Usuário responsável
* Data
* Hora

---

## 9. Regras Gerais

* O médico não pode alterar dados cadastrais do paciente
* O atendimento é sempre vinculado a uma consulta válida
* Nenhuma informação clínica pode ser excluída
