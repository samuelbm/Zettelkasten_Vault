
# 2026
```dataviewjs
console.log("test213");
const folder = "00 - Daily Notes";
const heading = "Gratitude & Pride";
const yearFilter = "2026";

let pages = dv.pages(folder)
  .filter(p => p.file.name.includes(yearFilter))
  .sort(p => p.file.name, 'desc');

console.log(`${folder}`);
console.log(`${pages}`);

let allBullets = [];

for (let p of pages) {
  console.log(p);
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
        allBullets.push(
          `(${p.file.name}) ${line.replace(/^(\*|-)\s*/, "")}`
        );
      }
    }
  }
}

dv.list(allBullets);
```