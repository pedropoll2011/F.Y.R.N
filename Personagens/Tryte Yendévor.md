---
type: character
name: Tryte Yendévor
image:
images:
  - Imagens/tryte.jpeg
species: "[[Humano]]"
gender: Masculino
age: "20"
birthday:
  - 01/04/514 da [[Segunda Era]]
sign: "[[O Herdeiro]]"
status: Vivo
affiliation:
  - Co-fundador da [[Ordem]]
occupation:
  - "[[Aventureiro]]"
class: "[[Artífice]]"
believes_in:
  - "[[Ýria]]"
city: "[[Lumorath]]"
region: "[[Ther (Condado)]]"
continent: ""
kingdom: "[[Alvölandd]]"
altura: 1,70
origin: "[[A Ordem e o Kaos]]"
alignment: "[[Rebel Good]]"
tags:
  - character
  - male
  - player_character
cssclasses:
  - wiki-character
---

![[Blocos/pesquisa_cards#^semtitulo]]

> [!abstract] Voce esta numa página de categoria: Personagem de Jogador
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

function normalizeImagePath(value) {
  if (value === null || value === undefined) return "";

  if (typeof value === "string") {
    return value
      .trim()
      .replace(/^!\[\[|\]\]$/g, "")
      .replace(/^["']|["']$/g, "");
  }

  if (typeof value === "object") {
    if (typeof value.path === "string") return value.path.trim();
    if (typeof value.file?.path === "string") return value.file.path.trim();
    if (typeof value.value === "string") return value.value.trim();

    if (typeof value.markdown === "string") {
      return value.markdown
        .trim()
        .replace(/^!\[\[|\]\]$/g, "")
        .replace(/^["']|["']$/g, "");
    }
  }

  return String(value).trim();
}

function isUsableImagePath(path) {
  const clean = normalizeImagePath(path).toLowerCase();
  return !!clean &&
    clean !== "undefined" &&
    clean !== "null" &&
    clean !== "false" &&
    clean !== "[object object]";
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
  const slidesHTML = resolvedImages
    .map((img, index) => {
      const activeClass = index === 0 ? " is-active" : "";
      const altText = esc(v(page.name || page.file.name));
      return `
        <div class="wiki-carousel-slide${activeClass}" data-index="${index}">
          <img class="wiki-carousel-slide-img" src="${img.src}" alt="${altText}">
        </div>
      `;
    })
    .join("");

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

## Descrição

De estatura média, Tryte possui cabelos escuros e desgrenhados que chegam até os ombros, olhos castanhos e uma barba mal feita. Sua aparência carrega sinais claros de desgaste e experimentação, com marcas sutis de manipulação arcana e mecânica pelo corpo.

Atualmente, veste equipamentos de sua própria criação, incluindo o [[Hex Launcher v2]], os [[Rea-lenses]] e a [[Lifesink]], que se integram parcialmente ao seu corpo e estilo de combate.

## Evolução

### Início da jornada
No início de sua vida como aventureiro, Tryte usava uma jaqueta longa aos trapos, cheia de bolsos, junto de botas e calças em condições igualmente humildes. Em sua cintura, carregava uma corrente conectada ao seu grimório de necromante.

### Serviço em [[Ther (Condado)]]
Após se juntar ao exército de [[Ther (Condado)]], passou a adotar vestimentas mais estruturadas e adequadas ao combate, embora ainda mantivesse elementos improvisados de seu equipamento original.

### Após [[A purificação de Tryte]]
Após esses eventos, abandonou gradualmente sua identidade como necromante e se tornou um artífice. Seu visual mudou drasticamente, passando a incorporar dispositivos criados por ele mesmo, como o [[Hex Launcher v1]], os [[Rea-lenses]] e a [[Lifesink]].
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

const links = dv.container.querySelectorAll(".wiki-inline-link[data-href]");
links.forEach(linkEl => {
  linkEl.addEventListener("click", (e) => {
    e.preventDefault();
    e.stopPropagation();
    const path = linkEl.getAttribute("data-href");
    app.workspace.openLinkText(path, page.file.path);
  });
});

const carousel = dv.container.querySelector(".wiki-carousel");
if (carousel) {
  const slides = Array.from(carousel.querySelectorAll(".wiki-carousel-slide"));
  const dots = Array.from(carousel.querySelectorAll(".wiki-carousel-dot"));
  const prevBtn = carousel.querySelector(".wiki-carousel-prev");
  const nextBtn = carousel.querySelector(".wiki-carousel-next");

  let currentIndex = 0;

  function updateCarousel(index) {
    currentIndex = index;

    slides.forEach((slide, i) => {
      slide.classList.toggle("is-active", i === currentIndex);
    });

    dots.forEach((dot, i) => {
      dot.classList.toggle("is-active", i === currentIndex);
    });
  }

  if (prevBtn) {
    prevBtn.addEventListener("click", (e) => {
      e.preventDefault();
      e.stopPropagation();
      const nextIndex = (currentIndex - 1 + slides.length) % slides.length;
      updateCarousel(nextIndex);
    });
  }

  if (nextBtn) {
    nextBtn.addEventListener("click", (e) => {
      e.preventDefault();
      e.stopPropagation();
      const nextIndex = (currentIndex + 1) % slides.length;
      updateCarousel(nextIndex);
    });
  }

  dots.forEach((dot, index) => {
    dot.addEventListener("click", (e) => {
      e.preventDefault();
      e.stopPropagation();
      updateCarousel(index);
    });
  });

  updateCarousel(0);
}
```
<!-- WIKI:TOP:END -->

## História

Escreva aqui o passado do personagem, eventos importantes e papel na campanha.

## Personalidade

Descreva qualidades, defeitos, desejos, medos, manias e comportamento.

## Habilidades e notas

Anote poderes, técnicas, itens importantes, segredos e curiosidades.

## Relações
-

## Aparições
-

<!-- WIKI:RELACIONADOS:START -->
![[Blocos/reino#^semtitulo]]
![[Blocos/cidade#^semtitulo]]
<!-- WIKI:RELACIONADOS:END -->