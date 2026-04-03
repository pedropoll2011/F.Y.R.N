```dataviewjs
const container = dv.container;

/* =========================
   CONFIG
========================= */

const SEARCHABLE_TYPES = ["character", "organization", "entity"];

/* =========================
   HELPERS
========================= */

function normalize(text) {
  return String(text || "")
    .toLowerCase()
    .normalize("NFD")
    .replace(/[\u0300-\u036f]/g, "")
    .trim();
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

function resolveImageSrc(path, currentFilePath) {
  const cleanPath = normalizeImagePath(path);
  if (!isUsableImagePath(cleanPath)) return "";

  const imageFile = app.metadataCache.getFirstLinkpathDest(cleanPath, currentFilePath);
  return imageFile ? app.vault.getResourcePath(imageFile) : "";
}

function getMainImage(pageObj) {
  if (Array.isArray(pageObj.images)) {
    for (const img of pageObj.images) {
      const src = resolveImageSrc(img, pageObj.file.path);
      if (src) return src;
    }
  }

  if (pageObj.image) {
    const src = resolveImageSrc(pageObj.image, pageObj.file.path);
    if (src) return src;
  }

  return resolveImageSrc("Imagens/placeholder.png", pageObj.file.path);
}

function getTypeLabel(type) {
  const labels = {
    character: "Personagem",
    organization: "Organização"
  };

  return labels[type] || type;
}

/* =========================
   HTML BASE
========================= */

container.innerHTML = `
  <div class="wiki-global-search">
    <input
      type="text"
      class="wiki-global-search-input"
      placeholder="Buscar personagens e organizações..."
    >
    <div class="wiki-global-results"></div>
  </div>
`;

const input = container.querySelector(".wiki-global-search-input");
const resultsDiv = container.querySelector(".wiki-global-results");

/* =========================
   RENDER
========================= */

function renderResults(rawQuery) {
  const query = normalize(rawQuery);
  resultsDiv.innerHTML = "";

  if (!query) return;

  const pages = dv.pages()
    .where(p => p && p.file && SEARCHABLE_TYPES.includes(String(p.type || "").trim()))
    .array()
    .filter(p => normalize(p.name || p.file.name).includes(query))
    .sort((a, b) => {
      const an = normalize(a.name || a.file.name);
      const bn = normalize(b.name || b.file.name);
      return an.localeCompare(bn);
    })
    .slice(0, 20);

  if (!pages.length) {
    resultsDiv.innerHTML = `<div class="wiki-search-empty">Nenhum resultado</div>`;
    return;
  }

  let html = `<div class="wiki-related">`;

  for (const p of pages) {
    const src = getMainImage(p);
    const filePath = String(p.file.path).replace(/"/g, "&quot;");
    const fileName = String(p.name || p.file.name);
    const typeLabel = getTypeLabel(String(p.type || "").trim());

    html += `
      <div class="wiki-related-card" data-href="${filePath}">
        <div class="wiki-related-img">
          ${src ? `<img src="${src}" alt="${fileName}">` : ""}
        </div>
        <div class="wiki-related-name">${fileName}</div>
        <div class="wiki-search-card-type">${typeLabel}</div>
      </div>
    `;
  }

  html += `</div>`;
  resultsDiv.innerHTML = html;

  const cards = resultsDiv.querySelectorAll(".wiki-related-card[data-href]");
  cards.forEach(card => {
    card.addEventListener("click", () => {
      const path = card.getAttribute("data-href");
      app.workspace.openLinkText(path, dv.current().file.path);
    });
  });
}

/* =========================
   EVENTS
========================= */

input.addEventListener("input", () => {
  renderResults(input.value);
});
```
^semtitulo