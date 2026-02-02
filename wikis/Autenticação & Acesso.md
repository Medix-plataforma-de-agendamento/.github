# Wiki – Autenticação & Acesso

## 1. Visão Geral

Esta wiki descreve as regras e fluxos de **autenticação, acesso e controle de sessão** do sistema Medix.

O objetivo é garantir **segurança, rastreabilidade e clareza funcional**, respeitando as particularidades de cada perfil de usuário (Paciente, Médico e Administrador).

> **Importante:** este documento trata apenas de regras funcionais e de negócio. Detalhes técnicos, algoritmos criptográficos e implementação serão abordados em documentação técnica específica.

---

## 2. Formas de Login

O sistema suporta múltiplas formas de autenticação, de acordo com o perfil do usuário.

| Perfil        | Identificador de Login | Observação                                                 |
| ------------- | ---------------------- | ---------------------------------------------------------- |
| Paciente      | CPF                    | Utilizado exclusivamente para acesso ao perfil de paciente |
| Médico        | CRM                    | Utilizado exclusivamente para acesso ao perfil médico      |
| Administrador | Código de inscrição    | Identificador interno do administrador                     |

### 2.1 Regras Gerais

* Cada perfil possui **credenciais próprias e independentes**
* O mesmo usuário pode autenticar-se em perfis diferentes, desde que possua autorização
* Não é permitido autenticar-se em um perfil utilizando o identificador de outro perfil
* A seleção do perfil ocorre implicitamente pelo identificador informado no login

---

## 3. Regras de Autenticação por Perfil

### 3.1 Paciente

* Autenticação via **CPF + senha do perfil paciente**
* O acesso é permitido somente após:

  * Confirmação do e-mail pessoal
  * Conta ativa e não bloqueada

### 3.2 Médico

* Autenticação via **CRM + senha do perfil médico**
* O acesso é permitido somente se:

  * O perfil médico estiver **aprovado pelo administrador**
  * O e-mail corporativo estiver confirmado
  * O perfil não estiver bloqueado

> Caso o médico possua também perfil de paciente, os acessos são tratados de forma independente.

### 3.3 Administrador

* Autenticação via **código de inscrição + senha do perfil administrador**
* O acesso é permitido somente se:

  * A conta estiver ativa
  * O perfil não estiver bloqueado

---

## 4. Confirmação de E-mail e Liberação de Conta

### 4.1 Confirmação de E-mail

* Todo cadastro exige **confirmação de e-mail** antes da liberação do acesso
* A confirmação é realizada por meio de **código ou link enviado ao e-mail informado**
* Cada perfil possui seu próprio e-mail associado

### 4.2 Liberação de Conta

* Perfil de paciente:

  * Liberado automaticamente após confirmação do e-mail pessoal

* Perfil de médico:

  * Liberação do perfil de paciente segue a regra padrão
  * O perfil de médico permanece **em análise administrativa** até aprovação

* Perfil de administrador:

  * Criado e liberado exclusivamente por outro administrador

---

## 5. Controle de Sessão

### 5.1 Sessão de Usuário

* Cada autenticação válida gera uma **sessão ativa**
* A sessão está sempre vinculada a:

  * Perfil autenticado
  * Usuário responsável
  * Data e hora de início

### 5.2 Expiração de Sessão

* Sessões possuem tempo de validade
* A expiração pode ocorrer por:

  * Inatividade
  * Logout explícito
  * Ação administrativa

---

## 6. Logout

* O logout pode ser realizado:

  * Pelo próprio usuário
  * Por ação administrativa
* O logout encerra imediatamente a sessão ativa
* O evento de logout deve ser registrado na auditoria

---

## 7. Bloqueio e Desbloqueio de Conta

### 7.1 Bloqueio

Uma conta pode ser bloqueada por:

* Ação administrativa
* Tentativas excessivas de autenticação inválida
* Violação de regras de segurança

Efeitos do bloqueio:

* Impede novas autenticações no perfil bloqueado
* Sessões ativas podem ser encerradas automaticamente

### 7.2 Desbloqueio

* O desbloqueio é realizado exclusivamente por um administrador
* Todas as ações de bloqueio e desbloqueio devem ser **registradas na auditoria**

---

## 8. Regras Gerais e Observações

* Cada perfil possui autenticação **independente**
* Bloqueio, desbloqueio e expiração de sessão são **por perfil**, não por usuário global
* Nenhuma credencial sensível é exibida ou compartilhada
* Toda ação relevante de autenticação gera registro de auditoria

