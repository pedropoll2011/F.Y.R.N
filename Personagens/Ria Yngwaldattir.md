---
type: character
name: Ria Yngwaldattir
image: ""
images:
  - Imagens/ria.png
species: "[[Humano|Humana]]"
gender: Feminino
age: ""
birthday: ""
sign: ""
status: Viva
affiliation: ""
occupation: ""
class: ""
believes_in: []
city: ""
region: ""
continent: ""
kingdom: "[[Valkland]]"
altura: ""
origin: "[[A Ordem e o Kaos]]"
alignment: ""
tags:
  - character
  - female
  - npc
cssclasses:
  - wiki-character
---

![[Blocos/pesquisa_cards#^semtitulo]]

> [!abstract] Voce esta numa página de categoria: NPC  
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

Em algum momento, Ria, seu pai [[Yngwald]] e sua mãe fogem de [[Valkland]]. Seu pai havia desertado do exército e era acusado de traição.

A família viaja rumo ao sul, eventualmente chegando ao reino de [[Alvölandd]], passando pela região das [[Ruínas de Ýria]]. Durante essa jornada, Ria perde seu pai em combate e é capturada junto com sua mãe.

Mais tarde, ambas são libertadas. No entanto, vivendo em um reino onde enfrentam preconceito contra valkianos e sem qualquer apoio, passam a enfrentar grandes dificuldades para sobreviver.

Após esses eventos, um único sentimento permanece no coração da garota:  
o desejo de vingança contra aqueles que tiraram a vida de seu pai.

## Descrição

Ria é uma jovem valkiana de cabelos ruivos compridos.

## Personalidade

Pouco se sabe sobre como Ria era antes da morte de seu pai.

Após sua perda, desenvolveu um forte rancor, tornando-se alguém mais impulsivo e propenso a deixar a raiva guiar suas ações.

## Relações

- [[Yngwald]] — Pai  
- ?? — Mãe  
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

  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Espécie</div><div class="wiki-infobox-value">${rich(page.species)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Gênero</div><div class="wiki-infobox-value">${rich(page.gender)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Idade</div><div class="wiki-infobox-value">${rich(page.age)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Data de nascimento</div><div class="wiki-infobox-value">${rich(page.birthday)} (${rich(page.sign)})</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Altura</div><div class="wiki-infobox-value">${rich(page.altura)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Status</div><div class="wiki-infobox-value">${rich(page.status)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Afiliação</div><div class="wiki-infobox-value">${rich(page.affiliation)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Ocupação</div><div class="wiki-infobox-value">${rich(page.occupation)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Alinhamento</div><div class="wiki-infobox-value">${rich(page.alignment)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Crença</div><div class="wiki-infobox-value">${rich(page.believes_in)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Classe</div><div class="wiki-infobox-value">${rich(page.class)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Local de origem</div><div class="wiki-infobox-value">${rich(page.kingdom)} &gt; ${rich(page.region)} &gt; ${rich(page.city)}</div></div>
  <div class="wiki-infobox-row"><div class="wiki-infobox-label">Campanha de origem</div><div class="wiki-infobox-value">${rich(page.origin)}</div></div>
</div>
`;
```
<!-- WIKI:TOP:END -->


## Habilidades e notas

- Demonstrou habilidade com estilingue ao auxiliar seu pai em combate.  
- Durante a emboscada contra o grupo de Tryte, utilizava uma lâmina curta.  
- Apresenta forte inclinação para se tornar uma [[Ladina]].  

## Aparições

### [[Saga dos Santos de Ýria]]
Sua primeira aparição ocorre durante essa saga, onde enfrenta a party principal e os paladinos de Ýria.

### Preparo e descanso, aguardando um aliado e um dilema
Durante a noite, no caminho para a residência de [[Affons]], o grupo de [[Tryte Yendévor|Tryte]] e [[Kurtis]] é emboscado por Ria e outros indivíduos desconhecidos.

Após ser derrotada, Ria é capturada e levada para interrogatório.

> ⚠️ Definir com o mestre:  
> - Ela foi libertada?  
> - Fugiu?

Mais tarde, reaparece no [[Peitoral de Adamante]], onde [[Tryte Yendévor|Tryte]], [[Drake]] e [[Floki]] estavam.  
Tenta atacar Tryte, mas falha.

Após uma conversa com Floki, foge novamente, tornando-se uma fugitiva procurada em [[Ther (Cidade)]] após ser denunciada por Drake.

### Pré-Arco da Força

Ria invade a base dos [[Santos de Ýria]] no forte da Ordem.  
Dessa vez, no entanto, o encontro não é hostil — ela surge com um pedido de ajuda.

--- 
<!-- WIKI:RELACIONADOS:START -->
![[Blocos/reino#^semtitulo]]
![[Blocos/cidade#^semtitulo]]
<!-- WIKI:RELACIONADOS:END -->