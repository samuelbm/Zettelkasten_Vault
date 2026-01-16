---
type: Daily
id: <% tp.date.now("YYYYMMDDHHmmss") %>
tags:
  - daily
---
# Goals
<%*
// template 1
async function getIncompleteTasks(filePath, title) {
  // Get the file
  const file = app.vault.getAbstractFileByPath(filePath);
  
  if (!file) {
    console.error(`File not found: ${filePath}`);
    return [];
  }
  
  // Read the content
  const content = await app.vault.read(file);
  const lines = content.split("\n");
  
  let incompleteTasks = [];
  let inSection = false;
  
  for (let line of lines) {
    // Check if we found the title/heading
    if (line.trim().startsWith("#") && line.includes(title)) {
      inSection = true;
      continue;
    }
    
    // If we hit another heading, stop looking
    if (inSection && line.trim().startsWith("#")) {
      break;
    }
    
    // If we're in the right section, look for incomplete tasks
    if (inSection) {
      // Match incomplete tasks: - [ ] but not - [x] or - [X]
		if (line.includes("- [ ]")) {
			incompleteTasks.push(line.trim() + "\n");
		}
    }
  }
  
  return incompleteTasks;
}

// Example usage:
// const tasks = await getIncompleteTasks("path/to/file.md", "My Section");
// console.log(tasks);


const yesterdayYear = tp.date.now("YYYY", -1);
const yesterdayMonth = tp.date.now("MMMM", -1);
const yesterdayMonthIndex = tp.date.now("MM", -1);
const yesterdayDay = tp.date.now("DD", -1);
const yesterdayDailyNoteName = `Daily ${tp.date.now("YYYY-MM-DD", -1)}`;
const yesterdayMonthStr = `${yesterdayMonthIndex} - ${yesterdayMonth} - ${yesterdayYear}`;
const goals = await getIncompleteTasks(`00 - Daily Notes/Years/${yesterdayYear}/${yesterdayMonthStr}/${yesterdayDailyNoteName}.md`, "Goals");
tR += goals.join("");

const companyName = "__Work__";
%>
# Daily activity
- [ ] weigh-in
- [ ] meditation
- [ ] respiration
- [ ] miracle morning
- [ ] musculation
- [ ] cardio
# Gratitude & Pride

# Accomplishments


<%*
tR += `# ${__Work__}`;
tR += `## __Work__ activity log`;
 
const activityLog = await getIncompleteTasks(`00 - Daily Notes/Years/${yesterdayYear}/${yesterdayMonthStr}/${yesterdayDailyNoteName}.md`, "activity log");
tR += activityLog.join("");
%>
## __Work__ timesheet

## __Work__ vacation sickdays and personal days

