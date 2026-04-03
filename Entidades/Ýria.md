---
type: entity
name: Ýria
image: Imagens/yria.jpg
kind: Deus Neutro
domains:
  - Destino
alignment: "[[True Neutral]]"
status: ""
worshippers: Seguidores de Ýria
symbol: Pinheiro
plane: ""
location: Ruínas de Ýria
titles:
  - A Deusa do Destino
  - A Deusa da Árvore
origin: "[[A Ordem e o Kaos]]"
tags:
  - entity
cssclasses:
  - wiki-character
---

# Ýria

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

Ýria existe na forma etérea de uma mulher adulta, seu corpo não possui todos os detalhes e é totalmente água-marinha em cor. Ela tem uma personalidade brincalhona e as vezes até infantil.

## Natureza e domínio

É um completo mistério o que faz Ýria agir, talvez até seja completamente espontâneo ou randômico de sua parte. seu domínio a possibilita dar relíquias e bençãos aos seus seguidores, mas apenas se Ýria o escolher, talvez seja ela ditando o destino daqueles que são escolhidos por ela.

## Culto e influência

Não se tem muita informação de quantos veneram Ýria, ela não é uma crença popular na região de [[Alvölandd]]
* Nas [[Ruínas de Ýria]] três entitulados paladinos de Ýria moravam em uma fortificação perto de uma ilhota com uma grande árvore rodeada por um lago.

## História e mitologia

Ýria foi o resultado de uma anomalia, quando a viajante dimensional [[Lily Shatterfield]] vem a falecer durante o parto, o seu espírito se ligou a uma grande árvore numa ilhota circulada por um lago, naquele momento a entidade conhecida como Ýria vem a nascer, contendo traços de ambos mãe e progênito, mas indo além disso, não se sabe de muito.

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