# Mode: pdf — ATS-Optimized PDF Generation

## Complete Pipeline

1. Read `cv.md` as the base source of truth and `article-digest.md` (if it exists) as a supplementary source of proof points
2. Ask the user for the JD if not already in context (text or URL)
3. Extract 15-20 keywords from the JD
4. Detect JD language → CV language (EN default)
5. Detect company location → paper format:
   - US/Canada → `letter`
   - Rest of the world → `a4`
6. Detect role archetype → adapt framing
7. Rewrite Professional Summary injecting JD keywords + exit narrative bridge ("Built and sold a business. Now applying systems thinking to [JD domain].")
8. Select top 3-4 most relevant projects for the offer
9. Reorder experience bullets by JD relevance
10. Build competency grid from JD requirements (6-8 keyword phrases)
11. Inject keywords naturally into existing achievements (NEVER invent)
12. **Content budget — HARD RULE: target exactly 2 pages. No more, no less. If content is running short, expand — never leave half a page blank. If running long, compress.**
13. **All work experience MUST appear — HARD RULE: every role from cv.md must be present in the final PDF, no exceptions. Older or less-relevant roles use compact format: company | role | date range + 1 tight bullet. Never drop a role entirely.**
14. **Space-filling priority order (use in sequence until 2 pages are filled):**
    1. Expand top 2-3 roles with additional bullets from article-digest.md (up to 5 bullets per role)
    2. Add a second or third relevant project from cv.md
    3. Expand compact older roles from 1 bullet to 2 bullets if JD-relevant (delivery, consulting, QA, implementation)
    4. Expand the Professional Summary from 3 lines to 4-5 lines
    5. Expand the Core Competencies grid from 6 to 8-10 tags
    6. Add a "Key Achievements" callout row under the most relevant role (3 metrics in a tight inline list)
    **NEVER leave visible whitespace greater than one blank line anywhere on the page.**
15. **Compression priority order (use in sequence if content exceeds 2 pages):**
    1. Trim older roles to 1 bullet each, starting from the last two in reverse chronological order — but KEEP them visible
    2. Reduce Summary to 3 lines
    3. Drop the least-relevant project (keep minimum 2)
    4. Trim bullets on mid-tier roles to 2 each
    **NEVER drop a role entirely to save space.**
16. Generate complete HTML from template + personalized content
17. Write HTML to `/tmp/cv-candidate-{company}.html`
18. Run: `node generate-pdf.mjs /tmp/cv-candidate-{company}.html output/cv-candidate-{company}-{YYYY-MM-DD}.pdf --format={letter|a4}`
19. Report: PDF path, page count (must be 2), % keyword coverage, list of all roles included (verify none were dropped)
20. **Retry if page count ≠ 2:** If the rendered PDF is not exactly 2 pages, adjust using the space-filling ladder (step 14) or compression ladder (step 15), re-generate HTML, and re-render. Maximum 2 retries.

## ATS Rules (clean parsing)

- Single-column layout (no sidebars, no parallel columns)
- Standard headers: "Professional Summary", "Work Experience", "Education", "Skills", "Certifications", "Projects"
- No text in images/SVGs
- No critical info in PDF headers/footers (ATS ignores them)
- UTF-8, selectable text (not rasterized)
- No nested tables
- JD keywords distributed: Summary (top 5), first bullet of each role, Skills section

## PDF Design

- **Fonts**: Space Grotesk (headings, 600-700) + DM Sans (body, 400-500)
- **Fonts self-hosted**: `fonts/`
- **Header**: name in Space Grotesk 24px bold + gradient line `linear-gradient(to right, hsl(187,74%,32%), hsl(270,70%,45%))` 2px + contact row
- **Section headers**: Space Grotesk 13px, uppercase, letter-spacing 0.05em, cyan primary color
- **Body**: DM Sans 11px, line-height 1.5
- **Company names**: accent purple `hsl(270,70%,45%)`
- **Margins**: 0.6in
- **Background**: pure white

## Section Order (optimized for "6-second recruiter scan")

1. Header (large name, gradient, contact, portfolio link)
2. Professional Summary (3-5 lines, keyword-dense)
3. Core Competencies (6-10 keyword phrases in flex-grid)
4. Work Experience (reverse chronological)
5. Projects (top 3-4 most relevant)
6. Education & Certifications
7. Skills (languages + technical)

## Keyword Injection Strategy (ethical, truth-based)

Legitimate reformulation examples:
- JD says "RAG pipelines" and CV says "LLM workflows with retrieval" → change to "RAG pipeline design and LLM orchestration workflows"
- JD says "MLOps" and CV says "observability, evals, error handling" → change to "MLOps and observability: evals, error handling, cost monitoring"
- JD says "stakeholder management" and CV says "collaborated with team" → change to "stakeholder management across engineering, operations, and business"

**NEVER add skills the candidate does not have. Only reformulate real experience using the exact vocabulary of the JD.**

**Supplementary source usage:**
- `cv.md` remains the canonical base of the CV
- `article-digest.md` is used to recover specificity that was compressed out of `cv.md`
- If a detail appears only in `article-digest.md`, use it to enrich bullets or recover relevant older experience — but keep the output shaped like a CV, not a dossier

## HTML Template

Use the template in `cv-template.html`. Replace the `{{...}}` placeholders with personalized content:

| Placeholder | Content |
|-------------|-----------|
| `{{LANG}}` | `en` or `es` |
| `{{PAGE_WIDTH}}` | `8.5in` (letter) or `210mm` (A4) |
| `{{NAME}}` | (from profile.yml) |
| `{{EMAIL}}` | (from profile.yml) |
| `{{LINKEDIN_URL}}` | [from profile.yml] |
| `{{LINKEDIN_DISPLAY}}` | [from profile.yml] |
| `{{PORTFOLIO_URL}}` | [from profile.yml] (or /es depending on language) |
| `{{PORTFOLIO_DISPLAY}}` | [from profile.yml] (or /es depending on language) |
| `{{LOCATION}}` | [from profile.yml] |
| `{{SECTION_SUMMARY}}` | Professional Summary / Resumen Profesional |
| `{{SUMMARY_TEXT}}` | Personalized summary with keywords |
| `{{SECTION_COMPETENCIES}}` | Core Competencies / Competencias Core |
| `{{COMPETENCIES}}` | `<span class="competency-tag">keyword</span>` × 6-10 |
| `{{SECTION_EXPERIENCE}}` | Work Experience / Experiencia Laboral |
| `{{EXPERIENCE}}` | HTML for each role with reordered bullets |
| `{{SECTION_PROJECTS}}` | Projects / Proyectos |
| `{{PROJECTS}}` | HTML for top 3-4 projects |
| `{{SECTION_EDUCATION}}` | Education / Formación |
| `{{EDUCATION}}` | HTML for education |
| `{{SECTION_CERTIFICATIONS}}` | Certifications / Certificaciones |
| `{{CERTIFICATIONS}}` | HTML for certifications |
| `{{SECTION_SKILLS}}` | Skills / Competencias |
| `{{SKILLS}}` | HTML for skills |

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

Same content generation as the HTML flow (Steps 1-11 above), plus the same layout rules (Steps 12-15):
- Rewrite Professional Summary with JD keywords + exit narrative
- Reorder experience bullets by JD relevance
- Select top competencies from JD requirements
- Inject keywords naturally (NEVER invent)
- **All work experience must appear** (step 13 applies here too — no role may be dropped)
- **Target 2 pages** (step 12 applies — use space-filling/compression ladders as needed)

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

## Post-generation

Update tracker if the offer is already registered: change PDF from ❌ to ✅.
