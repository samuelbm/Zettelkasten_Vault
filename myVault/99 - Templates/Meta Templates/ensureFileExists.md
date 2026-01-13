async function ensureFileExists(filePath, content = "") {
  // Check if file exists
  let file = app.vault.getAbstractFileByPath(filePath);
  
  if (!file) {
    // Create the file with optional initial content
    await app.vault.create(filePath, content);
  }
}
