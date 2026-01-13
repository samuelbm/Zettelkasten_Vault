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

//Template 3
async function ensureFileExists(filePath, content = "") {
  // Check if file exists
  let file = app.vault.getAbstractFileByPath(filePath);
  
  if (!file) {
    // Create the file with optional initial content
    await app.vault.create(filePath, content);
  }
}

// Date
const dateStr = "2026-02-05";//tp.date.now("YYYY-MM-DD"); 
const year = "2026";//tp.date.now("YYYY"); 
const month = "February";//tp.date.now("MMMM"); 
const monthIndex = "02";//tp.date.now("MM");
const monthStr = `${monthIndex} - ${month} - ${year}`; 

// File Hierarchy
//00 - Daily Notes
//----->Years
//----------->2026
//----------------->January
//----------------------->Daily 2026-01-01
//----------------------->Daily Notes 2026 January MOC
//----------------->Daily Notes 2026 MOC
//----------->Daily Notes MOC

//Create Folder Path 
const basePath = `00 - Daily Notes`;
const yearsFolderPath = `${basePath}/Years`;
const yearFolderPath = `${yearsFolderPath}/${year}`;
const monthFolderPath = `${yearFolderPath}/${monthStr}`;

//

//Create File Path 
const yearsMocFilePath = `${yearsFolderPath}/Daily Notes Moc.md`;
const yearMocFilePath =`${yearFolderPath}/Daily Notes ${year} MOC.md`;
const monthMocFilePath = `${monthFolderPath}/Daily Notes ${year} ${month} MOC.md`;
const dailyNoteFilePath = `${monthFolderPath}/Daily ${dateStr}`;

// Create folder if needed
await ensureFolderExists(yearsFolderPath);
await ensureFolderExists(yearFolderPath);
await ensureFolderExists(monthFolderPath);

// Move the note
await tp.file.move(`${dailyNoteFilePath}`);

//Create Moc Files if needed
await ensureFileExists(`${yearsMocFilePath}`);
await ensureFileExists(`${yearMocFilePath}`);
await ensureFileExists(`${monthMocFilePath}`);

// Add link to MOC Files
await appendToFile(`${yearsMocFilePath}`, `[[${yearMocFilePath}]]`);
await appendToFile(`${yearMocFilePath}`, `[[${monthMocFilePath}]]`);
await appendToFile(`${monthMocFilePath}`, `[[${noteFilePath}]]`); 
%>