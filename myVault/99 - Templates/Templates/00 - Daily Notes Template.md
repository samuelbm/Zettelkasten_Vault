# Allo

<%*
// Templates
//Templates 1
async function ensureFolderExists(path) {
  const folder = app.vault.getAbstractFileByPath(path);
  if (!folder) {
    await app.vault.createFolder(path);
  }
}

//Templates 2
async function appendToFile(filePath, content) {
  // Get the file object
  const file = app.vault.getAbstractFileByPath(filePath);
  
  if (!file) {
    console.error(`File not found: ${filePath}`);
    return;
  }
  
  // Read existing content
  const existingContent = await app.vault.read(file);
  
  // Append new content
  const newContent = existingContent + "\n" + content;
  
  // Write back
  await app.vault.modify(file, newContent);
}

// Date
const dateStr = "2026-01-02"//tp.date.now("YYYY-MM-DD"); 
const year = "2026"//tp.date.now("YYYY"); 
const monthText = "January"//tp.date.now("MMMM"); 
const monthIndex = "01"//tp.date.now("MM");
const monthStr = `${monthIndex} - ${monthText}`; 

// Create folder path dynamically
const basePath = `00 - Daily Notes`;
const yearsFolderPath = `${basePath}/Years`;
const yearsMocFilePath = `${yearsFolderPath}/Daily Notes Moc`;

const yearFolderPath = `${yearsFolderPath}/${year}`;
const yearMocFilePath =`${yearFolderPath}/Daily Notes 2026 MOC`;

const monthFolderPath = `${yearFolderPath}/${monthStr}`;
const monthFilePath = `${monthFolderPath}/Daily Notes 2026 ${monthText} MOC`;

const noteName = "Daily " + dateStr;
const noteFilePath = `${yearFolderPath}/${noteName}`;

// Create folder if needed
await ensureFolderExists(yearFolderPath);
await ensureFolderExists(monthFolderPath);

// Move the note
await tp.file.move(`${noteFilePath}`);

// Add link to MOCs
await appendToFile(`${yearsMocFilePath}`, `[[${yearFolderPath}]]`);
await appendToFile(`${yearMocFilePath}`, `[[${monthFilePath}]]`);
await appendToFile(`${monthFilePath}`, `[[${noteFilePath}]]`);
%>