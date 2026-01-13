
# Allo





<%*
// Date
const dateStr = tp.date.now("YYYY-MM-DD"); 
const year = tp.date.now("YYYY"); 
const month = tp.date.now("MMMM"); 

// Create folder path dynamically
const basePath = `00 - Daily Notes`
const yearPath = 
const folderPath = `00 - Daily Notes/${year}/${month}`;

// New note name
const noteName = "Daily " + dateStr;

// Move the note
await tp.file.move(`${folderPath}/${noteName}`);
%>