## __Work__ things done for performance review

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
async function appendToBottomOfFile(filePath, content) {
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

//Template 4
async function appendToTopOfFile(filePath, content, position = 'top') {
  // Get the file object
  const file = app.vault.getAbstractFileByPath(filePath);
  
  if (!file) {
    console.error(`File not found: ${filePath}`);
    return;
  }
  
  // Read existing content
  const existingContent = await app.vault.read(file);
  
  // Append new content at top or bottom
  const newContent = position === 'top' 
    ? content + "\n" + existingContent
    : existingContent + "\n" + content;
  
  // Write back
  await app.vault.modify(file, newContent);
}

// Template 5
function dataviewScript(year, folderPath, heading) { 
	const baseScript = `
#  ${year}
\`\`\`dataviewjs
const folder = "${folderPath}";
const heading = "${heading}";
const yearFilter = "${year}"; // only show notes from this year
let pages = dv.pages(\`"\${folder}"\`)
    .filter(p => p.file.name.includes(yearFilter)) // only notes containing 2025
    .sort(p => p.file.name, 'desc');
let allBullets = [];
for (let p of pages) {
    const content = await dv.io.load(p.file.path);
    const lines = content.split("\\n");
    let inSection = false;
    for (let line of lines) {
		if (new RegExp(\`^#+\\\\s+\${heading}$\`).test(line.trim())) {
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
\`\`\`
`;
	return baseScript; 
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
if (wasYearMocCreated) {
	await appendToBottomOfFile(`${yearsMocFilePath}`, `[[${yearMocFileName}]]`); // only if year created
}
if (wasMonthMocFileCreated) {
	await appendToBottomOfFile(`${yearMocFilePath}`, `[[${monthMocFileName}]]`); // only if month created
}
await appendToBottomOfFile(`${monthMocFilePath}`, `[[${dailyNoteFileName}]]`); //always

//Add Dataview and Dataview MOC
const dataviewFolderPath = `${baseFolderPath}/Dataview`;
await ensureFolderExists(`${dataviewFolderPath}`);
//Proud
const dataviewProudFileName = `Proud`;
const dataviewProudFilePath = `${dataviewFolderPath}/${dataviewProudFileName}.md`;
const wasProudFileCreated = await ensureFileExists(`${dataviewProudFilePath}`);
const scriptProud = dataviewScript(year, "00 - Daily Notes", "Gratitude & Pride");
if (wasYearMocCreated) {
	await appendToTopOfFile(`${dataviewProudFilePath}`, `${scriptProud}`); 
}
//work things done
const dataviewWorkThingsDoneFileName = `${companyName} things done`;
const dataviewWorkThingsDonePath = `${dataviewFolderPath}/${dataviewWorkThingsDoneFileName}.md`;
const wasWorkThingsDoneFileCreated = await ensureFileExists(`${dataviewWorkThingsDonePath}`);
const scriptWorkThingsDone = dataviewScript(year, "00 - Daily Notes", `${companyName} things done for performance review`);
if (wasYearMocCreated) {
	await appendToTopOfFile(`${dataviewWorkThingsDonePath}`, `${scriptWorkThingsDone}`); 
}
//work timesheet 
const dataviewWorkTimesheetFileName = `${companyName} timesheet`;
const dataviewWorkTimesheetPath = `${dataviewFolderPath}/${dataviewWorkTimesheetFileName}.md`;
const wasWorkTimesheetFileCreated = await ensureFileExists(`${dataviewWorkTimesheetPath}`);
const scriptWorkTimesheet = dataviewScript(year, "00 - Daily Notes", `${companyName} timesheet`);
if (wasYearMocCreated) {
	await appendToTopOfFile(`${dataviewWorkTimesheetPath}`, `${scriptWorkTimesheet}`); 
}

//work vacation, sickdays and personal days
const dataviewWorkVacationFileName = `${companyName} vacation, sickdays and personal days`;
const dataviewWorkVacationPath = `${dataviewFolderPath}/${dataviewWorkVacationFileName}.md`;
const wasWorkVacationFileCreated = await ensureFileExists(`${dataviewWorkVacationPath}`);
const scriptWorkVacation = dataviewScript(year, "00 - Daily Notes", `${companyName} vacation sickdays and personal days`);
if (wasYearMocCreated) {
	await appendToTopOfFile(`${dataviewWorkVacationPath}`, `${scriptWorkVacation}`); 
}

//DataviewMoc 
const dataviewMocFileName = `Daily Notes Dataview MOC`;
const dataviewMocFilePath = `${dataviewFolderPath}/${dataviewMocFileName}.md`;
const wasDataviewMocFileCreated = await ensureFileExists(`${dataviewMocFilePath}`);
if (wasProudFileCreated)
{
	await appendToBottomOfFile(`${dataviewMocFilePath}`, `[[${dataviewProudFileName}]]`);
} 
if (wasWorkThingsDoneFileCreated)
{
	await appendToBottomOfFile(`${dataviewMocFilePath}`, `[[${dataviewWorkThingsDoneFileName}]]`);
}
if (wasWorkTimesheetFileCreated)
{
	await appendToBottomOfFile(`${dataviewMocFilePath}`, `[[${dataviewWorkTimesheetFileName}]]`);
}
if (wasWorkVacationFileCreated)
{
	await appendToBottomOfFile(`${dataviewMocFilePath}`, `[[${dataviewWorkVacationFileName}]]`);
}
%>