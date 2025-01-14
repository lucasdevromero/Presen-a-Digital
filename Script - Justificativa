  // Função para mostrar o formulário no Google Sheets
function doGet() {
  return HtmlService.createHtmlOutputFromFile('formulario');
}

function processJustificativa(gestor, colaborador, matricula, data, justificativa, presenca) {
  if (!gestor || !colaborador || !matricula || !data || gestor.trim() === "" || colaborador.trim() === "" || matricula.trim() === "") {
    Logger.log("Erro: Um ou mais parâmetros estão inválidos.");
    return { success: false, message: "Dados incompletos. Verifique os parâmetros." };
  }

  const ss = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1OWGerTtlEXH-wXOYoxmYyxxXc3Yu8XyS4Arpo9dbSm8/edit?gid=678987279#gid=678987279");
  const respostasSheet = ss.getSheetByName("respostas");

  let formattedData = new Date(data);
  formattedData.setDate(formattedData.getDate() + 1); // Ajusta a data para o dia correto

  const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
  const formattedDate = formattedData.toLocaleDateString('pt-BR', options);
  const chavePrimaria = gestor.trim() + "-" + colaborador.trim() + "-" + matricula.trim() + "-" + formattedDate;

  Logger.log("Chave primária gerada: " + chavePrimaria);  // Log para verificar a chave primária gerada

  // Procura o código primário na planilha "respostas"
  const respostasData = respostasSheet.getDataRange().getValues();
  let foundRow = -1;

  for (let i = 0; i < respostasData.length; i++) {
    const row = respostasData[i];
    const rowChavePrimaria = row[0];

    Logger.log("Comparando chave primária: " + chavePrimaria + " com: " + rowChavePrimaria);  // Log de comparação

    if (rowChavePrimaria === chavePrimaria) {
      foundRow = i + 1; // Linha onde está o código primário
      break;
    }
  }

  Logger.log("Linha encontrada: " + foundRow);

  if (foundRow === -1) {
    return { success: false, message: "Não foi possível encontrar os dados correspondentes." };
  }

  // Armazenar presença e justificativa pendentes nas colunas K (Presença Pendente) e L (Justificativa Pendente)
  const presencePendingColumn = 11; // Coluna K para Presença Pendente
  const justificationPendingColumn = 12; // Coluna L para Justificativa Pendente
  const approvalColumn = 9; // Coluna I para Status (Pendente de Aprovação ou Aprovado)

  // Colocar dados nas colunas K (Presença Pendente) e L (Justificativa Pendente)
  respostasSheet.getRange(foundRow, presencePendingColumn).setValue(presenca);
  respostasSheet.getRange(foundRow, justificationPendingColumn).setValue(justificativa);

  // Marcar a coluna I como "Pendente de Aprovação"
  respostasSheet.getRange(foundRow, approvalColumn).setValue("Pendente de Aprovação");

  return { success: true, message: "Presença e Justificativa aguardando aprovação." };
}

// Função para buscar gestores com base no turno
function getGestoresPorTurno(turno) {
  const ssBaseHC = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1OWGerTtlEXH-wXOYoxmYyxxXc3Yu8XyS4Arpo9dbSm8/edit"); // Planilha externa
  const baseSheet = ssBaseHC.getSheetByName("Base HC");
  const baseData = baseSheet.getDataRange().getValues();
  
  let gestores = [];
  for (let i = 1; i < baseData.length; i++) {
    if (baseData[i][3] === turno && gestores.indexOf(baseData[i][0]) === -1) {
      gestores.push(baseData[i][0]); // Adiciona o gestor único
    }
  }
  return gestores;
}

// Função para buscar colaboradores por gestor e turno
function getColaboradoresPorGestorETurno(gestor, turno) {
  const ssBaseHC = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1OWGerTtlEXH-wXOYoxmYyxxXc3Yu8XyS4Arpo9dbSm8/edit"); // Planilha externa
  const baseSheet = ssBaseHC.getSheetByName("Base HC");
  const baseData = baseSheet.getDataRange().getValues();
  
  let colaboradores = [];
  for (let i = 1; i < baseData.length; i++) {
    if (baseData[i][0] === gestor && baseData[i][3] === turno && colaboradores.indexOf(baseData[i][1]) === -1) {
      colaboradores.push(baseData[i][1]); // Adiciona colaborador único
    }
  }
  return colaboradores;
}

