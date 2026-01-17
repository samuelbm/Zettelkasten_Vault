---
type: permanent
id: <% tp.date.now("YYYYMMDDHHmmss") %>
tags:
  - "#permanent"
status: draft
---
# Context / Problem
* 

# Notes
* 

# References / Sources
* 
<%*
 // Folder to move note into
const folderPath = "03 - Permanent Notes";

// Prompt for note title
const noteName = "Permanent Note";

// Move the note
await tp.file.move(`${folderPath}/${noteName}`);
%>