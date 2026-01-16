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