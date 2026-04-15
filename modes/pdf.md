# Modo: pdf — Generación de PDF ATS-Optimizado

## Pipeline completo

1. Lee `cv.md` como fuentes de verdad
2. Pide al usuario el JD si no está en contexto (texto o URL)
3. Extrae 15-20 keywords del JD
4. Detecta idioma del JD → idioma del CV (EN default)
5. Detecta ubicación empresa → formato papel:
   - US/Canada → `letter`
   - Resto del mundo → `a4`
6. Detecta arquetipo del rol → adapta framing
7. Adapta el Professional Summary para este JD:
   - PARTE del resumen existente en cv.md — NO reescribas desde cero
   - Inyecta keywords del JD QUIRÚRGICAMENTE: inserta un keyword en una frase existente o añádelo al final; no reestructures frases enteras
   - Reglas de voz (ver también _shared.md "Professional Writing & ATS Compatibility"):
     * Primera persona implícita: empieza con verbos de acción fuertes; sin sujeto "I"; NUNCA en tercera persona ("He led...", "Vimal is...", "The candidate...")
     * Sin voz pasiva ("was responsible for" → "owned", "built", "led")
     * Sin corporate filler (ver lista en _shared.md: leveraged, spearheaded, facilitated, passionate about, proven track record, robust, seamless, cutting-edge, synergies, best practices, etc.)
   - Preserva todas las métricas de cv.md exactamente como están (14+ years, 125,000+, NPS 26→70, $420K+)
   - Closing bridge cuando sea relevante al JD: "Building AI-native products independently since 2025. Now applying that end-to-end product thinking to [JD domain]."
   - Máximo 4 líneas / ~70 palabras
8. Selecciona top 3-4 proyectos más relevantes para la oferta

8a. Aplica content budget ANTES de seleccionar bullets (objetivo: exactamente 2 páginas PDF):

    | Antigüedad del rol       | Máx. bullets |
    |--------------------------|--------------|
    | Actual (0–2 años)        | 6            |
    | Reciente (2–5 años)      | 4            |
    | Anterior (5–10 años)     | 3            |
    | Primera carrera (10+ años) | 2          |

    Aplicado al orden canónico de este CV:
    - Thomson Reuters (Jun 2021, actual):       máx. 6 bullets — elige top 6 por relevancia al JD
    - Self-Employed (Jun 2025, actual):         máx. 4 bullets — tiene 3, conserva todos, añade 1 si hay espacio
    - Cognizant PO (Jul 2016, reciente):        máx. 4 bullets — elige top 4 por relevancia al JD
    - Cognizant Sr BA (Apr 2014, anterior):     máx. 3 bullets — tiene 3, conserva todos
    - HCL Technologies (May 2012, anterior):    máx. 3 bullets — elige top 3 por relevancia al JD
    - Murugappa Group (Apr 2011, +15 años):     máx. 2 bullets — tiene 2, conserva todos
    - Infosys (Jul 2008, +15 años):             máx. 2 bullets — elige top 2 por relevancia al JD

    Si el contenido sigue desbordando 2 páginas tras aplicar los límites:
    1. Reduce Murugappa + Infosys a 1 bullet cada uno
    2. Reduce HCL a 2 bullets
    3. Reduce Cognizant Sr BA a 2 bullets
    Nunca reduzcas los dos roles actuales (Thomson Reuters y Self-Employed) por debajo de su selección.

9. Reordena bullets DENTRO de cada trabajo por relevancia al JD. El orden de los trabajos en sí DEBE coincidir exactamente con el orden de cv.md — NO intercambies, muevas ni reordenes bloques de trabajo completos. Solo los bullets dentro de un mismo trabajo pueden reordenarse.

9a. Orden canónico de experiencia para este CV (coincide con el orden del archivo cv.md — no modificar):
    1. Thomson Reuters | Product Manager            | Jun 2021 – Present  ← siempre primero
    2. Self-Employed   | AI Product Builder          | Jun 2025 – Present
    3. Cognizant       | Product Owner & Consultant  | Jul 2016 – Jun 2021
    4. Cognizant       | Senior BA & BD Lead         | Apr 2014 – Jul 2016
    5. HCL Technologies| BA & Solution Consultant    | May 2012 – Mar 2014
    6. Murugappa Group | Mgmt Consultant Intern      | Apr 2011 – Jun 2011
    7. Infosys         | Software Test Engineer      | Jul 2008 – Jun 2010
    Verifica este orden en el HTML generado antes de escribirlo a disco.
