---
type: content
id: <% tp.date.now("YYYYMMDDHHmmss") %>
tags:
  - "#notes"
  - content
---
# Notes
* 
<%*
const dateStr = tp.date.now("YYYY-MM-DD");  

// Folder to move note into
const folderPath = "02 - Literature Notes";

// Prompt for note title
const noteName = "Literature Note " + dateStr;

// Move the note
await tp.file.move(`${folderPath}/Work Notes/${noteName}`);
%>