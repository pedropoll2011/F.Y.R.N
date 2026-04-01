> [!info]- Membros da organização
> ```dataviewjs
> const activeFile = app.workspace.getActiveFile();
>
> if (!activeFile) {
>   dv.paragraph("Nenhuma nota ativa encontrada.");
> } else {
>   const current = dv.page(activeFile.path);
>
>   if (!current) {
>     dv.paragraph("A Dataview ainda não indexou esta página.");
>   } else {
>     const factionName = String(current.name || current.file.name || "").trim();
>
>     if (!factionName) {
>       dv.paragraph("Nome da organização não definido.");
>     } else {
>       let pages = dv.pages()
>       if(dv.pages() == "??"){
>           pages = "-";
>     }
>         .where(p => p && p.type === "character" && p.file)
>         .array()
>         .filter(p => {
>           if (!p.affiliation) return false;
>
>           const affiliations = Array.isArray(p.affiliation)
>             ? p.affiliation
>             : [p.affiliation];
>
>           return affiliations.some(a => {
>             if (!a) return false;
>
>             if (typeof a === "object" && a.path) {
>               return a.path === current.file.path;
>             }
>
>             const text = String(a).trim();
>             const clean = text.replace(/\[\[|\]\]/g, "").split("|")[0].replace(/\.md$/i, "");
>             const currentName = factionName.replace(/\.md$/i, "");
>
>             return clean === currentName;
>           });
>         })
>         .sort((a, b) => {
>           const an = String(a.name || a.file.name).toLowerCase();
>           const bn = String(b.name || b.file.name).toLowerCase();
>           return an.localeCompare(bn);
>         });
>
>       if (pages.length === 0) {
>         dv.paragraph("Nenhum membro encontrado.");
>       } else {
>         let html = `<div class="wiki-related">`;
>
>         for (const p of pages) {
>           let img = String(p.image || "").trim();
>           if (!img) img = "Imagens/placeholder.png";
>
>           img = img.replace(/^!\[\[|\]\]$/g, "").replace(/^["']|["']$/g, "");
>
>           const imageFile = app.metadataCache.getFirstLinkpathDest(img, p.file.path);
>           const src = imageFile ? app.vault.getResourcePath(imageFile) : "";
>
>           const filePath = String(p.file.path).replace(/"/g, "&quot;");
>           const fileName = String(p.name || p.file.name);
>
>           html += `
>             <div class="wiki-related-card" data-href="${filePath}">
>               <div class="wiki-related-img">
>                 ${src ? `<img src="${src}" alt="${fileName}">` : ""}
>               </div>
>               <div class="wiki-related-name">${fileName}</div>
>             </div>
>           `;
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