10. Construye competency grid desde requisitos del JD (6-8 keyword phrases)
11. Inyecta keywords naturalmente en logros existentes (NUNCA inventa)
12. Genera HTML completo desde template + contenido personalizado
13. Lee `name` de `config/profile.yml` → normaliza a kebab-case lowercase (e.g. "John Doe" → "john-doe") → `{candidate}`
14. Escribe HTML a `/tmp/cv-{candidate}-{company}.html`
15. Ejecuta: `node generate-pdf.mjs /tmp/cv-{candidate}-{company}.html output/cv-{candidate}-{company}-{YYYY-MM-DD}.pdf --format={letter|a4}`

15a. Verifica el número de páginas en la salida de generate-pdf.mjs (línea "📊 Pages: N"):
     - Pages = 2 → continúa al reporte
     - Pages = 1 → contenido escaso; amplía Thomson Reuters a 8 bullets, Self-Employed a 4, regenera HTML y vuelve a ejecutar
     - Pages ≥ 3 → desbordamiento; aplica la secuencia de trim del Step 8a, regenera HTML y vuelve a ejecutar
     - Máximo 2 intentos de regeneración; si sigue siendo incorrecto tras el intento 2, reporta el número de páginas actual y las secciones específicas a recortar al usuario

16. Reporta: ruta del PDF, nº páginas, % cobertura de keywords

## Reglas ATS (parseo limpio)

- Layout single-column (sin sidebars, sin columnas paralelas)
- Headers estándar: "Professional Summary", "Work Experience", "Education", "Skills", "Certifications", "Projects"
- Sin texto en imágenes/SVGs
- Sin info crítica en headers/footers del PDF (ATS los ignora)
- UTF-8, texto seleccionable (no rasterizado)
- Sin tablas anidadas
- Keywords del JD distribuidas: Summary (top 5), primer bullet de cada rol, Skills section

## Diseño del PDF

- **Fonts**: Space Grotesk (headings, 600-700) + DM Sans (body, 400-500)
- **Fonts self-hosted**: `fonts/`
- **Header**: nombre en Space Grotesk 24px bold + línea gradiente `linear-gradient(to right, hsl(187,74%,32%), hsl(270,70%,45%))` 2px + fila de contacto
- **Section headers**: Space Grotesk 13px, uppercase, letter-spacing 0.05em, color cyan primary
- **Body**: DM Sans 11px, line-height 1.5
- **Company names**: color accent purple `hsl(270,70%,45%)`
- **Márgenes**: 0.6in
- **Background**: blanco puro

## Orden de secciones (optimizado "6-second recruiter scan")

1. Header (nombre grande, gradiente, contacto, link portfolio)
2. Professional Summary (3-4 líneas, keyword-dense)
3. Core Competencies (6-8 keyword phrases en flex-grid)
4. Work Experience (cronológico inverso)
5. Projects (top 3-4 más relevantes)
6. Education & Certifications
7. Skills (idiomas + técnicos)

## Estrategia de keyword injection (ético, basado en verdad)

Ejemplos de reformulación legítima:
- JD dice "RAG pipelines" y CV dice "LLM workflows with retrieval" → cambiar a "RAG pipeline design and LLM orchestration workflows"
- JD dice "MLOps" y CV dice "observability, evals, error handling" → cambiar a "MLOps and observability: evals, error handling, cost monitoring"
- JD dice "stakeholder management" y CV dice "collaborated with team" → cambiar a "stakeholder management across engineering, operations, and business"

**NUNCA añadir skills que el candidato no tiene. Solo reformular experiencia real con el vocabulario exacto del JD.**

## Template HTML

Usar el template en `cv-template.html`. Reemplazar los placeholders `{{...}}` con contenido personalizado:

| Placeholder | Contenido |
|-------------|-----------|
| `{{LANG}}` | `en` o `es` |
| `{{PAGE_WIDTH}}` | `8.5in` (letter) o `210mm` (A4) |
| `{{NAME}}` | (from profile.yml) |
| `{{CONTACT_ITEMS}}` | Header contact HTML built from `config/profile.yml`: email, phone, LinkedIn, portfolio if present, location |
| `{{SECTION_SUMMARY}}` | Professional Summary / Resumen Profesional |
| `{{SUMMARY_TEXT}}` | Summary personalizado con keywords |
| `{{SECTION_COMPETENCIES}}` | Core Competencies / Competencias Core |
| `{{COMPETENCIES}}` | `<span class="competency-tag">keyword</span>` × 6-8 |
| `{{SECTION_EXPERIENCE}}` | Work Experience / Experiencia Laboral |
| `{{EXPERIENCE}}` | HTML de cada trabajo con bullets reordenados |
| `{{SECTION_PROJECTS}}` | Projects / Proyectos |
| `{{PROJECTS}}` | HTML de top 3-4 proyectos |
| `{{SECTION_EDUCATION}}` | Education / Formación |
| `{{EDUCATION}}` | HTML de educación |
| `{{SECTION_CERTIFICATIONS}}` | Certifications / Certificaciones |
| `{{CERTIFICATIONS}}` | HTML de certificaciones |
| `{{SECTION_SKILLS}}` | Skills / Competencias |
| `{{SKILLS}}` | HTML de skills |

