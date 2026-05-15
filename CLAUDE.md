# CLAUDE.md — Python Prep Course

## Project overview

This is the **Henry Python Prep Course** — a structured, self-paced preparatory curriculum for students applying to Henry's Data Science career. The repository contains 9 numbered lesson modules plus supplementary sections, each with written content (Markdown), Jupyter notebooks, solved homework files (`.ipynb` / `.py`), and homework prompts (`.md`).

The content is **bilingual in practice**: lesson prose is in Spanish, but code is standard Python 3.

An **Eleventy (11ty)** static-site generator renders the Markdown files into a hosted website (`data.prep.soyhenry.com`). The site build is a secondary concern; the primary artifacts are the lesson content and homework files.

---

## Repository structure

```
Python-Prep/
├── 01 - Introducción a la Programación/   # Lesson 1: tooling setup + intro
├── 02 - Variables y Tipos de Datos/       # Lesson 2: variables & data types
├── 03 - Flujos de Control/                # Lesson 3: control flow (if/else/loops)
├── 04 - Estructuras de datos/             # Lesson 4: lists, dicts, sets, tuples
├── 05 - Iteradores e Iterables/           # Lesson 5: iterators & iterables
├── 06 - Funciones/                        # Lesson 6: functions
├── 07 - Classes & OOP/                    # Lesson 7: OOP & classes
├── 08 - Error Handling/                   # Lesson 8: exceptions & error handling
├── 09 - Entrada-Salida y Manejo de Archivos/ # Lesson 9: I/O & file handling
├── 10 - Repaso/                           # Lesson 10: review session
├── 11 - HC/                               # Henry Challenge info
├── Prep_Course 01/                        # Archived class recordings index
├── _src/
│   ├── assets/                            # Images referenced in lessons
│   ├── data/
│   │   ├── config.json                    # Site title & Airtable form ID
│   │   ├── layout.js                      # Default layout assignment
│   │   └── styles/                        # JS modules exporting CSS strings
│   └── layouts/
│       ├── intro.njk                      # Landing page layout
│       └── lesson.njk                     # Per-lesson layout
├── .eleventy.js                           # Eleventy build configuration
├── .eleventyignore                        # Excludes node_modules & homework dirs
├── .gitignore                             # Excludes node_modules, _dist, _cache
├── package.json                           # Node dependencies & npm scripts
└── README.md                              # Course welcome page / intro lesson
```

### Per-lesson directory layout

Each numbered module follows this consistent pattern:

| File | Role |
|---|---|
| `README.md` | Lesson content rendered by Eleventy |
| `README.json` | Eleventy front-matter data (title, permalink, nav order) |
| `Prep_Course_Homework_NN.md` | Student homework prompt |
| `Prep_Course_Homework_NN-Resuelto.ipynb` | Solved homework (notebook) |
| `Prep_Course_Homework_NN-Resuelto.py` | Solved homework exported as plain Python |
| `Prep_Course_Homework_NN-Resuelto Clase.ipynb` | In-class version of the solution |
| `.ipynb_checkpoints/` | Jupyter auto-save checkpoints (not for editing) |

---

## Technology stack

| Layer | Technology |
|---|---|
| Content | Markdown (`.md`) |
| Exercises | Jupyter Notebooks (`.ipynb`) + plain Python (`.py`) |
| Site generation | [Eleventy 0.12](https://www.11ty.dev/) |
| Templating | Nunjucks (`.njk`) |
| Markdown renderer | `markdown-it` with anchors and syntax highlighting |
| Navigation | `@11ty/eleventy-navigation` + `eleventy-navigation-bootstrap` |
| Table of contents | `eleventy-plugin-toc` |
| UI framework | Bootstrap 5.1 (via CDN) |
| Testing | Jest 27 (for homework auto-grading) |
| Linting | ESLint + Standard |

---

## npm scripts

```bash
npm start        # Run Eleventy dev server with live reload
npm run build    # Build static site into _dist/
npm test         # Run Jest homework tests (with coverage disabled)
```

The output directory is `_dist/`. Never commit `_dist/`, `_cache/`, or `node_modules/`.

---

## Eleventy configuration (`.eleventy.js`)

Key decisions:
- **Layouts** live in `/_src/layouts/`, **global data** in `/_src/data/`.
- Static assets (`_src/assets/`) are passed through as-is.
- A custom linter warns on common misspellings (`lenght`, `rigth`) found in `.md` files.
- The default layout for all lessons is `lesson.njk` (set via `_src/data/layout.js`).

### README.json front-matter fields

Each `README.json` controls the lesson's Eleventy metadata:

```json
{
  "lessonTitle": "Variables y Tipos de Datos",   // shown in page header
  "permalink": "/Clase_Dos/",                     // URL path
  "eleventyNavigation": {
    "key": "Tipos de Datos",                      // nav label
    "order": 2                                    // nav sort position
  },
  "feedbackID": "02 - Variables y Tipos de Datos" // Airtable feedback form
}
```

Optional fields in `README.json`:
- `quizzID` — links to a Henry quiz
- `homeworkUrl` — links to homework

---

## Lesson content conventions

- Lessons are written in **Spanish**.
- Code blocks use standard Python 3 syntax.
- Embedded videos use `<div class="iframeContainer"><iframe ...></iframe></div>`.
- Feedback links point to Airtable using the form ID in `_src/data/config.json`.
- Image paths reference `/_src/assets/` using absolute-from-root paths (e.g. `/_src/assets/01_imagen01.jpg`).
- Do not use relative `../` paths for images — the Eleventy build resolves them from the site root.

---

## Homework files

- Homework **prompts** (`Prep_Course_Homework_NN.md`) list numbered exercises in Spanish.
- Solved notebooks (`-Resuelto.ipynb`) contain cell-by-cell answers.
- Exported Python files (`-Resuelto.py`) are auto-generated from notebooks; the `.ipynb` is the source of truth.
- Homework directories are excluded from the Eleventy build via `.eleventyignore` (`**/homework`).

---

## Python conventions (lesson code)

- Python 3 throughout; files declare `# coding: utf-8` when needed.
- Naming: `snake_case` for variables and functions, `PascalCase` for classes.
- Helper module: `07 - Classes & OOP/herramientas.py` — a `Herramientas` class with utility methods (prime check, factorial, temperature conversion, mode). Private methods use the `__dunder` prefix convention.
- No external Python package dependencies in lesson code — stdlib only.

---

## Git workflow

- **Main branch**: `main` — production content.
- **Feature branches**: follow the pattern `claude/<description>`.
- Commit messages have historically been brief (`Update README.md`). Prefer descriptive messages for structural changes.
- The `.gitattributes` file is present; check it before committing binary files.

---

## What NOT to do

- Do not commit `node_modules/`, `_dist/`, or `_cache/`.
- Do not modify `.ipynb_checkpoints/` files directly.
- Do not use relative image paths (`../`) in lesson Markdown — use absolute paths from site root.
- Do not add comments to Python solutions explaining *what* the code does — the exercises are self-documenting by their numbered prompts.
- Do not introduce Python packages beyond the stdlib in lesson code without updating instructions for students.
- Do not change `_src/data/config.json` fields (`airtableFormId`, `repoTitle`) without coordinating with the Henry team.
