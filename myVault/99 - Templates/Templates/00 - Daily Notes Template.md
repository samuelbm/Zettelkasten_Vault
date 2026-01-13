
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
const dateStr = "2026-01-01"//tp.date.now("YYYY-MM-DD"); 
const year = "2026"//tp.date.now("YYYY"); 
const month = "January"//tp.date.now("MMMM"); 

// Create folder path dynamically
const basePath = `00 - Daily Notes`;
const yearsFolderPath = `${basePath}/Years`;
const yearsMocFilePath = `${yearsFolderPath}/Daily Notes Moc`;

const yearFolderPath = `${yearsFolderPath}/${year}`;
const yearMocFilePath =`${yearFolderPath}/Daily Notes 2026 MOC`;

const monthFolderPath = `${yearFolderPath}/${month}`;
const monthMocFilePath = 

const noteName = "Daily " + dateStr;
const noteFilePath = `${yearFolderPath}/${noteName}`;

// Create folder if needed
await ensureFolderExists(yearFolderPath);
await ensureFolderExists(monthFolderPath);

// Move the note
await tp.file.move(`${folderPath}/${noteName}`);

// Add link to MOCs
await appendToFile("Daily/Years/Daily Notes 2026 MOC.md", "[[Daily 2026-01-13]]");

%>