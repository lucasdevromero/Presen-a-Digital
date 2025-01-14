# Presença Digital - Sistema de Registro de Presença de Colaboradores

Este projeto é um sistema digital de registro de presença para colaboradores, desenvolvido usando Google Apps Script e HTML/CSS para o front-end. O sistema permite o login de usuários e o envio de informações sobre a presença dos colaboradores, incluindo justificativas para ausências. Abaixo estão as principais funcionalidades e componentes.

## Backend (Google Apps Script)

- **Funções de Login**: Valida o login de usuários com base em um banco de dados no Google Sheets.
- **Gestão de Turnos e Líderes**: Permite selecionar líderes e colaboradores de acordo com o turno (1º, 2º, 3º, Administrativo).
- **Validação de Presença**: Valida se o colaborador está presente ou ausente e se justificativas foram fornecidas adequadamente.
- **Envio de Respostas**: Envia as informações de presença para uma planilha de respostas no Google Sheets, incluindo o nome do líder, turno, matrícula, status de presença e justificativas.

## Frontend (HTML/CSS/JavaScript)

- **Tela de Login**: Permite que o usuário insira suas credenciais para acessar o sistema.
- **Formulário de Presença**: Permite selecionar o turno e o líder, exibir a lista de colaboradores e registrar a presença (com justificativas para ausências).
- **Interatividade**: O sistema usa seletores dinâmicos para exibir líderes e colaboradores com base nas escolhas de turno e líder. Justificativas para ausências são exibidas quando o status de presença é "Ausente".
- **Feedback Visual**: Exibição de mensagens de carregamento e barras de progresso para indicar o andamento das ações de login e envio de dados.
- **Responsividade**: O layout foi projetado para ser responsivo, se ajustando bem a diferentes tamanhos de tela.

## Funcionalidades Importantes

- **Login e Autenticação**: Protege o acesso ao sistema com login baseado em dados armazenados no Google Sheets.
- **Seleção de Turno e Líder**: O sistema permite que os usuários escolham um turno e, dependendo da escolha, exibe os líderes correspondentes.
- **Registro de Presença**: Os colaboradores podem ser marcados como "Presente" ou "Ausente", e uma justificativa é solicitada caso a presença seja registrada como ausente.
- **Processo de Envio**: Após preencher o formulário de presença, os dados são enviados para uma planilha no Google Sheets para registro e controle.

## Requisitos

- Conta do Google para acessar e editar o Google Sheets.
- O sistema utiliza o Google Apps Script para integração entre o front-end e o Google Sheets.

---

Este repositório contém o código para o backend (Apps Script) e o frontend (HTML, CSS, JavaScript) que juntos formam a aplicação de presença digital.

---

# Sistema de Solicitação de Justificativa de Presença com Login e Integração Google Sheets

## Descrição

Este projeto implementa uma aplicação web de login e um formulário de solicitação de justificativa de presença para colaboradores. A aplicação interage com o Google Sheets via Google Apps Script para registrar e processar as justificativas, além de permitir a validação de login, campos dinâmicos e envio de dados ao backend.

## Estrutura do Projeto

- **Página de Login**: Contém campos para o usuário e senha, juntamente com uma barra de progresso para simular o processo de login.
- **Formulário de Justificativa de Presença**: Exibe campos para selecionar turno, gestor, colaborador, matrícula, data e presença, além de um campo de justificativa que é habilitado se o colaborador estiver ausente.

## Funcionalidades

### Tela de Login

- Validação de credenciais com barra de progresso.
- Substituição da tela de login pelo formulário de justificativa em caso de sucesso.

### Formulário de Justificativa

- Campos dinâmicos com base na seleção de turno, gestor e colaborador.
- Habilitação de campos de data, presença e justificativa conforme as escolhas do usuário.
- Envio de dados para o backend para processamento e armazenamento no Google Sheets.

## Tecnologias Utilizadas

- **Frontend**: HTML, CSS, JavaScript
- **Backend**: Google Apps Script, Google Sheets

## Como Funciona

1. O usuário faz login informando o nome de usuário e a senha.
2. Após login bem-sucedido, o formulário de justificativa de presença é exibido.
3. O usuário preenche os campos do formulário (turno, gestor, colaborador, matrícula, presença, justificativa).
4. Ao submeter o formulário, os dados são enviados para o backend, que os processa e os armazena na planilha do Google Sheets.

## Como Rodar o Projeto

1. Clone este repositório para sua máquina local.
2. Configure o Google Apps Script e a planilha do Google Sheets conforme necessário.
3. Abra o arquivo `index.html` para visualizar a aplicação localmente.
4. Conecte o backend utilizando o Google Apps Script.
