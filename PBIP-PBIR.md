Here is your updated, clean, beautifully formatted Markdown file — ready to paste directly into GitHub.  
The tree structures now render perfectly on GitHub, GitLab, VS Code, etc.

```markdown
November 2025 – Current Format

## Part 1: Theory – What are .pbip and .pbir?

| Format   | Full Name                | What it actually is                                                                 | Main benefit                                      |
|----------|--------------------------|--------------------------------------------------------------------------------------|---------------------------------------------------|
| **.pbip**| Power BI Project         | A **folder** that contains the full report + dataset in text files                  | Perfect for Git, teams and DevOps                 |
| **.pbir**| Power BI Item Resource   | Tiny pointer file that tells Power BI Desktop “this folder is a report/dataset”     | Makes the folder open like a normal file          |

### Why Microsoft created .pbip + .pbir
- Old **.pbix** → one big binary file → terrible Git diffs and merge conflicts  
- New **.pbip** → everything is readable text (JSON + TMDL + DAX) → clean diffs, easy collaboration

### The two .pbir files you always see
- `Project.Report/definition.pbir` → “this folder is the report”  
- `Project.SemanticModel/definition.pbir` → “this folder is the dataset”  
(You never edit them manually – Power BI manages them.)

## Part 2: Practical Example – Full Folder Structure

Project name example: **AdventureWorks Sales.pbip**

### Root level
```plaintext
AdventureWorks Sales.pbip                 ← double-click to open in Power BI Desktop
AdventureWorks Sales.Report/              ← visuals, pages, themes
AdventureWorks Sales.SemanticModel/       ← tables, measures, relationships
.gitignore                                ← tells Git what to ignore
```

### 1. Report folder – AdventureWorks Sales.Report
```plaintext
AdventureWorks Sales.Report
├── definition.pbir                       ← pointer: “this is a report”
├── .pbi
│   └── localSettings.json                ← personal settings (don’t commit)
├── definition
│   ├── report.json                       ← all pages, visuals, filters, bookmarks
│   ├── version.json                      ← format version
│   └── pages                             ← one subfolder per report page
│       ├── Overview
│       └── Details
└── StaticResources
    └── SharedResources
        └── BaseThemes
            └── CY24SU10.json             ← current Microsoft default theme
```

### 2. Semantic Model folder – AdventureWorks Sales.SemanticModel  
(Modern TMDL format – recommended for all new projects)

```plaintext
AdventureWorks Sales.SemanticModel
├── definition.pbir                       ← pointer: “this is a dataset”
├── definition.pblsm                      ← legacy file – only present in very old/non-TMDL projects (safe to ignore)
├── diagramLayout.json                    ← position of tables in Model view
├── .pbi
│   ├── cache.abf                         ← performance cache (ignore in Git)
│   └── localSettings.json
└── definition                            ← REAL model – everything here is text!
    ├── database.tmdl                     ← declares this is a Power BI model
    ├── model.tmdl                        ← Import / DirectQuery mode, default settings
    ├── relationships.tmdl                ← all relationships (one file in small–medium models)
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
    │   └── en-US.tmdl                    ← translations (add fr-FR.tmdl, es-ES.tmdl, etc.)
    └── roles
        └── Europe Managers.tmdl          ← row-level security
```

### Quick Cheat Sheet – “Which file do I edit?”

| You want to change         | File to open                                                                 |
|----------------------------|------------------------------------------------------------------------------|
| Report pages & visuals     | `…Report/definition/report.json`                                             |
| Add a new page             | `…Report/definition/pages/`                                                 |
| Add/edit a measure         | `…SemanticModel/definition/tables/TableName/measures/MeasureName.dax`        |
| Rename a column            | `…SemanticModel/definition/tables/TableName/columns/ColumnName.tmdl`        |
| Relationships              | `…SemanticModel/definition/relationships.tmdl`                              |
| Translations               | `…SemanticModel/definition/cultures/fr-FR.tmdl`                              |
| Row-level security         | `…SemanticModel/definition/roles/RoleName.tmdl`                              |

### Final Summary
A **.pbip** is simply a normal folder with human-readable files instead of one opaque .pbix file.  
The tiny **.pbir** files are just signposts so Power BI Desktop knows how to open each part correctly.

```

Just replace the old content with this — the trees now display perfectly on GitHub and everywhere else.
```
