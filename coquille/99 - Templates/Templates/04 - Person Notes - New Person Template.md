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
```dataviewjs
const folder = "04 - Person Notes/People Notes";
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





<%*
const dateStr = tp.date.now("YYYY-MM-DD-HH-mm-ss");  

// Folder to move note into
const folderPath = "04 - Person Notes";

// Prompt for note title
const noteName = "Person Note " + dateStr;

// Move the note
await tp.file.move(`${folderPath}/People/${noteName}`);
%>

