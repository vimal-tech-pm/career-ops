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
12. **Content budget — HARD RULE: the final PDF MUST fill exactly 2 full pages. No half-empty pages. No 3-page overflow.**
13. **All work experience MUST appear — HARD RULE: every role from cv.md must be present in the final PDF, no exceptions. Never drop a role entirely.**
14. **Content density targets (apply BEFORE generating HTML — do not generate and then check):**
    With the current template (0.6in margins, 11px body, line-height 1.5), a full 2-page letter/A4 PDF holds approximately 85-95 content lines. Use these targets as your starting point:
    - **Professional Summary:** 4-5 lines (not 3 — use the space)
    - **Core Competencies:** keep as-is from JD keyword extraction
    - **Top 2 most relevant roles:** 4-5 bullets each, with at least 2 bullets wrapping to 2 lines. Pull additional proof points from article-digest.md.
    - **Mid-tier roles:** 2-3 bullets each
    - **Older/less relevant roles (last 2-3 in reverse chronological order):** 1-2 bullets each. If the JD values delivery, consulting, QA, or implementation, expand to 2 bullets.
    - **Projects:** 3 projects, each with a 2-line description (not 1-line summaries)
    - **Education, Certifications, Skills:** keep as-is (these are fixed-length)
    **Mental math check: before generating HTML, count your approximate lines. If total is under 80, you need more content. If over 95, compress.**
15. **Space-filling priority order (if estimated content is under ~85 lines, apply in sequence):**
    1. Expand top roles from 4 to 5 bullets using article-digest.md proof points
    2. Expand older roles from 1 to 2 bullets each
    3. Expand project descriptions from 2 to 3 lines each
    4. Add a 4th project
    5. Add a "Key Achievements" callout row under the most relevant role (3 metrics in a tight inline list)
    **NEVER leave visible whitespace greater than one blank line anywhere on the page.**
16. **Compression priority order (if estimated content exceeds ~95 lines, apply in sequence):**
    1. Trim older roles to 1 bullet each, starting from the last two in reverse chronological order — but KEEP them visible
    2. Reduce Summary to 3 lines
    3. Drop the least-relevant project (keep minimum 2)
    4. Trim bullets on mid-tier roles to 2 each
    **NEVER drop a role entirely to save space.**
17. Generate complete HTML from template + personalized content
18. Read `name` from `config/profile.yml` → normalize to kebab-case lowercase (e.g. "Vimal Sekar" → "vimal-sekar") → `{candidate}`
19. Write HTML to `/tmp/cv-{candidate}-{company}.html`
20. Run: `node generate-pdf.mjs /tmp/cv-{candidate}-{company}.html output/cv-{candidate}-{company}-{YYYY-MM-DD}.pdf --format={letter|a4}`
21. Report: PDF path, page count (must be 2), % keyword coverage, list of all roles included (verify none were dropped)
22. **Retry if page count ≠ 2:** If the rendered PDF is not exactly 2 pages, adjust using the space-filling ladder (step 15) or compression ladder (step 16), re-generate HTML, and re-render. Maximum 2 retries.

## ATS Rules (clean parsing)

- Single-column layout (no sidebars, no parallel columns)
- Standard headers: "Professional Summary", "Work Experience", "Education", "Skills", "Certifications", "Projects"
- No text in images/SVGs
- No critical info in PDF headers/footers (ATS ignores them)
- UTF-8, selectable text (not rasterized)
- No nested tables
- JD keywords distributed: Summary (top 5), first bullet of each role, Skills section

## PDF Design

- **Fonts**: Calibri (Carlito-Bold, 700) headings + DM Sans (body, 400-500)
- **Fonts self-hosted**: `fonts/`
- **Header**: name in Calibri 26-28px bold + gradient line `linear-gradient(to right, hsl(187,74%,32%), #2563b0)` 2px + contact row
- **Section headers**: Calibri 11-12px, uppercase, letter-spacing 0.06em, cyan primary `hsl(187,74%,32%)`
- **Body**: DM Sans 10.5-11px, line-height 1.5
- **Company names**: accent blue `#2563b0`
- **Primary text**: `#1a1a2e`; secondary text: `#555555` (no other mid-grays)
- **Margins**: 0.6in
- **Background**: pure white

## Section Order (optimized for "6-second recruiter scan")

1. Header (large name, gradient, contact, portfolio link)
2. Professional Summary (3-5 lines, keyword-dense)
3. Core Competencies (6-8 keyword phrases in flex-grid)
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

Use the template in `templates/cv-template.html`. Replace the `{{...}}` placeholders with personalized content:

| Placeholder | Content |
|-------------|-----------|
| `{{LANG}}` | `en` or `es` |
| `{{PAGE_WIDTH}}` | `8.5in` (letter) or `210mm` (A4) |
| `{{NAME}}` | (from profile.yml) |
| `{{CONTACT_ITEMS}}` | Header contact HTML built from `config/profile.yml`: email, phone, LinkedIn, portfolio if present, location |
| `{{SECTION_SUMMARY}}` | Professional Summary / Resumen Profesional |
| `{{SUMMARY_TEXT}}` | Personalized summary with keywords |
| `{{SECTION_COMPETENCIES}}` | Core Competencies / Competencias Core |
| `{{COMPETENCIES}}` | `<span class="competency-tag">keyword</span>` × 6-8 |
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

Same content generation as the HTML flow (Steps 1-11 above), plus the same layout rules (Steps 12-16):
- Rewrite Professional Summary with JD keywords + exit narrative
- Reorder experience bullets by JD relevance
- Select top competencies from JD requirements
- Inject keywords naturally (NEVER invent)
- **All work experience must appear** (step 13 applies here too — no role may be dropped)
- **Use content density targets from step 14** — same bullet counts and line estimates apply
- **Target 2 full pages** (step 12 applies — use space-filling/compression ladders as needed)

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
e. Show the user the final preview and ask for approval
f. `commit-editing-transaction` to save (ONLY after user approval)

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
