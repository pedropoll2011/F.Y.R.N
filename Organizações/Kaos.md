---
type: organization
name: Kaos
image: ""
images:
  - Imagens/kaos.png
kind: ""
leader: ""
founder: ""
status: ""
alignment: ""
headquarters: ""
city: ""
region: ""
continent: ""
kingdom: ""
scope: ""
members: ""
founded: ""
origin: ""
tags:
  - organization
cssclasses:
  - wiki-character
---

![[Blocos/pesquisa_cards#^semtitulo]]
> [!abstract] Voce esta numa página de categoria: Organização
[[Voltar a tela principal]]

# Kaos

<!-- WIKI:INFOBOX:START -->
```dataviewjs
const page = dv.current();
dv.container.classList.add("wiki-infobox-host");

function v(value) {
  if (value === null || value === undefined) return "—";
  if (Array.isArray(value)) {
    const cleaned = value.map(x => String(x ?? "").trim()).filter(Boolean);
    return cleaned.length ? cleaned.join(", ") : "—";
  }
  return String(value).trim() === "" ? "—" : String(value);
}

function linkifyText(text) {
  return String(text).replace(/\[\[(.*?)\]\]/g, (match, p1) => {
    let [target, alias] = p1.split("|");

    target = (target || "").trim();
    alias = (alias || target).trim();

    const safeTarget = target.replace(/"/g, "&quot;");
    const display = alias.replace(/"/g, "&quot;");

    return `<a class="internal-link wiki-inline-link" data-href="${safeTarget}" href="${safeTarget}">${display}</a>`;
  });
}

function rich(value) {
  if (value === null || value === undefined) return "—";

  if (Array.isArray(value)) {
    const cleaned = value.map(x => String(x ?? "").trim()).filter(Boolean);
    if (!cleaned.length) return "—";
    return cleaned.map(item => linkifyText(item)).join("<br>");
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
  if (!clean) return false;
  if (clean === "undefined") return false;
  if (clean === "null") return false;
  if (clean === "false") return false;
  return true;
}

function resolveImageSrc(path) {
  const cleanPath = normalizeImagePath(path);
  if (!isUsableImagePath(cleanPath)) return "";

  const imageFile = app.metadataCache.getFirstLinkpathDest(cleanPath, page.file.path);
  return imageFile ? app.vault.getResourcePath(imageFile) : "";
}

let imageList = [];

/* images extras */
if (Array.isArray(page.images)) {
  imageList = page.images
    .map(normalizeImagePath)
    .filter(isUsableImagePath);
} else if (page.images) {
  const single = normalizeImagePath(page.images);
  if (isUsableImagePath(single)) imageList = [single];
}

/* image principal */
let mainImage = normalizeImagePath(page.image);
if (!isUsableImagePath(mainImage)) {
  mainImage = "";
}

if (mainImage && !imageList.includes(mainImage)) {
  imageList.unshift(mainImage);
}

/* resolve imagens existentes */
let resolvedImages = imageList
  .map(path => ({
    path,
    src: resolveImageSrc(path)
  }))
  .filter(img => img.src);

/* fallback único */
if (!resolvedImages.length) {
  const placeholderPath = "Imagens/placeholder.png";
  const placeholderSrc = resolveImageSrc(placeholderPath);
  if (placeholderSrc) {
    resolvedImages = [{
      path: placeholderPath,
      src: placeholderSrc
    }];
  }
}

/* HTML da imagem:
   - 1 imagem => imagem única
   - 2+ imagens => carrossel
*/
let imageHTML = "";

if (resolvedImages.length <= 1) {
  const onlySrc = resolvedImages[0]?.src || "";
  imageHTML = onlySrc
    ? `<img class="wiki-infobox-main-image" src="${onlySrc}" alt="${v(page.name || page.file.name)}">`
    : "";
} else {
  const slidesHTML = resolvedImages
    .map((img, index) => {
      const activeClass = index === 0 ? " is-active" : "";
      const altText = v(page.name || page.file.name);
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

let html = `
<div class="wiki-infobox">
  <div class="wiki-infobox-image">
    ${imageHTML}
  </div>

  <div class="wiki-infobox-name">
    ${v(page.name || page.file.name)}
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Tipo</div>
    <div class="wiki-infobox-value">${rich(page.kind)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Líder</div>
    <div class="wiki-infobox-value">${rich(page.leader)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Fundador</div>
    <div class="wiki-infobox-value">${rich(page.founder)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Status</div>
    <div class="wiki-infobox-value">${rich(page.status)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Alinhamento</div>
    <div class="wiki-infobox-value">${rich(page.alignment)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Sede</div>
    <div class="wiki-infobox-value">${rich(page.headquarters)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Membros</div>
    <div class="wiki-infobox-value">${rich(page.members)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Escopo</div>
    <div class="wiki-infobox-value">${rich(page.scope)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Fundação</div>
    <div class="wiki-infobox-value">${rich(page.founded)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Localização</div>
    <div class="wiki-infobox-value">${rich(page.kingdom)} > ${rich(page.region)} > ${rich(page.city)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Continente</div>
    <div class="wiki-infobox-value">${rich(page.continent)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Campanha</div>
    <div class="wiki-infobox-value">${rich(page.origin)}</div>
  </div>
</div>
`;

dv.container.innerHTML = html;

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
<!-- WIKI:INFOBOX:END -->

## Descrição

Escreva aqui a aparência geral, identidade visual, símbolos, reputação e impressão que a organização passa.

## História

Escreva aqui a origem da organização, eventos importantes, mudanças internas e papel dela no mundo.

## Estrutura

Descreva a hierarquia, patentes, divisões internas, forma de recrutamento e funcionamento.

## Objetivos e atuação

Explique o que a organização quer, como atua, quais métodos utiliza e qual sua área de influência.

## Recursos e notas

Anote bases, artefatos, contatos, tropas, segredos, regras internas ou curiosidades importantes.

## Relações

- 

## Aparições

- 

<!-- WIKI:EXTRAS:START -->

<!-- WIKI:EXTRAS:END -->

<!-- WIKI:RELACIONADOS:START -->

<!-- WIKI:RELACIONADOS:END -->