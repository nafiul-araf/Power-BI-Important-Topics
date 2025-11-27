# Power BI Project Files (.pbip and .pbir)

```markdown
# Power BI Project Files Explained (.pbip and .pbir)  
November 2025 – Current Format

## Part 1: Theory – What are .pbip and .pbir?

| Format   | Full Name                          | What it actually is                              | Who uses it                              | Main benefit                                    |
|----------|------------------------------------|--------------------------------------------------|------------------------------------------|-------------------------------------------------|
| **.pbip** | Power BI Project                   | A **folder** (not a single file) that contains the whole report + dataset | Developers, teams, Git/Azure DevOps users | Perfect for source control and team work        |
| **.pbir** | Power BI Item Resource             | A **small pointer file** that tells Power BI Desktop “this folder is a report” or “this folder is a dataset” | Power BI Desktop (automatically created) | Makes the folder open like a normal file        |

### Why Microsoft created .pbip + .pbir (instead of the old .pbix)
- Old .pbix = one big binary file → very hard to see who changed what in Git  
- New .pbip = everything is normal text files (JSON + TMDL + DAX) → Git shows exact changes  
- You can now have many people working on the same report without big merge problems

### The two .pbir files you will always see
1. `MyProject.Report/definition.pbir` → says “this folder is the report part”  
2. `MyProject.SemanticModel/definition.pbir` → says “this folder is the dataset part”

You almost never edit these two files yourself – Power BI creates and updates them.

## Part 2: Practical Example – Folder Structure (Real Project)

Imagine a project called **AdventureWorks Sales.pbip**

### Root level (what you see when you open the folder)
```
AdventureWorks Sales.pbip          ← double-click this to open in Power BI
AdventureWorks Sales.Report/       ← all visuals and pages
AdventureWorks Sales.SemanticModel/← all tables, calculations, relationships
.gitignore                         ← tells Git what to ignore
```

### 1. Report folder – AdventureWorks Sales.Report
```
AdventureWorks Sales.Report
│
├── definition.pbir                 ← pointer: “this is a report”
├── .pbi/
│   └── localSettings.json         ← your personal zoom, etc. (don’t commit)
│
├── definition/
│   ├── report.json                 ← ALL pages, visuals, filters, bookmarks
│   ├── version.json                ← format version
│   └── pages/                      ← one folder for each report page
│       ├── Overview/
│       └── Details/
│
└── StaticResources/
    └── SharedResources/
        └── BaseThemes/
            └── CY24SU10.json       ← current Microsoft default theme
```

### 2. Semantic Model folder – AdventureWorks Sales.SemanticModel  
(Uses the modern TMDL format – best for teams)

```
AdventureWorks Sales.SemanticModel
│
├── definition.pbism                ← legacy TMSL model definition (JSON schema for database objects); used only in non-TMDL projects.
├── diagramLayout.json              ← where you placed tables in Model view
│
├── .pbi/
│   ├── cache.abf                   ← fast-loading cache (ignore in Git)
│   └── localSettings.json
│
└── definition/                     ← REAL model files (all text!)
    ├── database.tmdl               ← says “this is a Power BI model”
    ├── model.tmdl                  ← Import or DirectQuery? Default language?
    ├── relationships.tmdl          ← all relationships (one file for small models)
    │
    ├── tables/                     ← one folder per table
    │   ├── Sales/
    │   │   ├── Sales.tmdl
    │   │   ├── columns/
    │   │   │   ├── Order Date.tmdl
    │   │   │   └── Sales Amount.tmdl
    │   │   └── measures/
    │   │       ├── Total Sales.dax
    │   │       └── Profit.dax
    │   └── Customer/
    │       └── Customer.tmdl
    │
    ├── cultures/
    │   └── en-US.tmdl               ← English names (you can add fr-FR.tmdl etc.)

```

### Quick “What file do I edit?” Cheat Sheet

| You want to change          | Exact file to open                                            |
|-----------------------------|---------------------------------------------------------------|
| Report pages / visuals     | …Report/definition/report.json                               |
| Add a new page              | …Report/definition/pages/                                    |
| Add a measure               | …SemanticModel/definition/tables/TableName/measures/Name.dax |
| Change a column name        | …SemanticModel/definition/tables/TableName/columns/Name.tmdl |
| Add or edit a relationship  | …SemanticModel/definition/relationships.tmdl                |
| Add translations            | …SemanticModel/definition/cultures/fr-FR.tmdl                |
| Add security role           | …SemanticModel/definition/roles/RoleName.tmdl               |

### Final Summary

A **.pbip** is a normal folder that contains human-readable files instead of one big .pbix,  
and the tiny **.pbir** files are just “signposts” that help Power BI Desktop open the report and dataset correctly.

```
