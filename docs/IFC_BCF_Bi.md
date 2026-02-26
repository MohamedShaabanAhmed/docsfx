---
uid: ifc-bcf-dashboard
title: IFC & BCF Issue Tracker Dashboard (Power BI)
description: A Power BI report template that combines IFC model viewing with BCF issue tracking. Load your own .bcfzip and IFC files to review topics, comments, and linked model elements in a single dashboard.
keywords: BCF Power BI, IFC BCF dashboard, BIM issue tracker, bcfzip, Power Query BCF, clash detection, BIM Collaboration Format, issue tracking
canonical_url: https://docs.flinker.app/docs/IFC_BCF.html
---

# IFC & BCF Issue Tracker Dashboard

<iframe title="IFC-BCF" style="width: 100%; aspect-ratio: 16 / 9;" src="https://app.powerbi.com/view?r=eyJrIjoiZGFhYzdiZjAtNGQ0Mi00NjQ3LWFjY2ItZWQ1ZTg2MjNhNjg3IiwidCI6IjQ0YjY0MGYzLTQ5YjAtNDMwNC05Yzk4LWM2MWQwYmMwZGMwMiJ9" frameborder="0" allowFullScreen="true"></iframe>

## Overview

The **IFC & BCF Issue Tracker Dashboard** lets you load a `.bcfzip` file alongside one or more IFC models into a single Power BI report. It displays all BCF topics in a filterable table, visualizes their distribution by type and status, and shows a comment thread for any selected topic. The IFC Viewer visual on the right displays the referenced 3D model.

The report parses `.bcfzip` files entirely within Power Query M-Code. No external tools, plugins, or Python scripts are required.

## What You Can Do

- Review all BCF topics with their type, status, priority, author, and description
- Filter by title, status, priority, type, creator, assigned user, or topic age
- See a stacked bar chart showing how many topics of each type are open, closed, or overdue
- Click any topic row to view its full comment thread in the HTML panel
- View the linked IFC model alongside the issues
- Track overdue topics using the three-state status logic (Open / Closed / Overdue)

## How to Load Your Own Files

### Parameters

The report uses four text parameters. Set them in Power BI under **Home** > **Transform Data** > **Home** > **Manage Parameters**.

| Parameter | What to enter |
| :--- | :--- |
| `BCFZIP_Path` | Full path to your `.bcfzip` file |
| `IFC_FilePath1` | Full path to the primary IFC file |
| `IFC_FilePath2` | Full path to a second IFC file, if applicable |
| `IFC_FilePath3` | Full path to a third IFC file, if applicable |

All parameters accept local file paths and SharePoint URLs:

```
C:\BIM\Coordination_Issues.bcfzip
```
```
https://company.sharepoint.com/sites/BIM/Documents/Issues.bcfzip
```

After entering the paths, click **Close & Apply** to refresh the data.

> **Note:** If your BCF file references only one IFC file, delete the unused `IFC_FilePath` parameters (right-click > Delete in Power Query Editor) to avoid refresh errors.

### Where the BCF File Comes From

BCF files are exported from BIM coordination tools such as BIMcollab, Solibri, Navisworks, or Tekla BIMsight. The standard export format is a `.bcfzip` archive. This report expects that file as-is, with no pre-processing.

## Dashboard Layout

### KPI Cards

Three cards at the top show:

| Card | Color | What it counts |
| :--- | :--- | :--- |
| **Open / Active Topics** | Amber | Topics with status Open or Active, not past due date |
| **Closed / Resolved Topics** | Green | Topics with status Closed or Resolved |
| **Overdue Topics** | Red | Topics with status Open or Active that are past their due date |

### Filters

The slicer panel provides filtering by Topic Title, Topic Priority, Topic Status, Topic Type, Topics Age Bucket, Creation Author, and Assigned To. All filters work together and update every visual on the page.

### Type Distribution Chart

A stacked horizontal bar chart shows the count of topics per `Topic_Type` (e.g., Issue, Clash, Inquiry, Request). Each bar is segmented by status color (amber for open, green for closed, red for overdue). The chart adapts automatically to whatever types exist in your BCF file.

### Topic Comments Panel

The HTML panel on the lower right displays:
- When no topic is selected: *"Please select a Topic to show its comments"*
- When a topic is selected: the topic title, status badge, type, author, creation date, and the full comment thread with timestamps

### IFC Viewer

The 3D model viewer on the upper right shows the IFC file linked to the BCF. If the BCF references multiple IFC files, each one is loaded through its own parameter.

## Data Tables

The report produces three tables from the BCF file. These are available in the Power BI data model for building additional visuals or measures.

### BCF_Topics

One row per topic. Key columns: `Topic_GUID`, `Topic_Title`, `Topic_Type`, `Topic_Status`, `Topic_Priority`, `Creation_Author`, `Creation_Date`, `Due_Date`, `Comment_Count`.

### BCF_Components

One row per IFC element referenced in a topic's viewpoint. Key columns: `Topic_GUID`, `Component_IFC_GUID`. The `Component_IFC_GUID` column can be linked to IFC element GUIDs for model-level issue tracking.

### BCF_Comments

One row per comment. Key columns: `Topic_GUID`, `Comment_Author`, `Comment_Date`, `Comment_Text`, `Verbal_Status`.

## Compatibility and Troublshooting

### Compatible Versions

This report has been built with **BCF 2.0** files. BCF 2.1 files will be also working given the backward-compatible structure.

### Troubleshooting

| Issue | Solution |
| :--- | :--- |
| No topics appear after refresh | Verify that `BCFZIP_Path` points to a valid `.bcfzip` file |
| Comment panel shows nothing when clicking a row | Install the "HTML Content" visual from the Power BI marketplace |
| Closed / Overdue KPI shows (Blank) | This is expected if no topics match that status in your file |
| Components table is empty | Not all topics include viewpoint files; this is normal |
| IFC model does not load | Check `IFC_FilePath` values and ensure the file is accessible |
| Refresh error on unused parameter | Delete any `IFC_FilePath` parameters that are not in use.
