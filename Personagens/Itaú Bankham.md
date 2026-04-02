---
type: character
name: Itaú Bankham
image: ""
images:
  - Imagens/itau.png
  - Imagens/itau3.png
species: "[[Humano]]"
gender: Masculino
age: "19"
birthday:
  - ??/??/515 da segunda era
sign: "[[O Herdeiro]]"
status: Vivo
affiliation:
  - "[[Ordem]]"
occupation: "[[Aventureiro]]"
class: "[[Bárbaro]]"
city: ??
region: ??
continent: ""
kingdom: "[[Valkland]]"
altura: 1,90
origin: "[[A Ordem e o Kaos]]"
tags:
  - character
  - player_character
  - male
cssclasses:
  - wiki-character
---

![[Blocos/pesquisa_cards#^semtitulo]]
> [!abstract] Voce esta numa página de categoria: Personagem de Jogador
[[Voltar a tela principal]]
# Itaú Bankham

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
   - 1 imagem => modo antigo do Itaú
   - 2+ imagens => carrossel, mas cada slide usa o mesmo encaixe do modo antigo
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
    <div class="wiki-infobox-label">Espécie</div>
    <div class="wiki-infobox-value">${rich(page.species)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Gênero</div>
    <div class="wiki-infobox-value">${rich(page.gender)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Idade</div>
    <div class="wiki-infobox-value">${rich(page.age)}</div>
  </div>
  
  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Data de nascimento</div>
    <div class="wiki-infobox-value">${rich(page.birthday)} (${rich(page.sign)})</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Altura</div>
    <div class="wiki-infobox-value">${rich(page.altura)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Status</div>
    <div class="wiki-infobox-value">${rich(page.status)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Afiliação</div>
    <div class="wiki-infobox-value">${rich(page.affiliation)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Ocupação</div>
    <div class="wiki-infobox-value">${rich(page.occupation)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Classe</div>
    <div class="wiki-infobox-value">${rich(page.class)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Local de origem</div>
    <div class="wiki-infobox-value">${rich(page.kingdom)} > ${rich(page.region)} > ${rich(page.city)}</div>
  </div>

  <div class="wiki-infobox-row">
    <div class="wiki-infobox-label">Campanha de origem</div>
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

Itaú é um valkiano de estatura alta, cabelo e barba ruivos.
### Evolução
* Durante o arco tal — Matou um urso e agora usa a pele dele
* Durante o outro arco tall 2 — Começou a usar mascara para esconder a indentidade


## História

### Raízes

Itaú nasceu no clã do corvo, filho de Harald Bankham, um antigo bardo que sonhava em unir os saxões de [[Valkland]].

Após ser acusado de traição, sua família foi forçada a fugir. Durante a fuga, sua mãe morreu, evento que marcou profundamente Itaú e despertou nele um juramento: nunca recuar novamente.

Posteriormente, seu pai desafia o conde Canute em duelo, o derrota e assume o comando do clã.

### Infância e formação

Durante sua infância, Itaú foi impedido de lutar diretamente, sendo treinado em estratégia, negociação e combate.

Mesmo assim, desenvolveu uma visão dura do mundo, baseada em honra, vingança e sobrevivência.

### Queda do clã do corvo

A tentativa de seu pai de unir os saxões gerou desconfiança entre outros clãs.

Lobos, ursos e cabras se uniram em uma aliança temporária para destruir o clã do corvo, visando dividir suas terras, riquezas e poder.

O ataque veio de todas as direções, dando início a uma batalha desesperada.


### O massacre

Desobedecendo ordens, Itaú foge do caminho de evacuação e acaba testemunhando o fim de seu clã.

No salão central, vê seu pai ser morto com o próprio machado do clã, empunhado por [[Yngwaldd]], enquanto [[Ivan O Louco]] observa.

Movido por impulso, Itaú ataca Ivan e o mata, marcando seu primeiro ato como guerreiro.

### Sobrevivência

No momento em que acreditava que morreria, é salvo por [[Floki]], que o retira do local em meio ao caos.

O clã do corvo é destruído, deixando Itaú como um de seus últimos sobreviventes.

## Personalidade

Itaú é moldado por perda, violência e sobrevivência.

Carrega uma visão dura do mundo, onde honra e morte são inseparáveis.  
É impulsivo em momentos críticos, mas também demonstra inteligência estratégica, fruto do treinamento recebido de seu pai.

## Objetivos

- Honrar o legado de [[Harald Bankham]]  
- Dar sentido à queda do clã do corvo  
- Sobreviver em um mundo marcado por traição  

## Habilidades e notas

- Treinamento estratégico desde a infância  
- Experiência precoce em combate  
- Forte resistência mental a situações extremas  

## Durante a campanha

AQUI VAI TODO O RESTO DO CARALHO


## Relações

- [[Harald Bankham]] — Pai  
- [[Floki]] — Protetor / aliado  
- [[Yngwaldd]] — Assassino de seu pai  
- [[Ivan O Louco]] — Morto por Itaú  

## Aparições

- Origem em [[Valkland]]  
- Queda do clã do corvo  
- Início da jornada como sobrevivente  

## Linha do tempo

- Fuga de [[Valkland]]  
- Morte da mãe  
- Duelo de Harald contra Canute  
- Ascensão do clã do corvo  
- Treinamento de Itaú  
- Ataque dos clãs aliados  
- Morte de Harald  
- Morte de Ivan  
- Fuga com Floki


<!-- WIKI:EXTRAS:START -->

<!-- WIKI:EXTRAS:END -->

<!-- WIKI:RELACIONADOS:START -->
![[Blocos/reino#^semtitulo]]
![[Blocos/cidade#^semtitulo]]
<!-- WIKI:RELACIONADOS:END -->