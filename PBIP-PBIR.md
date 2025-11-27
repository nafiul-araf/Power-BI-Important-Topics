# PBIP & PBIR Format – Complete Guide (Updated: November 2025)

## 🧠 Part 1: Understanding the New Power BI File Formats

### What are `.pbip` and `.pbir`?

| Format     | Full Name                 | What It Actually Is                                                            | Main Benefit                                   |
|------------|---------------------------|---------------------------------------------------------------------------------|------------------------------------------------|
| **.pbip**  | Power BI Project          | A **folder-based project** containing the full report + semantic model in text | Perfect for Git, collaboration, DevOps, CI/CD  |
| **.pbir**  | Power BI Item Resource    | A small pointer file telling Power BI that “this folder is a report/dataset”   | Lets folders behave like openable files        |

### Why Microsoft Introduced This Format
- **.pbix (old)** → single binary file → bad diffs, merge conflicts, not version-control friendly  
- **.pbip (new)** → human-readable text files (JSON + TMDL + DAX) → clean diffs, teamwork-friendly

### The Two `.pbir` Files You Always See
- `Project.Report/definition.pbir` → marks the folder as a **report**  
- `Project.SemanticModel/definition.pbir` → marks the folder as a **dataset**  

> You never manually edit `.pbir` files — Power BI Desktop manages them automatically.

---

## 📁 Part 2: Full Project Folder Structure Example

Project example: **AdventureWorks Sales.pbip**

## 🔷 Root Level Structure

```plaintext
AdventureWorks Sales.pbip                 ← double-click to open the whole project
AdventureWorks Sales.Report/              ← report (visuals, pages, themes)
AdventureWorks Sales.SemanticModel/       ← semantic model (tables, measures, relationships)
.gitignore                                ← Git ignore rules
````

---

## 🔵 1. Report Folder — `AdventureWorks Sales.Report`

```plaintext
AdventureWorks Sales.Report
├── definition.pbir                       ← pointer: “this is a report”
├── .pbi
│   └── localSettings.json                ← personal settings (exclude from Git)
├── definition
│   ├── report.json                       ← visuals, pages, bookmarks, filters
│   ├── version.json                      ← schema and metadata version
│   └── pages                             ← one folder per report page
│       ├── Overview
│       └── Details
└── StaticResources
    └── SharedResources
        └── BaseThemes
            └── CY24SU10.json             ← default Power BI theme file
```

---

## 🟢 2. Semantic Model Folder — `AdventureWorks Sales.SemanticModel`

Modern **TMDL-based** dataset (recommended for all new projects).

```plaintext
AdventureWorks Sales.SemanticModel
├── definition.pbir                       ← pointer: “this is a dataset”
├── definition.pblsm                      ← legacy model file (ignore unless inherited)
├── diagramLayout.json                    ← table positions in Model View
├── .pbi
│   ├── cache.abf                         ← performance cache (ignore in Git)
│   └── localSettings.json
└── definition                            ← the TRUE semantic model (TMDL + DAX)
    ├── database.tmdl                     ← declares it as a Power BI database/model
    ├── model.tmdl                        ← model mode (Import/DirectQuery) & metadata
    ├── relationships.tmdl                ← relationships for all tables
    ├── tables
    │   ├── Sales
    │   │   ├── Sales.tmdl
    │   │   ├── columns
    │   │   │   ├── Order Date.tmdl
    │   │   │   └── Sales Amount.tmdl
    │   │   └── measures
    │   │       ├── Total Sales.dax
    │   │       └── Profit.dax
    │   └── Customer
    │       └── Customer.tmdl
    ├── cultures
    │   └── en-US.tmdl                    ← translations (add fr-FR, es-ES, etc.)
    └── roles
        └── Europe Managers.tmdl          ← row-level security
```

---

## 📝 Quick Cheat Sheet — What File Should I Edit?

| Task You Want to Do    | File to Edit                                                      |
| ---------------------- | ----------------------------------------------------------------- |
| Edit visuals/pages     | `…Report/definition/report.json`                                  |
| Add a new page         | `…Report/definition/pages/`                                       |
| Add/edit a measure     | `…SemanticModel/definition/tables/<Table>/measures/<Measure>.dax` |
| Rename/modify a column | `…SemanticModel/definition/tables/<Table>/columns/<Column>.tmdl`  |
| Edit relationships     | `…SemanticModel/definition/relationships.tmdl`                    |
| Add/edit translations  | `…SemanticModel/definition/cultures/<CultureCode>.tmdl`           |
| Edit RLS roles         | `…SemanticModel/definition/roles/<RoleName>.tmdl`                 |

---

## ✅ Final Summary

A **.pbip** project is simply a folder-based version of a Power BI report + dataset.
Everything is **transparent**, **text-based**, and **version-control friendly**.

`.pbir` files are just **tiny signposts** telling Power BI Desktop how to open each part of the project.

This modern structure makes Power BI development far more maintainable, Git-friendly, and professional.

```
