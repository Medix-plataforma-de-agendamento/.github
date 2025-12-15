# Cadastro de Usuários

## 1. Visão Geral

Esta wiki descreve o **fluxo de cadastro de usuários no Medix**, separando claramente as regras e comportamentos por **perfil** (Paciente, Médico e Administrador).

Antes de detalhar os fluxos específicos, são apresentadas as **informações comuns e específicas** que podem ser registradas no sistema, bem como suas regras de obrigatoriedade e unicidade.

> **Importante:** neste documento não são consideradas decisões de arquitetura, modelagem de dados ou estrutura técnica. O foco é **funcional e de negócio**.

---

## 2. Informações Cadastrais Comuns


### 2.1 Dados pessoais (todos os perfis)

| Campo              | Paciente | Médico | Administrador | Obrigatório | Único |Observação                                   |
| ------------------ | -------- | ------ | ------------- | ----------- | ----- |
| Nome               | ✔️       | ✔️     | ✔️            | ✔️          | ❌     |  |
| CPF                | ✔️       | ✔️     | ✔️            | ✔️          | ✔️    |  |
| Data de nascimento | ✔️       | ✔️     | ✔️            | ✔️          | ❌     |  |
| Gênero             | ✔️       | ✔️     | ✔️            | ✔️          | ❌     |  |
| RG                 | ✔️       | ✔️     | ✔️            | ❌           | ❌     |  |
| Senha              | ✔️       | ✔️     | ✔️            | ✔️          | ❌     |  |
| E-mail pessoal     | ✔️       | ✔️     | ✔️            | ✔️          | ❌     |  |
| Celular            | ✔️       | ✔️     | ✔️            | ✔️          | ❌     |  |
| CRM                           | ❌        | ✔️     | ❌             | ✔️          | ✔️    | Identificador profissional do médico         |
| Validade do CRM               | ❌        | ✔️     | ❌             | ✔️          | ❌     | Necessário para validação profissional       |
| Especialidades                | ❌        | ✔️     | ❌             | ✔️          | ❌     | Uma ou mais especialidades                   |
| E-mail corporativo            | ❌        | ✔️     | ✔️            | ✔️          | ❌     | Comunicação institucional                    |
| Senha do perfil médico        | ❌        | ✔️     | ❌             | ✔️          | ❌     | Credencial exclusiva do perfil médico        |
| Código de inscrição           | ❌        | ❌      | ✔️            | ✔️          | ✔️    | Identificador interno do administrador       |
| Senha do perfil administrador | ❌        | ❌      | ✔️            | ✔️          | ❌     | Credencial exclusiva do perfil administrador |

### 2.2 Endereço (todos os perfis)

| Campo          | Obrigatório |
| -------------- | ----------- |
| Logradouro     | ✔️          |
| Número da casa | ✔️          |
| CEP            | ✔️          |
| Complemento    | ❌           |
| Bairro         | ❌           |
| Cidade         | ✔️          |
| Estado         | ✔️          |

| Campo           | Obrigatório |
| --------------- | ----------- |
| Logradouro*     | ✔️          |
| Número da casa* | ✔️          |
| CEP*            | ✔️          |
| Complemento     | ❌           |
| Bairro          | ❌           |
| Cidade*         | ✔️          |
| Estado*         | ✔️          |

---

## 3. Fluxo de Cadastro por Perfil

## 3.1 Cadastro de Paciente

* Usuário não autenticado acessa a tela de **Entrar / Login**
* É apresentada a opção **“Realizar cadastro”**
* São informados todos os dados obrigatórios para cadastro de paciente
* O sistema envia um **e-mail de confirmação** com código de liberação
* Após a confirmação, o acesso ao sistema é liberado no perfil de paciente

---

## 3.2 Cadastro de Médico

### 3.2.1 – Usuário com cadastro prévio
* O cadastro pode ser iniciado por um usuário já autenticado como paciente ou por um usuário não autenticado
* Quando iniciado por paciente autenticado, são solicitadas apenas as informações específicas de médico
* O sistema envia e-mails de confirmação para os endereços corporativo informado
* O perfil de paciente é liberado após confirmação
* O perfil de médico permanece **em análise administrativa** até aprovação

---

### 3.2.2 – Usuário sem cadastro prévio
* Usuário não autenticado acessa a tela de **Entrar / Login**
* É apresentada a opção **“Realizar cadastro”**
* São informados todos os dados obrigatórios para cadastro de paciente
* O sistema envia um **e-mail de confirmação** com código de liberação
* Após a confirmação, o acesso ao sistema é liberado no perfil de paciente
* O usuário e marca a opção **“Cadastrar como médico”**


#### Fluxo

* Usuário não autenticado acessa **Entrar / Login**
* Seleciona **“Realizar cadastro”**
* Preenche:

  * Dados obrigatórios de paciente
  * Marca a opção **“Cadastrar também como médico”**
* O sistema exibe os campos específicos do perfil médico

#### Confirmação

* São enviados e-mails de confirmação para:

  * E-mail pessoal
  * E-mail corporativo

#### Estado do perfil

* Perfil de paciente: **liberado após confirmação**
* Perfil de médico: **em análise pelo administrador**

> Enquanto o perfil médico estiver em análise, **não é possível acessar o sistema como médico**.

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
