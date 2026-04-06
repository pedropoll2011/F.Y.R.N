<%* 
let nome = await tp.system.prompt("Nome da organização");
if (!nome || nome.trim() === "") {
  nome = "Organização Sem Nome";
}
nome = nome.replace(/[\\/:*?"<>|]/g, "").trim();
await tp.file.rename(nome);
_%>
---
type: organization
name: "<% nome %>"
image: ""
images: []
category: ""
alignment: ""
status: ""
leader: ""
founder: ""
members: []
headquarters: ""
city: ""
region: ""
continent: ""
kingdom: ""
founded_in: ""
beliefs: []
goals: []
origin: ""
tags: [organization]
cssclasses:
  - wiki-character
---

![[Blocos/pesquisa_cards#^semtitulo]]

> [!abstract] Voce esta numa página de categoria: Organização  
[[Voltar a tela principal]]

<!-- WIKI:TOP:START -->
```dataviewjs
const page = dv.current();
dv.container.classList.add("wiki-top-host");

function v(value) {
  if (value === null || value === undefined) return "—";
  if (Array.isArray(value)) {
    const cleaned = value.map(x => String(x ?? "").trim()).filter(Boolean);
    return cleaned.length ? cleaned.join(", ") : "—";
  }
  return String(value).trim() === "" ? "—" : String(value);
}

function esc(text) {
  return String(text ?? "")
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;");
}

function linkifyText(text) {
  return String(text).replace(/\[\[(.*?)\]\]/g, (match, p1) => {
    let [target, alias] = p1.split("|");
    target = (target || "").trim();
    alias = (alias || target).trim();
    return `<a class="internal-link wiki-inline-link" data-href="${esc(target)}" href="${esc(target)}">${esc(alias)}</a>`;
  });
}

function rich(value) {
  if (value === null || value === undefined) return "—";
  if (Array.isArray(value)) {
    const cleaned = value.map(x => String(x ?? "").trim()).filter(Boolean);
    return cleaned.length ? cleaned.map(linkifyText).join("<br>") : "—";
  }
  const text = String(value).trim();
  if (!text) return "—";
  return linkifyText(text);
}

function normalizeImagePath(path) {
  return String(path || "")
    .trim()
    .replace(/^!\[\[|\]\]$/g, "")
    .replace(/^["']|["']$/g, "");
}

function isUsableImagePath(path) {
  const clean = normalizeImagePath(path).toLowerCase();
  return !!clean && clean !== "undefined" && clean !== "null" && clean !== "false";
}

function resolveImage(path) {
  const clean = normalizeImagePath(path);
  if (!isUsableImagePath(clean)) return "";
  const file = app.metadataCache.getFirstLinkpathDest(clean, page.file.path);
  return file ? app.vault.getResourcePath(file) : "";
}

let imageList = [];

if (Array.isArray(page.images)) {
  imageList = page.images.map(normalizeImagePath).filter(isUsableImagePath);
} else if (page.images) {
  const single = normalizeImagePath(page.images);
  if (isUsableImagePath(single)) imageList = [single];
}

let mainImage = normalizeImagePath(page.image);
if (isUsableImagePath(mainImage) && !imageList.includes(mainImage)) {
  imageList.unshift(mainImage);
}

let resolvedImages = imageList
  .map(path => ({ path, src: resolveImage(path) }))
  .filter(img => img.src);

if (!resolvedImages.length) {
  const fallback = resolveImage("Imagens/placeholder.png");
  if (fallback) {
    resolvedImages = [{ path: "Imagens/placeholder.png", src: fallback }];
  }
}

let imageHTML = "";

if (resolvedImages.length <= 1) {
  const onlySrc = resolvedImages[0]?.src || "";
  imageHTML = onlySrc
    ? `<img class="wiki-infobox-main-image" src="${onlySrc}" alt="${esc(v(page.name || page.file.name))}">`
    : "";
} else {
  const slidesHTML = resolvedImages.map((img, index) => {
    const activeClass = index === 0 ? " is-active" : "";
    return `
      <div class="wiki-carousel-slide${activeClass}" data-index="${index}">
        <img class="wiki-carousel-slide-img" src="${img.src}" alt="${esc(v(page.name || page.file.name))}">
      </div>
    `;
  }).join("");

  const dotsHTML = `
    <div class="wiki-carousel-dots">
      ${resolvedImages.map((img, index) => {
        const activeClass = index === 0 ? " is-active" : "";
        return `<button class="wiki-carousel-dot${activeClass}" type="button" data-index="${index}" aria-label="Ir para imagem ${index + 1}"></button>`;
      }).join("")}
    </div>
  `;

  imageHTML = `
    <div class="wiki-carousel" data-total="${resolvedImages.length}">
      <div class="wiki-carousel-track">
        ${slidesHTML}
      </div>
      <button class="wiki-carousel-btn wiki-carousel-prev" type="button" aria-label="Imagem anterior">‹</button>
      <button class="wiki-carousel-btn wiki-carousel-next" type="button" aria-label="Próxima imagem">›</button>
      ${dotsHTML}
    </div>
  `;
}

dv.container.innerHTML = "";

/* layout */
const layout = dv.container.createDiv({ cls: "wiki-top-layout" });
const left = layout.createDiv({ cls: "wiki-top-main" });
const right = layout.createDiv({ cls: "wiki-top-side" });

/* MARKDOWN REAL */
const leftMarkdown = `
# ${v(page.name || page.file.name)}


## História

Escreva aqui a origem da organização, eventos marcantes, mudanças internas e papel dela no cenário.


`;

const MR =
  window.MarkdownRenderer ||
  window.obsidian?.MarkdownRenderer ||
  obsidian?.MarkdownRenderer;

if (MR) {
  await MR.renderMarkdown(leftMarkdown, left, page.file.path, null);
}

/* INFOBOX COMPLETA */
right.innerHTML = `
<div class="wiki-infobox">
  <div class="wiki-infobox-image">${imageHTML}</div>

  <div class="wiki-infobox-name">${esc(v(page.name || page.file.name))}</div>

  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Categoria</div><div class="wiki-infobox-value">${rich(page.category)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Alinhamento</div><div class="wiki-infobox-value">${rich(page.alignment)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Status</div><div class="wiki-infobox-value">${rich(page.status)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Líder</div><div class="wiki-infobox-value">${rich(page.leader)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Fundador</div><div class="wiki-infobox-value">${rich(page.founder)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Membros</div><div class="wiki-infobox-value">${rich(page.members)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Sede</div><div class="wiki-infobox-value">${rich(page.headquarters)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Fundação</div><div class="wiki-infobox-value">${rich(page.founded_in)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Crenças</div><div class="wiki-infobox-value">${rich(page.beliefs)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Objetivos</div><div class="wiki-infobox-value">${rich(page.goals)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Localização</div><div class="wiki-infobox-value">${rich(page.kingdom)} &gt; ${rich(page.region)} &gt; ${rich(page.city)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Campanha de origem</div><div class="wiki-infobox-value">${rich(page.origin)}</div></div>
</div>
`;
```
<!-- WIKI:TOP:END -->

## Descrição

Escreva aqui a visão geral da organização, sua reputação, presença no mundo e como ela é percebida.

## Estrutura

Descreva hierarquia, cargos, divisões, modo de operação e funcionamento interno.

## Objetivos e ideologia

Explique as metas da organização, suas crenças, valores e métodos.

## Relações

- 

## Membros notáveis

- 

## Aparições

- 

<!-- WIKI:RELACIONADOS:START -->
![[Blocos/reino#^semtitulo]]
![[Blocos/cidade#^semtitulo]]
<!-- WIKI:RELACIONADOS:END -->