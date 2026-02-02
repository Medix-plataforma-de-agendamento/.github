# Wiki – Agendamentos

## 1. Visão Geral

Esta wiki descreve as **regras funcionais de agendamento de consultas no Medix**, contemplando os diferentes perfis de usuário (Paciente, Médico e Administrador), bem como cenários específicos como **menor de idade**, **acompanhantes**, **confirmações**, **cancelamentos** e **duração de consultas por especialidade**.

O foco deste documento é **regra de negócio e comportamento funcional**, sem considerar decisões técnicas ou de arquitetura.

---

## 2. Regras Gerais de Agendamento

### 2.1 Janela de Agendamento

* As consultas devem ser agendadas com:

  * **Antecedência mínima:** 12 horas
  * **Antecedência máxima:** 3 meses

### 2.2 Conflito de Horários do Paciente

* Um paciente pode possuir múltiplas consultas ativas, desde que:

  * Exista um **intervalo mínimo de 30 minutos** entre o término de uma consulta e o início da próxima
* Não é permitido que um paciente possua **mais de uma consulta ativa com o mesmo profissional**, mesmo em datas diferentes

  * Nesse caso, o sistema deve bloquear o agendamento e exibir mensagem informativa
  * Deve ser oferecida a opção de **reagendamento da consulta existente**

### 2.3 Status de Consulta

Uma consulta pode assumir os seguintes status:

* **Agendada**
* **Aguardando confirmação do responsável**
* **Confirmada**
* **Reagendada**
* **Não compareceu**
* **Cancelada**
* **Finalizada**

---

## 3. Busca e Seleção de Horários

### 3.1 Filtros Disponíveis

Os filtros de busca podem ser utilizados de forma individual ou combinada:

* Nome do médico
* CRM
* Especialidade
* Data
* Horário
* Cidade

### 3.2 Exibição da Agenda

* A agenda exibida considera:

  * Horário de trabalho do médico
  * Dias da semana configurados
  * Duração da consulta definida por especialidade
  * Horários já ocupados não são exibidos

### 3.3 Configuração da Agenda Médica

* O médico define, no cadastro:

  * Especialidades atendidas
  * Dias da semana por especialidade
  * Horário de início e término do atendimento
  * Duração da consulta para cada especialidade

---

## 4. Confirmação de Comparecimento

* A confirmação de comparecimento é solicitada **24 horas antes da consulta**
* Quando confirmada:

  * O status da consulta é alterado para **Confirmada**
* Caso não haja confirmação:

  * O status permanece **Agendada**
  * Um **aviso visual** é exibido ao médico ao visualizar sua agenda

---

## 5. Agendamento pelo Administrador

* O administrador atua como **suporte ao usuário**, seguindo as mesmas regras funcionais de agendamento
* Permissões adicionais:

  * Pode agendar consultas com **menos de 12 horas de antecedência**
  * Pode restringir agendas (função administrativa)
* Todas as ações são registradas em auditoria com identificação do administrador responsável

---

## 6. Restrições por Perfil

### 6.1 Médico

* Pode visualizar **apenas a própria agenda**
* Pode cancelar apenas consultas futuras

  * É obrigatória a inclusão de uma **justificativa escrita com no mínimo 50 caracteres**
  * A justificativa é exibida ao paciente
* Caso o médico realize **mais de 10 cancelamentos em um período de 30 dias**:

  * A conta é **suspensa temporariamente**
  * É necessário contato com a administração para avaliação

### 6.2 Paciente

* Pode cancelar ou reagendar consultas
* Ao reagendar:

  * A consulta original recebe status **Reagendada**
  * A consulta anterior não é mais exibida na agenda
  * Um novo agendamento é criado

---

## 7. Cancelamento de Consultas

* Cancelamentos só podem ocorrer com **mínimo de 12 horas de antecedência**
* Consultas canceladas permanecem visíveis no **histórico**

---

## 8. Agendamento Envolvendo Menor de Idade

### 8.1 Regras de Agendamento

* O menor pode realizar o agendamento com:
  * Antecedência mínima de 24 horas
* Caso o agendamento esteja dentro de 12 horas:
  * É obrigatória a **confirmação do responsável**
  * Se não confirmada, a consulta é cancelada e o horário liberado

### 8.2 Responsável / Acompanhante

* No agendamento, o menor deve informar:
  * Nome
  * CPF
  * Data de nascimento
  * Grau de proximidade (ex.: mãe, pai, irmão)
* O vínculo do responsável é feito **exclusivamente pelo CPF informado**
* Apenas **um responsável por consulta** é permitido
* Substituição do responsável:
  * Permitida apenas até **32 horas antes do atendimento**
  * Caso o novo responsável não confirme, o responsável anterior é restaurado e deve confirmar novamente
* O responsável pode visualizar os **detalhes da consulta**
* O responsável não pode possuir outro agendamento para o mesmo horário

### 8.3 Validação Pós-Atendimento

* Após o atendimento, o médico deve confirmar se o responsável presente foi o mesmo informado
* Em caso de divergência:

  * A conta relacionada é suspensa
  * É necessário contato com o suporte para esclarecimento

---

## 9. Suspensão de Conta e Impactos

### 9.1 Médico

* Consultas já agendadas são mantidas
* Novos horários ficam bloqueados
* Aviso de conta suspensa é exibido no topo do sistema

### 9.2 Paciente

* Após 48 horas da suspensão:

  * Todas as consultas futuras são automaticamente canceladas

* Em ambos os casos:

  * São enviados **e-mails diários** informando a suspensão e suas consequências

---

## 10. Duração de Consultas por Especialidade

* A duração da consulta é definida no cadastro da especialidade
* Alterações de duração:

  * Só entram em vigor **após a última consulta agendada com a duração anterior**
* Não é permitido criar consultas com duração diferente da padrão definida

---

## 11. Auditoria e Rastreabilidade

* **Todas as ações** relacionadas a agendamentos devem ser auditadas, incluindo:

  * Criação
  * Confirmação
  * Cancelamento
  * Reagendamento
  * Alteração de responsável
* Cada registro deve conter:

  * Data e hora
  * Usuário responsável pela ação
