---
type: person
Jobs:
hobbies:
birthdate:
partners:
Parents:
siblings:
children:
friends:
collegues:
authors:
id: <% tp.date.now("YYYYMMDDHHmmss") %>
tags:
  - "#person"
  - "#notes"
---
# Notes
* 
<%* 
const folderPath = "04 - Person Notes";
// Prompt for note title
const noteName = "Person Note";
// Move the note
await tp.file.move(`${folderPath}/People/${noteName}`);
%>
```dataviewjs
const folder = "04 - Person Notes/People";
const heading = "Things to remember";
const targetName = dv.current().file.name;
let pages = dv.pages(`"${folder}"`)
    .sort(p => p.file.name, 'desc');
let allBullets = [];
for (let p of pages) {
    // --- FILTER CHECK ---
    const content = await dv.io.load(p.file.path);
    // check [[Name]] anywhere in file
    const hasLink =
        content.includes(`[[${targetName}]]`) ||
        content.includes(`[[${targetName}|`);
    // check YAML frontmatter people: [Name, Other]
    const hasPeople =
        p.people &&
        Array.isArray(p.people) &&
        p.people.map(x => String(x).toLowerCase()).includes(targetName.toLowerCase());
    if (!hasLink && !hasPeople) continue;
    // --- END FILTER CHECK ---
    const lines = content.split("\n");
    let inSection = false;
    for (let line of lines) {
        if (line.trim() === `# ${heading}`) {
            inSection = true;
            continue;
        }
        if (inSection) {
            if (line.startsWith("#")) break;
            if (line.trim().startsWith("*") || line.trim().startsWith("-")) {
                allBullets.push(
                    `(${p.file.name}) ${line.replace(/^(\*|-)\s*/, "")}`
                );
            }
        }
    }
}
dv.list(allBullets);
```