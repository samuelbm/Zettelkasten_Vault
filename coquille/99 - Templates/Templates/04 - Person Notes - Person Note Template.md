<%*
const dateStr = tp.date.now("YYYY-MM-DD-HH-mm-ss");  

// Folder to move note into
const folderPath = "04 - Person Notes";

// Prompt for note title
const noteName = "Person Note " + dateStr;

// Move the note
await tp.file.move(`${folderPath}/People Notes/${noteName}`);
%>
# Things to remember