# EPM Datamart

## Package description

This package provide a datamart to share data between EPM applications and external producers and consumers. Import, export and currency exchange rates is functions provided here.

## EPM Datamart Versions

- [EPM Datamart 6.2.1](#epm-datamart-621) - Released 2026.06.23
- [EPM Datamart 6.2.0](#epm-datamart-620) - Released 2026.06.19
- [EPM Datamart 6.1.2](#epm-datamart-612) - Released 2026.03.30
- [EPM Datamart 6.1.1](#epm-datamart-611) - Released 2026.01.28
- [EPM Datamart 6.1.0](#epm-datamart-610) - Released 2026.01.
- [EPM Datamart 6.0.0](#epm-datamart-600) - Released 2025.11.11
- [EPM Datamart 5.5.0](#epm-datamart-550) - Released 2024.06.05
- [EPM Datamart 5.4.0](#epm-datamart-540) - Released 2024.02.21
- [EPM Datamart 5.3.6](#epm-datamart-536)
- [EPM Datamart 5.3.4](#epm-datamart-534)
- [EPM Datamart 5.3.3](#epm-datamart-533)
- [EPM Datamart 5.3.2](#epm-datamart-532)
- [EPM Datamart 5.3.0](#epm-datamart-530)

## Unreleased

**Changes:**

**Fixes:**

- Limit file upload size for Data Import/Export to 100 mb (#1804)
- Limit number of files possible to upload and disable open option (#1826)

## EPM Datamart 6.2.1

**Notes:**

- Invision 2026.2+

## EPM Datamart 6.2.0

**Notes:**

- Invision 2026.2+

## EPM Datamart 6.1.2

**Notes:**

- Invision 2026.1+

**Fixes:**

- Error message when browsing the currency echange rate workbook ( https://github.com/Profitbase-Public/Planner/issues/2190 )
- Data Export File Extension default (to csv if not specified by user) ( https://github.com/Profitbase-Public/Planner/issues/2196 )
- Actions menu hidden and hard to find in data import export workbook ( https://github.com/Profitbase-Public/Planner/issues/2214 )

## EPM Datamart 6.1.1

**Notes:**

- Invision 2025.7+

**Fixes:**

- Data Import page slowness fixed. ( https://github.com/Profitbase/Planner/issues/2158 https://github.com/Profitbase/Planner/issues/1753 )
- Potential collation Errors fixed ( https://github.com/Profitbase/Planner/issues/2166 ).
- Error when importing to Finance Actuals - Duplicate value for Data Last Updated ( https://github.com/Profitbase/Planner/issues/2143 )
- EPM Currency Exchange Rates - Daily Override Header ( https://github.com/Profitbase/Planner/issues/2163 )

## EPM Datamart 6.1.0

**Fixes**

- Data Import (no longer) needs file extension specified ( https://github.com/Profitbase/Planner/issues/2133 )
- ReportAccountByMonthHistorical no longer write datasets to cache. ( https://github.com/Profitbase/Planner/issues/2135 )

## EPM Datamart 6.0.0

**Changes:**

- Data Import and Export uses Flow instead of Dataflows and Azure Functions.
- New and improved Column Mapping popup for import.
- File splitting is now set with maximum rows instead of file size.
- Migration from Eaze to JavaScript started.
- Data Import & Export support .parquet file type. 

## EPM Datamart 5.5.0

**Changes:**

- New workbook "System Configuration": (#1580)
  - Configure which database objects to expose to the data import and export modules.
  - Option to give friendly names and descriptions to each object.
  - Configure Instance settings in UI for Datamart and Common (previously done by running scripts in designer)
  - Azure Function URLs visible and editable in UI.
  - New import option can be configured per import object: "Incremental File Reload", enables a secondary file prefix to specify periods to import based on file name patterns YYYY or YYYYMM. (#1690)
- Data Export: new option to create multiple files, by specifying a maximum file size. If exceeded, multiple files will be created. Each file will have a suffix [FileName]_[file number].csv (#1617)
- Data Import: new options for "custom" merge method. Enables abort copying data from temporary table to final destination if row count decreases or is 0 (#1655)
- Data Import: Warn user when changing fields that trigger autosave. A (modal) confirm box implemented when changing destination for import or connection. (#1689)
- Currency Exchange Rates Monthly Override decimals - Changed format to 5 decimals. (#1662)
- Azure Functions upgraded with newest packages and upgraded .NET framework to 8.0. Greatly improves performance and stability. (#1707)

## EPM Datamart 5.4.0

**Changes:**

- New Config setting to set Azure Hosted or on Premise (#1621)
- Data Import - Temp Table Name read-only. (#1600)
  - Reduces the chance of error by multiple rulesets using the same temp table (may arise from copying a ruleset, or user input using a common word such as "ledger").
  - Guarantees that it is unique per ruleset.
  - Makes maintenance and clean up easier (clean up improved in 5.4.0).
  - One field less for the user to worry about.
  - Still readable if user needs it for debug purposes.
- Data Import Destinations dropdown sorting updated to show most relevant tables first.
- Data Import form disabled while saving / loading to prevent input while saving (#1445)

**Enhancement:**

- A new target "FINANCE_ACTUAL" has been created to make it easier and more efficient to import finance actual data (#1624).

Data is imported as a slice of data for some or all legal entities and some or all fiscal periods.
This will make it faster to do incremental data loading by:

- Replace imported fiscal periods where imported fiscal periods determined by filter on source or CSV file(s) content.
- Replace imported or all legal entity fiscal periods loaded
- From Operations this load is now compleded in one step (where pbTransdataActual and ReportAccountByMonthActual is updated in same operation).

This require customers to ensure that finance actual data is not loaded to both pbTransdataHistory and FINANCE_ACTUAL (table pbTransdataActual).

Note that a new mandatory column, FiscalPeriod, must be mapped for this to work.

- Data Import (Azure) greatly increased performance.
- Data Import - Azure method - Add support for more complex SQL expressions. Was previously limited to simple "Linq" expressions. Now supports more advanced SQL. (#1412)
- Data Import - File Cleaning option (#1620)
  - File Cleaning is now an option you can set per ruleset. If on, the source files will be overwritten by a cleaning routine.
  - This removes some ASCII symbols, whitespace at end of file, etc..
  - Deprecates this issue: #1575
- Add custom File Import Destinations (#1608)
- Add Operation Launcher in Data Import / Export. It is now possible to configure Operations to be launchable from Data Import / Export.
- Data Import improved auto cast for import of empty values into numeric fields.
- Data Import Column Mapping (AutoCast) - Replace decimal separators (space or apostrophe) from csv with nothing for numeric fields. (#1572)

  (Reduce further errors converting nvarchar to numeric problems)
- Data Import Auto Cast for column mapping improved to better handle decimals (#1587)
  - Simpler formula that will handle more scenarios, whether using SQL import or CSV import.

    And will help seeing less of the annoying "Error converting data type varchar to numeric. "
- Data Import Column mapping - More space for SQL formulas (#1586)
- Data Export performance increased. (Automatically fetches WorkProcessVersionID when ExportName is specified.)
- Optimize and enable finance actual data import and enable for reporting (#1597)

**Bug fixes:**

- Data Import run as operation ignores "Auto Archive" setting (#1619)
- Excel Export in DataMart Export (restored) (#1609)
- Timeout increased for Import jobs run from operations (#1583)
- Incorrect last modified date shown for destination in Data Import. (#1584)
- Currency exchange rates - not generated after 2028 (#1565)
- File Import of empty values into numeric fields now properly handled.

## EPM Datamart 5.3.6

**Changes:**

Mostly affects instances hosted by Profitbase/Microsoft Azure (.biz domains):

- Data Import Archive functionality restored.
- Data Import Blank Rows handled (#1488)
- Importing a large number of CSV files from Blob storage (#1489)
- Data Export improved efficiency. (#1471)
- Hosting environment for import export functions moved to more efficient deployment slot.

## EPM Datamart 5.3.4

**Bug fixes:**

- Import from Blob fails due to Azure Storage version being incompatible and mismatch in encryption of connection string sent to Azure Functions (#1432)

## EPM Datamart 5.3.3

**Changes:**

- #1424
- Preview popups of temp data and destination data, increase displayed rows from 200 to 1000.
- Client side filtering enabled for preview popups.
- Added option of (server side) data filtering for those 1000 rows.
- Column Mapping - New test button to validate and test formulas and mapping to find potential errors.
- Auto Cast button - Changed to "splitbutton" for more options (CAST, TRY_CAST)
- Auto Cast no longer wraps transform formulas that already start with CAST or TRY_CAST.
- "Full import test" - List up to 1000 rows that fail to import. Helper function to find bad rows from data source.
- Azure Function import - Added filter to make sure it only tries to import .CSV files. (Implemented before 5.3.3 release)

**Bug fixes:**

- Auto Cast bug fixed; caused wrong type to be used in some cases. (#1421)
- 5.3.2 Datamart Column Mapping collation error.  (#1414)
- 5.3.2 Datamart Data Import Error; New "EPM Assemblies". (#1425)

## EPM Datamart 5.3.2

**Changes:**

- Added logging for import & export jobs. Available under the "..." menu. (#1351)
- File Import: Improved handling of nullable and numeric fields (#1353)
- File Import - New dropdown to explicitly specify the incoming date format. (#1307)

**Bug fixes:**

- File Import context menu not active. (#1352)
- Fix for collation error in column mapping for data import.  (#1376)

## EPM Datamart 5.3.1

**Bug fixes:**

- Data Import fails when hosted by Profitbase and using SQL to SQL import - **Critical** (#1354)
- Date Error popup on some client PCs when loading or saving import rulesets. (#1350)
- Data Import / Export errors when hosted in offline setup(#1358)

## EPM Datamart 5.3.0

**Changes:**

- Revised GUI with the aim of simplifying the process, reducing errors, and implementing wishes expressed by users.
- Connection list implemented instead of defining the connection properties for each import/export ruleset. Each connection is defined once, and then re-used. Helps to simplify GUI and reduce typing errors.
- Button to browse Azure storage and download files.
- Test Connection button added for Azure blob connections.
- Alternative (faster) methods available to instances hosted by Profitbase for import, export (#1272).
- Excel export for instances hosted by Profitbase (#789)
- Column mapping improved to better handle different scenarios (#1312)
- Operations created from a data import job only require 1 step, making it easier to use (#961)
- New options for data import "replace all": "Keep existing data if no rows loaded" and "Abort if rows decrease" (#1343)
- Implemented use of WebFunctions API (new feature, invision 2023.3) for much greater stability in "asynchronous" functions.
- New helper features in data import SQL server (external) browser popup: auto-collate and button to use the suggested query.
- Data export to Azure Blob (CsV): new dropdown to select date format output.

**Bug fixes:**

- Typing error in Currency Exchange Rates Comments field - changed to "Historical override" (#1293)
- Tables missing from source dropdown in data export, list will now include previously filtered out tables (#1219)
- Collation issues resolved (#1298).
- Data Export Column Mapping - Cannot select destination for certain source columns (when there are less destination columns than source columns) (#1312)
- CSV Import New Data button disabled for new import rulesets (known issues 5.2.5) (#1259)
