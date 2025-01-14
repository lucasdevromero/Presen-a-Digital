// Função que carrega a página solicitada (login ou presença)
function doGet() {
  return HtmlService.createHtmlOutputFromFile('index');
}

// Função para validar o login
function checkLogin(username, password) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Users');
  var data = sheet.getRange(2, 1, sheet.getLastRow() - 1, 2).getValues();
  
  for (var i = 0; i < data.length; i++) {
    if (data[i][0] === username && data[i][1] === password) {
      return "success";
    }
  }
  return "failure";
}

// Função para obter os líderes com base no turno selecionado
function getLeadersByTurno(turno) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('BASE HC');
  const data = sheet.getRange(2, 1, sheet.getLastRow() - 1, 4).getValues(); // Considerando colunas A (líderes) e D (turnos)

  const leaders = new Set();

  // Filtra os líderes com base no turno selecionado (coluna D)
  data.forEach(row => {
    if (row[3] === turno) { // Verifica a coluna 4 (D) para o turno
      leaders.add(row[0]); // Adiciona o líder da coluna 1 (A)
    }
  });

  return Array.from(leaders);  // Retorna os líderes como um array
}

// Função para obter os colaboradores de um líder com as matrículas
function getCollaborators(leader) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('BASE HC');
  const data = sheet.getRange(2, 1, sheet.getLastRow() - 1, 3).getValues(); // Inclui a matrícula (coluna C)
  const collaborators = [];

  data.forEach(row => {
    if (row[0] === leader) {
      collaborators.push({ name: row[1], matricula: row[2] }); // Retorna nome e matrícula
    }
  });

  return collaborators;
}

// Função para validar a presença
function validatePresenceForm(status, reason) {
  if (status === "Ausente" && !reason) {
    return "Você deve fornecer uma justificativa para os colaboradores ausentes.";
  }
  return null;
}

// Função para enviar respostas de presença para a planilha
function submitResponses(leader, names, matriculas, presenceStatus, reasons, dateTime, turno) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Respostas');
  const responses = [];
  let errorMessage = null;

  // Validação e adição das respostas
  for (let i = 0; i < names.length; i++) {
    const status = presenceStatus[i];
    const reason = reasons[i];
    errorMessage = validatePresenceForm(status, reason);
    if (errorMessage) {
      return { success: false, message: errorMessage };
    }

    // Padronizando a data para o formato "dd/MM/yyyy"
    const dateTime = new Date();
    const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
    const formattedDate = dateTime.toLocaleDateString('pt-BR', options); // 'pt-BR' para português do Brasil

    const primaryKey = leader + "-" + matriculas[i] + "-" + names[i] + "-" + formattedDate;

    // Adicionando a chave primária e os dados de presença
    responses.push([primaryKey, leader, turno, matriculas[i], names[i], status, reason || "", dateTime]);
  }

  try {
    if (responses.length > 0) {
      const range = sheet.getRange(sheet.getLastRow() + 1, 1, responses.length, responses[0].length);
      range.setValues(responses);
      return { success: true, message: 'Respostas enviadas com sucesso.' };
    } else {
      return { success: false, message: 'Nenhuma resposta para enviar.' };
    }
  } catch (error) {
    Logger.log("Erro ao inserir dados na planilha:", error);
    return { success: false, message: "Erro ao salvar os dados na planilha." };
  }
}
