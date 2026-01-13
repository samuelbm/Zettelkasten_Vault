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
  const file = app.vault.getAbstractFileByPath(filePath);

  if (!file) {
    await app.vault.create(filePath, content);
    return true;   // file was created
  }

  return false;    // file already existed
}

// Date
const dateStr = tp.date.now("YYYY-MM-DD"); 
const year = tp.date.now("YYYY"); 
const month = tp.date.now("MMMM"); 
const monthIndex = tp.date.now("MM");
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
const baseFolderPath = `00 - Daily Notes`;
const yearsFolderPath = `${baseFolderPath}/Years`;
const yearFolderPath = `${yearsFolderPath}/${year}`;
const monthFolderPath = `${yearFolderPath}/${monthStr}`;

//File Name
const yearsMocFileName = `Daily Notes MOC`;
const yearMocFileName = `Daily Notes ${year} MOC`;
const monthMocFileName = `Daily Notes ${year} ${month} MOC`;
const dailyNoteFileName = `Daily ${dateStr}`;

//Create File Path 
const yearsMocFilePath = `${yearsFolderPath}/${yearsMocFileName}.md`;
const yearMocFilePath =`${yearFolderPath}/${yearMocFileName}.md`;
const monthMocFilePath = `${monthFolderPath}/${monthMocFileName}.md`;
const dailyNoteFilePath = `${monthFolderPath}/${dailyNoteFileName}`;

// Create folder if needed
await ensureFolderExists(`${baseFolderPath}`);
await ensureFolderExists(`${yearsFolderPath}`);
await ensureFolderExists(`${yearFolderPath}`);
await ensureFolderExists(`${monthFolderPath}`);

// Move the note
await tp.file.move(`${dailyNoteFilePath}`);

//Create Moc Files if needed
const wasYearsMocFileCreated =  await ensureFileExists(`${yearsMocFilePath}`);
const wasYearMocCreated = await ensureFileExists(`${yearMocFilePath}`);
const wasMonthMocFileCreated = await ensureFileExists(`${monthMocFilePath}`);

// Add link to MOC Files
await appendToFile(`${yearsMocFilePath}`, `[[${yearMocFileName}]]`);
await appendToFile(`${yearMocFilePath}`, `[[${monthMocFileName}]]`);
await appendToFile(`${monthMocFilePath}`, `[[${dailyNoteFileName}]]`); 

//Add Dataview and Dataview MOC
const dataviewFolderPath = `${baseFolderPath}/Dataview`;
await ensureFolderExists(`${dataviewFolderPath}`);

const dataviewProudFileName = `Proud`;
const dataviewProudFilePath = `${dataviewFolderPath}/${dataviewProudFileName}.md`;
const wasProudFileCreated = await ensureFileExists(`${dataviewProudFilePath}`);
if (wasYearMocCreated) {
	const scriptProud = `\`\`\`dataviewjs
const folder = "00 - Daily Notes";
const heading = "Gratitude & Pride";
const yearFilter = ${year};

let pages = dv.pages("${folder}")
  .filter(p => p.file.name.includes(yearFilter))
  .sort(p => p.file.name, 'desc');

let allBullets = [];

for (let p of pages) {
  const content = await dv.io.load(p.file.path);
  const lines = content.split("\\n");

  let inSection = false;
  for (let line of lines) {
	if (line.trim() === \`# \${heading}\`) {
	  inSection = true;
	  continue;
	}
	if (inSection) {
	  if (line.startsWith("#")) break;
	  if (line.trim().startsWith("*") || line.trim().startsWith("-")) {
		allBullets.push(\`(\${p.file.name}) \${line.replace(/^(\\*|-)\\s*/, "")}\`);
	  }
	}
  }
}

dv.list(allBullets);
\`\`\``;
	await appendToFile(`${dataviewProudFilePath}`, `# ${year}`);
	await appendToFile(`${dataviewProudFilePath}`, `${scriptProud}`); 
}
 
const dataviewMocFileName = `Daily Notes Dataview MOC`;
const dataviewMocFilePath = `${dataviewFolderPath}/${dataviewMocFileName}.md`;
const wasDataviewMocFileCreated = await ensureFileExists(`${dataviewMocFilePath}`);
await appendToFile(`${dataviewMocFilePath}`, `[[${dataviewProudFileName}]]`); 
%>