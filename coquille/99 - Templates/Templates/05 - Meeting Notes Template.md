---
type: meeting
id: <% tp.date.now("YYYYMMDDHHmmss") %>
status: draft
Project: TBD
duration: TBD
tags:
  - "#meeting"
---
# To-DO
- [ ] -

# Presences
* #SamuelBrinMarquis 

# Résumé


# Notes

<%*
const dateStr = tp.date.now("YYYY-MM-DD-HH-mm-ss");  

// Folder to move note into
const folderPath = "05 - Meeting Notes";

// Prompt for note title
const noteName = "Meeting Note " + dateStr;

// Move the note
await tp.file.move(`${folderPath}/${noteName}`);
%>

