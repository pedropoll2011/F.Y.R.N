<%* 
let nome = await tp.system.prompt("Nome da entidade/divindade");
if (!nome || nome.trim() === "") {
  nome = "Entidade Sem Nome";
}
nome = nome.replace(/[\\/:*?"<>|]/g, "").trim();
await tp.file.rename(nome);
_%>
---
type: entity
name: "<% nome %>"
image: ""
kind: ""
domains: []
alignment: ""
status: ""
worshippers: []
symbol: ""
plane: ""
location: ""
origin: ""
titles: []
tags: [entity]
cssclasses:
  - wiki-character
---

# <% nome %>

<!-- WIKI:INFOBOX:START -->
```dataviewjs
const page = dv.current();
dv.container.classList.add("wiki-infobox-host");

function v(value) {
  if (!value || String(value).trim() === "") return "—";
  return String(value);
}

function rich(value) {
  if (!value) return "—";

  if (Array.isArray(value)) {
    return value.map(item => {
      let text = String(item);
      return text.replace(/\[\[(.*?)\]\]/g, (match, p1) => {
        let [target, alias] = p1.split("|");
        const safeTarget = target.replace(/"/g, "&quot;");
        const display = (alias || target).replace(/"/g, "&quot;");
        return `<a class="internal-link wiki-inline-link" data-href="${safeTarget}" href="${safeTarget}">${display}</a>`;
      });
    }).join(", ");
  }

  let text = String(value);
  text = text.replace(/\[\[(.*?)\]\]/g, (match, p1) => {
    let [target, alias] = p1.split("|");
    const safeTarget = target.replace(/"/g, "&quot;");
    const display = (alias || target).replace(/"/g, "&quot;");
    return `<a class="internal-link wiki-inline-link" data-href="${safeTarget}" href="${safeTarget}">${display}</a>`;
  });

  return text.trim() === "" ? "—" : text;
}

let img = page.image ? String(page.image).trim() : "";
if (!img) img = "Imagens/placeholder.png";

img = img.replace(/^!\[\[|\]\]$/g, "").replace(/^["']|["']$/g, "");

const imageFile = app.metadataCache.getFirstLinkpathDest(img, page.file.path);
const src = imageFile ? app.vault.getResourcePath(imageFile) : "";

let html = `
<div class="wiki-infobox">
  <div class="wiki-infobox-image">
    ${src ? `<img src="${src}" alt="${v(page.name || page.file.name)}">` : ""}
  </div>

  <div class="wiki-infobox-name">
    ${v(page.name || page.file.name)}
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Tipo</div>
    <div class="wiki-infobox-value">${rich(page.kind)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Domínios</div>
    <div class="wiki-infobox-value">${rich(page.domains)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Alinhamento</div>
    <div class="wiki-infobox-value">${rich(page.alignment)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Status</div>
    <div class="wiki-infobox-value">${rich(page.status)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Adoradores</div>
    <div class="wiki-infobox-value">${rich(page.worshippers)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Símbolo</div>
    <div class="wiki-infobox-value">${rich(page.symbol)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Plano / Reino</div>
    <div class="wiki-infobox-value">${rich(page.plane)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Local ligado</div>
    <div class="wiki-infobox-value">${rich(page.location)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Origem</div>
    <div class="wiki-infobox-value">${rich(page.origin)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Títulos</div>
    <div class="wiki-infobox-value">${rich(page.titles)}</div>
  </div>
</div>
`;

dv.container.innerHTML = html;

const links = dv.container.querySelectorAll(".wiki-inline-link[data-href]");
links.forEach(linkEl => {
  linkEl.addEventListener("click", (e) => {
    e.preventDefault();
    const path = linkEl.getAttribute("data-href");
    app.workspace.openLinkText(path, page.file.path);
  });
});
```
<!-- WIKI:INFOBOX:END -->

## Descrição

Descreva aqui a entidade de forma geral: aparência, presença, aura e como ela é percebida no mundo.

## Natureza e domínio

Explique o que ela representa, quais forças governa e qual seu papel no mundo.

## Culto e influência

Quem a venera? Como? Existem templos, rituais, sacerdotes?

## História e mitologia

Mitos de origem, feitos, guerras, ascensão ou queda.

## Relações

- 

## Dogmas, mandamentos ou sinais

- 

## Aparições

- 

## Observações

- 

<!-- WIKI:RELACIONADOS:START -->
![[Blocos/membros#^membros-org]]
<!-- WIKI:RELACIONADOS:END -->