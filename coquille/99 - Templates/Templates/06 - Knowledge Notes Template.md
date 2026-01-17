---
type: knowledge
id: <% tp.date.now("YYYYMMDDHHmmss") %>
MOC: "[[Knowledge Notes MOC]]"
tags:
  - "#knowledge"
  - "#notes"
---
# Context / Problem

# Notes

# References / Sources
* 

<%* 
// template 1
async function ensureFileExists(filePath, content = "") {
  const file = app.vault.getAbstractFileByPath(filePath);

  if (!file) {
    await app.vault.create(filePath, content);
    return true;   // file was created
  }

  return false;    // file already existed
}

const knowledgeNotesBasePath = "06 - Knowledge Notes Template";
const knowledgeNotesMocFilePath = `${knowledgeNotesBasePath}`;
const knowledgeNotesFilePath = `${knowledgeNotesBasePath}/Knowledge Notes`
const wasKnowledgeMocFileCreated =  await ensureFileExists(`${knowledgeNotesMocFilePath}`);

%>