### Contact row generation

Build `{{CONTACT_ITEMS}}` from `candidate` in `config/profile.yml` in this exact order:

1. `candidate.email`
2. `candidate.phone`
3. `candidate.linkedin`
4. `candidate.portfolio_url` if present
5. `candidate.location`

Render each present item as a `<span>` or `<a>` and render `<span class="separator">|</span>` only between present items. If `phone` or `portfolio_url` is missing, omit that item and its separator. Never leave leading, trailing, doubled, or empty separators.

## Canva CV Generation (optional)

If `config/profile.yml` has `canva_resume_design_id` set, offer the user a choice before generating:
- **"HTML/PDF (fast, ATS-optimized)"** — existing flow above
- **"Canva CV (visual, design-preserving)"** — new flow below

If the user has no `canva_resume_design_id`, skip this prompt and use the HTML/PDF flow.

### Canva workflow

#### Step 1 — Duplicate the base design

a. `export-design` the base design (using `canva_resume_design_id`) as PDF → get download URL
b. `import-design-from-url` using that download URL → creates a new editable design (the duplicate)
c. Note the new `design_id` for the duplicate

#### Step 2 — Read the design structure

a. `get-design-content` on the new design → returns all text elements (richtexts) with their content
b. Map text elements to CV sections by content matching:
   - Look for the candidate's name → header section
   - Look for "Summary" or "Professional Summary" → summary section
   - Look for company names from cv.md → experience sections
   - Look for degree/school names → education section
   - Look for skill keywords → skills section
c. If mapping fails, show the user what was found and ask for guidance

#### Step 3 — Generate tailored content

Same content generation as the HTML flow (Steps 1-11 above):
- Rewrite Professional Summary with JD keywords + exit narrative
- Reorder experience bullets by JD relevance
- Select top competencies from JD requirements
- Inject keywords naturally (NEVER invent)

**IMPORTANT — Character budget rule:** Each replacement text MUST be approximately the same length as the original text it replaces (within ±15% character count). If tailored content is longer, condense it. The Canva design has fixed-size text boxes — longer text causes overlapping with adjacent elements. Count the characters in each original element from Step 2 and enforce this budget when generating replacements.

#### Step 4 — Apply edits

a. `start-editing-transaction` on the duplicate design
b. `perform-editing-operations` with `find_and_replace_text` for each section:
   - Replace summary text with tailored summary
   - Replace each experience bullet with reordered/rewritten bullets
   - Replace competency/skills text with JD-matched terms
   - Replace project descriptions with top relevant projects
c. **Reflow layout after text replacement:**
   After applying all text replacements, the text boxes auto-resize but neighboring elements stay in place. This causes uneven spacing between work experience sections. Fix this:
   1. Read the updated element positions and dimensions from the `perform-editing-operations` response
   2. For each work experience section (top to bottom), calculate where the bullets text box ends: `end_y = top + height`
   3. The next section's header should start at `end_y + consistent_gap` (use the original gap from the template, typically ~30px)
   4. Use `position_element` to move the next section's date, company name, role title, and bullets elements to maintain even spacing
   5. Repeat for all work experience sections
d. **Verify layout before commit:**
   - `get-design-thumbnail` with the transaction_id and page_index=1
   - Visually inspect the thumbnail for: text overlapping, uneven spacing, text cut off, text too small
   - If issues remain, adjust with `position_element`, `resize_element`, or `format_text`
   - Repeat until layout is clean
d. Show the user the final preview and ask for approval
e. `commit-editing-transaction` to save (ONLY after user approval)

#### Step 5 — Export and download PDF

a. `export-design` the duplicate as PDF (format: a4 or letter based on JD location)
b. **IMMEDIATELY** download the PDF using Bash:
   ```bash
   curl -sL -o "output/cv-{candidate}-{company}-canva-{YYYY-MM-DD}.pdf" "{download_url}"
   ```
   The export URL is a pre-signed S3 link that expires in ~2 hours. Download it right away.
c. Verify the download:
   ```bash
   file output/cv-{candidate}-{company}-canva-{YYYY-MM-DD}.pdf
   ```
   Must show "PDF document". If it shows XML or HTML, the URL expired — re-export and retry.
d. Report: PDF path, file size, Canva design URL (for manual tweaking)

#### Error handling

- If `import-design-from-url` fails → fall back to HTML/PDF pipeline with message
- If text elements can't be mapped → warn user, show what was found, ask for manual mapping
- If `find_and_replace_text` finds no matches → try broader substring matching
- Always provide the Canva design URL so the user can edit manually if auto-edit fails

## Post-generación

Actualizar tracker si la oferta ya está registrada: cambiar PDF de ❌ a ✅.