function getMatriculasPorGestorColaboradorETurno(gestor, colaborador, turno) {
  const ssBaseHC = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1OWGerTtlEXH-wXOYoxmYyxxXc3Yu8XyS4Arpo9dbSm8/edit"); // Planilha externa
  const baseSheet = ssBaseHC.getSheetByName("Base HC");
  const baseData = baseSheet.getDataRange().getValues();

  let matriculas = [];
  for (let i = 1; i < baseData.length; i++) {
    if (baseData[i][0] === gestor && baseData[i][1] === colaborador && baseData[i][3] === turno) {
      matriculas.push(baseData[i][2]); // Adiciona matrícula
    }
  }
  return matriculas; // Adicionando o retorno
}

function processApproval() {
  const ss = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1OWGerTtlEXH-wXOYoxmYyxxXc3Yu8XyS4Arpo9dbSm8/edit?gid=678987279#gid=678987279");
  const respostasSheet = ss.getSheetByName("respostas");

  const data = respostasSheet.getDataRange().getValues();
  
  // Iterar pelas linhas para verificar o status de aprovação
  for (let i = 1; i < data.length; i++) { // Começar de 1 para pular o cabeçalho
    const row = data[i];
    const approvalStatus = row[8]; // Coluna I - Status (Pendente de Aprovação / Aprovado)
    const presencePending = row[10]; // Coluna K - Presença Pendente
    const justificationPending = row[11]; // Coluna L - Justificativa Pendente
    const presenceMarked = row[9]; // Coluna J - X ou vazio (Marca a presença)
    const approvalCell = respostasSheet.getRange(i + 1, 9); // Coluna I (Aprovado/Pendente)
    const presenceCell = respostasSheet.getRange(i + 1, 6); // Coluna F (Presença)
    const justificationCell = respostasSheet.getRange(i + 1, 7); // Coluna G (Justificativa)
    const presenceCheckCell = respostasSheet.getRange(i + 1, 10); // Coluna J (X ou vazio)

    // Se a presença foi marcada como "X" na coluna J e o status estiver "Pendente de Aprovação"
    if (presenceMarked === "X" && approvalStatus !== "Aprovado") {
      // Transferir os dados de K (Presença Pendente) e L (Justificativa Pendente) para F e G
      presenceCell.setValue(presencePending);
      justificationCell.setValue(justificationPending);

      // Marcar a presença como "X" na coluna J
      presenceCheckCell.setValue("X");

      // Limpar as colunas de pendente (K e L) após a transferência para F e G
      respostasSheet.getRange(i + 1, 11).setValue('');  // Limpar a coluna K (Presença Pendente)
      respostasSheet.getRange(i + 1, 12).setValue('');  // Limpar a coluna L (Justificativa Pendente)

      // Atualizar a coluna I para "Aprovado"
      approvalCell.setValue("Aprovado");

      // Apagar o "X" da coluna J após aprovar
      presenceCheckCell.setValue('');  // Limpar o "X" na coluna J
    }
  }
}


function clearPresenceColumn(gestor, colaborador, matricula, data) {
  const ss = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1OWGerTtlEXH-wXOYoxmYyxxXc3Yu8XyS4Arpo9dbSm8/edit?gid=678987279#gid=678987279");
  const respostasSheet = ss.getSheetByName("respostas");

  let formattedData = new Date(data);
  formattedData.setDate(formattedData.getDate() + 1); // Ajusta a data para o dia correto

  const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
  const formattedDate = formattedData.toLocaleDateString('pt-BR', options);
  const chavePrimaria = gestor.trim() + "-" + colaborador.trim() + "-" + matricula.trim() + "-" + formattedDate;

  // Procura o código primário na planilha "respostas"
  const respostasData = respostasSheet.getDataRange().getValues();
  let foundRow = -1;

  for (let i = 0; i < respostasData.length; i++) {
    const row = respostasData[i];
    const rowChavePrimaria = row[0]; // A chave primária está na coluna A (índice 0)

    if (rowChavePrimaria === chavePrimaria) {
      foundRow = i + 1; // Linha onde está o código primário
      break;
    }
  }

  if (foundRow === -1) {
    return { success: false, message: "Não foi possível encontrar os dados correspondentes." };
  }

  // Limpar a célula da coluna "J" (Presença) da linha correspondente
  respostasSheet.getRange(foundRow, 10).setValue(''); // Limpar a célula na coluna J (índice 10)

  return { success: true, message: "Presença limpa com sucesso." };
}


// Função para validar o login
function checkLogin(username, password) {
  var ss = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1OWGerTtlEXH-wXOYoxmYyxxXc3Yu8XyS4Arpo9dbSm8/edit?gid=678987279#gid=678987279"); // URL da planilha
  var sheet = ss.getSheetByName("users");
  var data = sheet.getRange(2, 1, sheet.getLastRow() - 1, 2).getValues();
  
  for (var i = 0; i < data.length; i++) {
    if (data[i][0] === username && data[i][1] === password) {
      return "success";
    }
  }
  return "failure";
}

