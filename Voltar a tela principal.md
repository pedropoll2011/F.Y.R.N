---
type: wiki-home
cssclasses:
  - wiki-character
  - wiki-home
---

# F.Y.R.N
![[fyrn.png]]
'Pela Ordem!'

![[Blocos/pesquisa_cards#^semtitulo]]
> [!abstract] Bem-vindo à wiki
> Aqui voce poderá encontrar uma multitude de informações sobre as campanhas e one-shots que englobam o cenário F.Y.R.N
---

## Explorar o mundo

---

## Personagens em destaque

```dataviewjs
const chars = dv.pages()
  .where(p => p.type === "character" && p.file)
  .sort(p => p.name || p.file.name, "asc")
  .limit(6);

let html = `<div class="wiki-related">`;

for (const p of chars) {
  let img = String(p.image || "").trim();
  if (!img) img = "Imagens/placeholder.png";

  img = img.replace(/^!\[\[|\]\]$/g, "").replace(/^["']|["']$/g, "");

  const file = app.metadataCache.getFirstLinkpathDest(img, p.file.path);
  const src = file ? app.vault.getResourcePath(file) : "";
  const name = String(p.name || p.file.name);
  const path = String(p.file.path).replace(/"/g, "&quot;");

  html += `
    <div class="wiki-related-card" data-href="${path}">
      <div class="wiki-related-img">
        ${src ? `<img src="${src}" alt="${name}">` : ""}
      </div>
      <div class="wiki-related-name">${name}</div>
    </div>
  `;
}

html += `</div>`;
dv.paragraph(html);

const cards = dv.container.querySelectorAll(".wiki-related-card");
cards.forEach(card => {
  card.addEventListener("click", () => {
    const path = card.getAttribute("data-href");
    app.workspace.openLinkText(path, "");
  });
});
```

---

## Regiões do cenário

```dataviewjs
const regions = dv.pages()
  .where(p => p.type === "region" && p.file)
  .sort(p => p.name || p.file.name, "asc")
  .limit(8);

let html = `<div class="wiki-home-links-grid">`;

for (const r of regions) {
  const name = String(r.name || r.file.name);
  const path = String(r.file.path).replace(/"/g, "&quot;");

  html += `
    <div class="wiki-home-link-card" data-href="${path}">
      <div class="wiki-home-link-title">${name}</div>
    </div>
  `;
}

html += `</div>`;
dv.paragraph(html);

const regionCards = dv.container.querySelectorAll(".wiki-home-link-card");
regionCards.forEach(card => {
  card.addEventListener("click", () => {
    const path = card.getAttribute("data-href");
    app.workspace.openLinkText(path, "");
  });
});
```

---

## Cidades e assentamentos

```dataviewjs
const cities = dv.pages()
  .where(p => p.type === "city" && p.file)
  .sort(p => p.name || p.file.name, "asc")
  .limit(10);

dv.list(cities.map(c => c.file.link));
```

---

## Campanhas e One-Shots
* [[A Ordem e o Kaos]]
	* Os arsonistas de Ther
	* Viajem a Valk
	* O mistério da mascara
* Calabouço de Sarah
* Dilema dos Visigodos
* O Rei zumbi

```dataviewjs
const factions = dv.pages()
  .where(p => p.type === "faction" && p.file)
  .sort(p => p.name || p.file.name, "asc")
  .limit(10);

dv.list(factions.map(f => f.file.link));
```
## Cronologia

- Origem do mundo
- Formação dos reinos
- Queda de antigas potências
- Era atual

---