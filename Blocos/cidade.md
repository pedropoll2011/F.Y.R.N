> [!info]- Personagens da mesma cidade
> ```dataviewjs
> const activeFile = app.workspace.getActiveFile();
>
> function normalizeImagePath(path) {
>   return String(path || "")
>     .trim()
>     .replace(/^!\[\[|\]\]$/g, "")
>     .replace(/^["']|["']$/g, "");
> }
>
> function isUsableImagePath(path) {
>   const clean = normalizeImagePath(path).toLowerCase();
>   if (!clean) return false;
>   if (clean === "undefined") return false;
>   if (clean === "null") return false;
>   if (clean === "false") return false;
>   return true;
> }
>
> function resolveImageSrc(path, currentFilePath) {
>   const cleanPath = normalizeImagePath(path);
>   if (!isUsableImagePath(cleanPath)) return "";
>
>   const imageFile = app.metadataCache.getFirstLinkpathDest(cleanPath, currentFilePath);
>   return imageFile ? app.vault.getResourcePath(imageFile) : "";
> }
>
> function getMainImage(pageObj) {
>   if (Array.isArray(pageObj.images)) {
>     for (const img of pageObj.images) {
>       const src = resolveImageSrc(img, pageObj.file.path);
>       if (src) return src;
>     }
>   }
>
>   if (pageObj.image) {
>     const src = resolveImageSrc(pageObj.image, pageObj.file.path);
>     if (src) return src;
>   }
>
>   return resolveImageSrc("Imagens/placeholder.png", pageObj.file.path);
> }
>
> if (!activeFile) {
>   dv.paragraph("Nenhuma nota ativa encontrada.");
> } else {
>   const current = dv.page(activeFile.path);
>
>   if (!current) {
>     dv.paragraph("A Dataview ainda não indexou esta página.");
>   } else {
>     let city = current.city;
>
>     if (current.city == "??") {
>       city = "-";
>     }
>
>     if (!city) {
>       dv.paragraph("Nenhuma cidade definida.");
>     } else {
>       const pages = dv.pages()
>         .where(p => p && p.type === "character" && p.file)
>         .array()
>         .filter(p => String(p.city || "") === String(city))
>         .sort((a, b) => {
>           const an = String(a.name || a.file.name).toLowerCase();
>           const bn = String(b.name || b.file.name).toLowerCase();
>           return an.localeCompare(bn);
>         });
>
>       if (pages.length === 0) {
>         dv.paragraph(`Nenhum personagem encontrado.`);
>       } else {
>         let html = `<div class="wiki-related">`;
>
>         for (const p of pages) {
>           const src = getMainImage(p);
>
>           const filePath = String(p.file.path).replace(/"/g, "&quot;");
>           const fileName = String(p.name || p.file.name);
>           const isCurrent = p.file.path === current.file.path;
>
>           if (isCurrent) {
>             html += `
>               <div class="wiki-related-card wiki-related-current">
>                 <div class="wiki-related-img">
>                   ${src ? `<img src="${src}" alt="${fileName}">` : ""}
>                 </div>
>                 <div class="wiki-related-name">${fileName}</div>
>               </div>
>             `;
>           } else {
>             html += `
>               <div class="wiki-related-card" data-href="${filePath}">
>                 <div class="wiki-related-img">
>                   ${src ? `<img src="${src}" alt="${fileName}">` : ""}
>                 </div>
>                 <div class="wiki-related-name">${fileName}</div>
>               </div>
>             `;
>           }
>         }
>
>         html += `</div>`;
>         dv.paragraph(html);
>
>         const cards = dv.container.querySelectorAll(".wiki-related-card[data-href]");
>         cards.forEach(card => {
>           card.addEventListener("click", () => {
>             const path = card.getAttribute("data-href");
>             app.workspace.openLinkText(path, current.file.path);
>           });
>         });
>       }
>     }
>   }
> }
> ```
^semtitulo