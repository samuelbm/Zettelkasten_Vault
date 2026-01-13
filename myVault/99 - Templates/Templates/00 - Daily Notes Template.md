
# Allo





<%*
// Date
const dateStr = tp.date.now("YYYY-MM-DD"); 
const year = tp.date.now("YYYY"); 
const month = tp.date.now("MMMM"); 

// Create folder path dynamically
const basePath = `00 - Daily Notes`;
const yearsFolderPath = `${basePath}/Years`;
const yearFolderPath = `${yearsFolderPath}/${year}`;
const monthFolderPath = `${yearFolderPath}/${month}`;
const noteName = "Daily " + dateStr;
const noteFilePath = `${yearFolderPath}/${noteName}`;

// Template
async function ensureFolderExists(path) {
  const folder = app.vault.getAbstractFileByPath(path);
  if (!folder) {
    await app.vault.createFolder(path);
  }
}

// Create folder if needed
await ensureFolderExists(yearFolderPath);
await ensureFolderExists(monthFolderPath);

// Move the note
await tp.file.move(`${folderPath}/${noteName}`);

// Add link to MOCs
//

%>