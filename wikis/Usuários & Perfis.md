# Wiki – Usuários & Perfis

## 1. Visão Geral

Esta wiki descreve o **fluxo de cadastro de usuários no Medix**, separando claramente as regras e comportamentos por **perfil** (Paciente, Médico e Administrador).

Antes de detalhar os fluxos específicos, são apresentadas as **informações comuns e específicas** que podem ser registradas no sistema, bem como suas regras de obrigatoriedade e unicidade.

> **Importante:** neste documento não são consideradas decisões de arquitetura, modelagem de dados ou estrutura técnica. O foco é **funcional e de negócio**.

---

## 2. Informações Cadastrais Comuns

Legenda:
* `*` Campo obrigatório
* `#` Campo único no sistema

Observações gerais:
* E-mails são **únicos por perfil**
* Cada perfil possui **credenciais próprias e independentes**

### 2.1 Dados pessoais (todos os perfis)

| Campo              | Paciente | Médico | Administrador | Obrigatório | Único |
| ------------------ | -------- | ------ | ------------- | ----------- | ----- |
| Nome               | ✔️       | ✔️     | ✔️            | ✔️          | ❌    |
| CPF                | ✔️       | ✔️     | ✔️            | ✔️          | ✔️    |
| Data de nascimento | ✔️       | ✔️     | ✔️            | ✔️          | ❌    |
| Gênero             | ✔️       | ✔️     | ✔️            | ✔️          | ❌    |
| RG                 | ✔️       | ✔️     | ✔️            | ❌          | ❌    |
| Senha              | ✔️       | ✔️     | ✔️            | ✔️          | ❌    |
| E-mail pessoal     | ✔️       | ✔️     | ✔️            | ✔️          | ❌    |
| Celular            | ✔️       | ✔️     | ✔️            | ✔️          | ❌    |

### 2.2 Endereço (todos os perfis)

| Campo          | Obrigatório |
| -------------- | ----------- |
| Logradouro     | ✔️          |
| Número da casa | ✔️          |
| CEP            | ✔️          |
| Complemento    | ❌          |
| Bairro         | ❌          |
| Cidade         | ✔️          |
| Estado         | ✔️          |

---

## 3. Informações Específicas por Perfil

| Campo                         | Paciente | Médico | Administrador | Obrigatório | Único | Observação                                   |
| ----------------------------- | -------- | ------ | ------------- | ----------- | ----- | -------------------------------------------- |
| CRM                           | ❌       | ✔️     | ❌            | ✔️          | ✔️    | Identificador profissional do médico         |
| Validade do CRM               | ❌       | ✔️     | ❌            | ✔️          | ❌    | Necessário para validação profissional       |
| Especialidades                | ❌       | ✔️     | ❌            | ✔️          | ❌    | Uma ou mais especialidades                   |
| E-mail corporativo            | ❌       | ✔️     | ✔️            | ✔️          | ❌    | Comunicação institucional                    |
| Senha do perfil médico        | ❌       | ✔️     | ❌            | ✔️          | ❌    | Credencial exclusiva do perfil médico        |
| Código de inscrição           | ❌       | ❌     | ✔️            | ✔️          | ✔️    | Identificador interno do administrador       |
| Senha do perfil administrador | ❌       | ❌     | ✔️            | ✔️          | ❌    | Credencial exclusiva do perfil administrador |

## 4. Fluxo de Cadastro por Perfil

## 4.1 Cadastro de Paciente

* Usuário não autenticado acessa a tela de **Entrar / Login**
* É apresentada a opção **“Realizar cadastro”**
* São informados todos os dados obrigatórios para cadastro de paciente
* O sistema envia um **e-mail de confirmação** com código de liberação
* Após a confirmação, o acesso ao sistema é liberado no perfil de paciente

---

## 4.2 Cadastro de Médico

* O cadastro pode ser iniciado por um usuário já autenticado como paciente ou por um usuário não autenticado

### 4.2.1 Usuário com cadastro prévio como paciente

* No menu "Conta" deve ser apresentado a opção **“Cadastrar também como médico”**
* São solicitadas as informações obrigatórias do perfil médico
* O sistema envia o e-mail de confirmação para os endereço profissional informado
* Após a confirmação o perfil de médico permanece **em análise administrativa** até aprovação

---

### 4.2.2 Usuário sem cadastro prévio

* Usuário não autenticado acessa **Entrar / Login**
* Seleciona **“Realizar cadastro”**
* O usuário informa os dados de paciente e marca a opção **“Cadastrar também como médico”**
* O sistema exibe os campos específicos do perfil médico que devem ser obrigatoriamente preenchidas
* O sistema envia e-mails de confirmação para os endereços informados
* Após a confirmação no email pessoal o perfil de paciente é liberado
* Após a confirmação no email profissional o perfil de médico permanece **em análise administrativa** até aprovação

---

## 4.3 Cadastro de Administrador

* Apenas usuários autenticados como administrador podem iniciar este fluxo
* O administrador acessa o menu de **Cadastro de Usuários**
* São preenchidas inicialmente as informações básicas comuns
* O administrador seleciona o perfil a ser criado
* O sistema exibe dinamicamente os campos específicos obrigatórios do perfil escolhido
* O usuário é criado no sistema
* Todas as ações são registradas na auditoria

---

## 5. Regras Gerais e Observações

* Um mesmo CPF pode possuir **múltiplos perfis**, totalmente independentes
* A desativação de um perfil **não impacta os demais perfis** do mesmo usuário
* Cada perfil possui:
  * Identificador de login próprio:
    * Paciente: CPF
    * Médico: CRM
    * Administrador: Código de inscrição
  * Senha exclusiva
  * Contexto de autenticação independente
* E-mails podem ser alterados pelo próprio usuário ou por um administrador
  * Toda alteração de e-mail exige **nova confirmação**
  * Caso a confirmação não seja realizada, o sistema mantém o e-mail anterior
* Nenhuma senha pode ser visualizada por outro usuário
* Todo cadastro, edição, bloqueio ou cancelamento gera **registro de auditoria**
* Um mesmo CPF pode possuir **múltiplos perfis**
* Cada perfil possui:
  * Credenciais próprias
  * Contexto de autenticação independente
* Nenhuma senha pode ser visualizada por outro usuário
* Todo cadastro e alteração relevante gera **registro de auditoria**

---

## 6. Dependências com Outras Funcionalidades

* Autenticação e autorização
* Confirmação de e-mail
* Auditoria e logs
* Gestão de perfis e permissões

Esta wiki deve ser mantida atualizada sempre que houver alteração nas regras de cadastro de usuários.
