
# 2025

```dataviewjs
const folder = "00 - Daily Notes";
const heading = "Gratitude & Pride";
const yearFilter = "2026"; // only show notes from this year
let pages = dv.pages(`"${folder}"`)
    .filter(p => p.file.name.includes(yearFilter)) // only notes containing 2025
    .sort(p => p.file.name, 'desc');
let allBullets = [];
for (let p of pages) {
    const content = await dv.io.load(p.file.path);
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
                allBullets.push(`(${p.file.name}) ${line.replace(/^(\*|-)\s*/, "")}`);
            }
        }
    }
}
dv.list(allBullets);
```
