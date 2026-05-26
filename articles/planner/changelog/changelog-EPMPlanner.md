# EPM Planner

## Package description

This package provide the financial planning function and can be organized in processes (e.g. Forecast) and one or more versions.

## EPM Planner Versions

- [EPM Planner 6.2.4](#epm-planner-624) - Released 2026.05.26
- [EPM Planner 6.2.3](#epm-planner-623) - Released 2026.04.22
- [EPM Planner 6.2.2](#epm-planner-622) - Released 2026.04.08
- [EPM Planner 6.2.1](#epm-planner-621) - Released 2026.03.16
- [EPM Planner 6.2.0](#epm-planner-620) - Released 2026.01.22
- [EPM Planner 6.1.1](#epm-planner-611) - Released 2025.10.29
- [EPM Planner 6.1.0](#epm-planner-610) - Released 2025.06.05
- [EPM Planner 6.0.3](#epm-planner-603) - Released 2025.02.17
- [EPM Planner 6.0.2](#epm-planner-602) - Released 2024.11.05
- [EPM Planner 6.0.1](#epm-planner-601) - Released 2024.09.27
- [EPM Planner 6.0.0](#epm-planner-600) - Released 2024.08.29
- [EPM Planner 5.5.1](#epm-planner-551) - Released 2024.04.24
- [EPM Planner 5.5.0](#epm-planner-550) - Released 2024.02.21
- [EPM Planner 5.4.1](#epm-planner-541)
- [EPM Planner 5.4.0](#epm-planner-540)

## Unreleased

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/92)

## EPM Planner 6.2.4

**Fixes:**

- Driver based
   - not able to test on attribute values in sql (button-click) formulae (#2270)
   - Incorrect historical reference data (#2298)
   - Account details - drill to details. Incorrect in certain situations (social cost data) (#2279)
- Deviation2 calculations does not work in plan overview, input modules and account detail reports (#2279)
- Version deployment fails in certain situations (#2277)
- Operations - certain planner operations may have steps that will be flagged as "Object not found" (#2288)

## EPM Planner 6.2.3

**Fixes:**

- Account/plan overview:
   - Plan overview account details - display social cost transactions over two rows (#2236)
   - Account module display error message when at non-leaf level in setup that does not allow multi department input (#2242)
   - Input on local account - adjustments EPM Planner (#2254)
   - Account module - input on local accounts - drop down is not filtered on report line (#2265)
- Personnel:
   - Personnel - dimension column ordering not correct for FTE and Headcount tabs (#2245)
- Driver based:
   - Driver based models columnsetup, YTD visible = true hides ytd columns (#2238)
   - Historic1 column in driver based models does is incorrect (#2258)
- Fixed assets:
   - Fixed assets input fails on save - gives error message (#2239)
- Version deployment:
   - Version deployment fails is sql aggregate calculations used for driver based models (#2237)
   - New version fails on duplicate values in FctAccInputDistributerRulesOverrideTempStore when input on CurrencyForeignor AccountSRc (local account) is used (#2240)
- Data quality:
   - Transaction processing - remove leading/trailing spaces in DepartmentID when comparing transaction slices (#2248)

## EPM Planner 6.2.2

**Fixes:**

- Account/plan overview:
   - plan differences after roll forward (#2212, #2215)
   - historic columns display 0 when sign factor missing in report setup (#2233)
- Personnel:
   - rollover does not properly roll forward HC (headcount) (#2216)
   - actuals missing in account details report (#2232)
   - pay raise settings not rolled forward when plan rolls to new year (#2213)
- Driver based:
   - transaction pipeline fails if plan horizon < 3 years (#2230)
   - actuals missing in account details report (#2232)
   - not able to add a style to a measure in a model (#2229)
- Version deletion fails (#2218)
- System operations:
   - missing steps related to 'Regenerate historical data for currency conversion' in operations 'Account(s) added..' and 'Actuals incomplete..' (#2235)
   - incorrect steps in operation 'Reload LegalEntity dimension' (#2225)
- Deployment: new deployments fail (#2228)

## EPM Planner 6.2.1

**Breaking Changes:**

- If you use custom assumption view for driver based assumptions, the following columns must be added to the view to make it adhere to the new format given an increased long term planning horizon of 2 + 10 years (#2167). All columns decimal(18,4):
  
   - FctNYPlus6
   - FctNYPlus7
   - FctNYPlus8
   - FctNYPlus9
   - FctNYPlus10

**Changes:**

- Long term planning horizon increased by 5 years to 2 + 10 years (#2167)
- Driver based: calculations are validated on save in the calculation definition page in Driver based modelling (#2199)
- Account module: Insert copy of row will keep the periods changed state of the source row (#2188)

**Fixes:**

- Personnell long term planning issues: missing cumulative salary increase (#2181), long term years missing (#2177) and scaling of long term data not updated after save or rollover (#2211)
- Fiscal year offset: actuals ytd part of historical reference data did not apply the fiscal offset correctly in account details report (#2187)
- Plan overview:
   - use of historic reference column Historic3 did not return any data (#2171)
   - drill to details from account details displayed all the account's transactions even if account was split over multiple report lines (#2204)
- Account module: opening balance was incorrectly included in first period of the year, this also applies to account details reports (#2202)
- Personnel module: unable to input vacation pay override values (#2173)
- Driver based module:
   - incorrect historical reference data (#2193)
   - error message when deleting input rows (#2183)
   - account details report missed social cost transactions if target account was itself a social cost account (#2182)
- Account, Personnel and Driver based modules:
   - auto transactions targeting departments in other legal entities got the soure row's legal entity (#2208)
   - pipeline processing of input data did not set a default sign factor if account did not have a sign factor set. Default now set to 1 (#2186)
- Version deployment: version deployment failed due when reapplying column store indexes (SQL server version dependant) (#2207)
- Certain operations might fail when deploying report setup if historical trend series used that referenced report linescontaining formulae where the denominator was 0 (divide by zero) (#2191)
- Saving module extension setting (Input settings and administration) gave an error message (#2172)

## EPM Planner 6.2.0

**Breaking Changes:**

- Historical reference columns that use another version as source (as opposed to Actuals), will now return not only plandata but also historical data prior to the cut-off date (#1827). This scenario has often been resolved by using deviation column to add a historical reference column containing actuals and a historical column containing the plan data. This is a change of behaviour and a potential breaking change.

**Changes:**

- Planning can be done on local (company) accounts as an option (#2118). The change involves the account module, supporting details reports as well as finance reports. A separate dimension (AccountSrc) is used for local accounts using the existing group account mapping setup (Dimensions workbook) as source.
- Right click menu "Distribute Value" is enabled in the input grid for the account, personnel, driver based  and capex modules (#1683). "Distribute Value" allows the user to select a range of input cells and then distribute a total across the selected cells by invoking this function which is found on mouse right-click in the input grid. Various distrution methods can be used such as relative add, uniform add, etc.
- Deviation columns can now be formatted with a style for Positive and/or Negative values (#1649). Styles can be set either to red text or green text. This change affects Plan overview, the Account module and the Driver based module. Different styles can be associated with say a positive value, an example being the Plan overview where a deviation column for one reportline can get a green text if positive whereas another can get a red text if positive.
- Dimensional columns in input modules Account, Personnel and Driver based can be re-ordered (left to right) if the default ordering is not appropriate. This ordering will also affect the sort order with which input data is rendered when loading data. (#2145)
- Driver based module and Driver based modelling:
	- Visible attribute introduced for measures (#2090)
	- Attribute values can only be altered in the input grid on the driver page while a specific driver measure is selected or the model is configured to have only one driver measure (#2067)
	- Attributes can be used when defining distribution rules and account mapping (#2066)
 	- Calculated measures can be made visible on the driver page so long as they are on-button click (sql) calculations. A Recalculate button will be visible on the driver page.
    - YTD historical period columns can be hidden when not relevant
    - A measure's "Total across periods" setting can be set to none (blank) if no year totals should be calculated nor any year totals be distributed to periods.
    - The Apply button in driver based modelling is removed as this is done automatically when saving changes to the model. In the Process.. dialogue, an option is available to update historical data.
    - An "Auto refresh after save" option is introduced in driver based modelling.
    - An auto refresh is now automatically done after a click on the Recalculate button (driver- and/or details-page)
    - Calculated measures (on-button click / sql) can now reference time through:
    	- Testing on YYYY (year), Q (quarter no.), MM (month no.), for example Case When MM > 6 Then... This also implies that there are certain reserved measure names: YYYY, Q, Qseq, MM. The token ##TY## can be used for the first plan year and will automatically be resolved. For example Case when YYYY = ##TY## Then... is resolved to Case When YYYY = 2026 Then... in a planner version with start date in 2026.
    	- Making use of directives that accumulates part of the calculation over multiple periods, for example using @SumPrevQuarter (sum of previous quarter), @SumYear (sum of current year), etc.. A calculation may be of the form [Measure1] * @SumPrevQuarterStart([Measure2] + [Measure3])@SumPrevQuarterEnd where the part of the calculation enclosed between @SumPrevQuarterStart and @SumPrevQuarterEnd is calculated over the previous quarter relative to the context of [Measure1].
- Account module: additional dimensionality available (#2056). Dimensions Product, Market, Supplier and Employee can be used in the module when appropriate.
- Version deployment of Planner:
	- Planner specific version dialogue reworked with parameter interdependency and validation (e.g. Action "Roll forward from source version" and the number of months) making version definition more intuitive and less prone to inconsistent definitions. (#2007, #2080)
	- When deploying a planner version using the "Start fresh at new start date.." action, any existing loan input data will not be removed, i.e. left as is in the source version used (#2121). This is a change of behaviour.
- Plan proposal: new import options (#1961, #2063)
	- Driver based: Option to insert new rows (true | false, default true) and to update historical data (true | false, default true)
 	- Account: Option to insert new rows (true | false, default true).
  
**Fixes:**
- Input modules:
	- insert new row context menu inactive outside of grid area (#2085)
 	- transactions not deleted when deleting ALL input rows for a department (#2137)
- Driver based: Social cost rules (employer tax, etc.) may not apply correctly for periods after the first plan periode when auto transactions targeting departments with differing rates are used (#2093)
- Account module:
	- supporting details page not sensitive to selected report line (#2091)
 	- deviation in periodic values displayed when useing multi-department input while using distribution keys generated from historical data (#2113)
  	- duplicate transactions when setting an explicit account context (Account filter set to explicit account) while triggering auto transactions targeting account(s) outside the set account context (#2147)
- Personnel module: long term FTE data not deleted even though all rows (year 1 and 2) are deleted (#2110)
- Plan overview: changes in account mapping is not updated (#2115)

## EPM Planner 6.1.1

**Changes:**

- Driver based module:
	- Social cost from advanced motor interface requires that involved accounts must be part of the social cost setup for payroll accounts in Finance settings. This is an intended change of behavior (#2035)
	- This year's total in driver based models for calculated and summable measures - calculation method is changed. This is an intended change of behavior (#2012)
	- Option to control sort order for dimensions (#2071)
	- Allow for "easy" plan proposal from Personnel fact (#2021)
	- Improved useability when inserting new rows to setting tables - unused dimensions hidden, automatically inserted All level selection for all non-department dimensions (#2064)
- Personnel module: option to hide columnSalary rest of year (#2031)
- CapEx module: new right-click menu to change dimensionality (#1912)
- Account module: option to avoid loading actuals last 12 months (#1954)
- Plan overview: option to include Year end due data (#1959)

**Fixes:**
- Pension in vacation month not calculated in Plan overview and Preview datasets (#2028)
- Post pipeline transactions from custom sources (pbTransdataPLSourceCM) not visible in AccountDetails report nor Plan overview (#2052)
- Short term finance transactions is not present in Plan overview report (#1910)
- Long term personnel planning: data not included in financial simulation and proposed salary raise fail to take in to account the year-on-year cumulative effect. (#1973)
- Driver based pipeline failed to produce all expected transactions (#2038)
- Additional dim mapping (split account between multiple report lines based on dimensionality) did not give correct result (#2036)
- Allocation keys based on report lines did not produce correct result (#2027)
- Pipelines were sensitive to changes of case in departmentID and processed data in unexpected batches not generating complete set of transactions (#2061) 
- Sum columnin account details (personnel) is incorrect (#2060)
- Process in driverbased modelling - spinner stops immediately - should await the flow pipeline to finish (#2033)
- Driver based flow - calculation script will terminate flow execution when execution time exceeeds 5 minutes (#2015)
- Column order Account details report not intuitive (#2018)
- Personell module process transactions from overtime column even if not mapped to account (#2006)
- Version deployment and Update and process data operation fails when historic column configuration is incomplete or incorrect (#2024)

## EPM Planner 6.1.0

**Breaking Changes:**

Planner 6.1.0 use flow based transaction pipelines (introduced as optional during Planner 6.0.x, mandatory from 6.1.0). The following breaking changes apply wrt to dataflow based transaction pipelines:

- The use of condition expressions in auto transaction configurations is no longer supported. Separate columns for period based condition exists to support previous expressions such as month() == 12. Manual
  configuration is required to migrate expressions to new condition columns.
- The use of custom pre- / post- transaction pipeline scripts require manual configuration in flows. Consult the "Planner customization patterns" document for details.
- Driver based models now include EmployeeID (new dimension) and attributes Attr1 and Attr2. If the solution makes use of custom driver based assumption views, those views must be adjusted to include columns EmployeeID, Attr1 and Attr2 all of which are Nvarchar(50) type columns. 
  Default value is * for all of these columns.

**Changes:**

- Personnel module: there is a change of behaviour related to base setting PersonnelUpdInputFromSrcFact. If set to FALSE, no input data will be changed. This applies to updating existing rows and inserting new rows (#1958)
- Driver based module:
	- Introduction of new dimension Employee (#909)
	- Introduction of attributes (#1663)
	- Ability to link models in order to process automatically. (#1861)
	- Ability to group model import by selected dimensions (#1779)
- Allocation: improved user interface introducing ability to select givers / bearers as well as accounts from dimensional hierarchies making selection easier and more maintanable as leaf nodes to aggregate selections are automatically compiled when involved dimensions change. 
  (#1620)
- Plan proposal (Account and Driver based) - new import options (https:#1847 and #1944)
- Plan overview: "excel-like" sum, average and count display sensitive to user selection. (#1979)
- Introduce KPI account type to cater for non-amount based data such as headcount, etc. (#1832)
- Account, Personnel, Driver based, CapEx and Loan modules: introduce ability to set default filter values for enabled input module filters (#1962)
- Account module: periodic comparison report displays amounts as default as opposed percentages (#1977)

**Fixes:**

- CapEx: column asset group is missing a translation (#1821)
- Long term planning in personal module, issue with deleting of rows (#1836)
- Planner engine source view - need to take into account any mapped measures from driver based models (#1830)
- Allocation gives wrong companycode on bearer if giver is a different company than bearer (#1868)
- LegalEntityID in input stores must be updated post publish of department dimension (#1871)
- Account worksheet - historic ref columns do not take into account sign factor when aggregating over multiple legal entitites with different functional currencies (#1840)
- Change dimensionality in Personal module do not copy salary increase amount (#1833)
- Capex Module, not showing VAT part of investment (#1831)
- Personnel: Autotransaction for additional columns with target department <> source department incorrect (flow) (#1924)
- Personnel Change dimensionality - data not updated (#1875)
- Triple numbers in historical column in account details report (#1965)
- Account details report missing actuals despite having actuals in Plan overview (#1927)
- Driverbased: Setting "navigate directly to details view" creates: Inner Exception: '@dbg' does not exist. (#1940)
- Account Plan Proposal Disappearing after exclusion of AccountID (#1942)
- Plan proposal, department override includes legalentity id from source department (#1857)
- Allocation - Execution timeout received (#1853)
- Assumption filter in driverbased module reveals other non relevant measures (#1860)
- Preview dataset (For Finance Reports) - employer tax does not include employer tax on pension when flow pipelines are activated (#1856)
- Plan Overview - cannot find counterpart transactions - using Flow pipeline (version Planner 6.0.3) (#1972)
- Long term planning in personnel module (#1973)
- Problem with long term planning in Account module (#1980)
- Flow pipelines - I/C transactions where LegalEntity/CPLegalEntity have differing functional currency does not work (#1985)
- FCTYearTotalFloating = FALSE is not supported when using Flow pipelines (#1839)
- Error message when inserting a comment in the Planoverview workbook (#1882)
- Allocation - incorrect keys for measure FTE when used in multiple models (#1885)
- Loan validation inconsistance for FirstPrincipalDate when date is selected to be first day of next periode (allowed). (#1923)
- Version roll forward, headcounts set incorrectly to 1 when null in personnel module (#1991)

## EPM Planner 6.0.3

**Changes:**

**Fixes:**

- Limit file types allowed (.doc,.docx,.gif,.jpeg,.jpg,.pdf,.png,.ppt,.pptx,.txt,.xls,.xlsx) to upload and upload size to 100 mb (#1806)
- Limit number of files possible to upload and disable open option (#1826)

## EPM Planner 6.0.2

**Changes:**

- OBdue amount due does not work (#1823)

**Fixes:**

- Translation fallback in calculation descriptions for driver based models shows id in stead of description field (#1812)
- Driver based modelling - Process - recalculation and assumption lookup not done (#1818)
- Plan Overview - Custom context menu is missing a valid Department context (#1817)
- Plan overview - currency converted historical reference data at non-leaf level fail to display when blank and not null is used for default values for optional dimensions (#1819)
- Driver based flow may fail when additional dim mapping is used (#1816)
- Account Plan proposal using opening balanse on P/L account (#1814)
- Personnel - changes made to Base monthly salary in Personnel details is reverted after a new change is done to FTE page (for same key ) (#1813)
- Flow pipelines - transactional data remain when deleting all input rows for department while at a non-leaf level (#1811)
- Driver based drill to account when at non-leaf - no data returned (#1803)
- Loan module Added new status translation doesnt work in dropdown (#1798)
- CapexModule - Copying data from other years does not include number of credit days (#1799)
- Limit file types allowed to upload and upload size to 100 mb (#1806)
- Not getting any transactions when using validate finance simulation (#1800)
- Pipeline flows terminate after 5 minutes (#1797)
- Plan overview slow (~20 sec) when selecting nodes (not leaf nodes) (#1802)
- Pipeline flows may fail if input store is empty (#1796)
- OBDueVacationPay does not work (#1824)
- SYS_DatasetID does not allow null values in OBDue (#1822)
- Plan overview | Account details - drill to detail may no return data when using optional filters (#1828)

## EPM Planner 6.0.1

**Changes:**

- Plan overview - ability to click a sum line and still go to an input module (#1784)
- Personell module - adding a new employee - FTEs and headcount is not updated when navigating to FTE and headcount tab (#1772)
- Driver based - make calculated measures aware of Historic override formula (#1755)

**Fixes:**

- Driver based modelling - save distribution keys may render a key violation error (#1786)
- Flow transaction tables not partitioned after a version deployment (#1782)
- Driver based flow - incorrect delete batch size setting (#1776)
- Driver based workbook - calculations not performed when saving from driver page in a multi department context when flow is activated (#1780)
- Flow pipelines do not generate employer tax on generated pension cost (#1767)
- Script fails as query optimizer runs out of resources due to too complex queries (#1745)
- Language code nn causes error when loading account module (#1758)
- Headcount for year 2 not visible in long term in Personnel module (#1766)
- Delete Driver based module does not remove transaction data from flow tables (#1771)
- Operation "Empty base data" does not empty flow transaction tables (#1770)
- Depreciation on Fixed Assets does not show up in Plan overview (#1769)
- Flow: Error message when deploying new version (#1762)
- Flow pipelines do not remove transaction data when all input rows for given department are removed (#1765)
- Driver based - historic numbers generated for hidden dimensions (#1750)
- Driver based - not able to drag cell values (#1787)
- Personnel - incorrect account mapping for salary when more than one salary account is used (#1789)

## EPM Planner 6.0.0

### Breaking Changes

Planner 6.0.0 supports flow based transaction pipelines (optional). If used, the following breaking changes apply:

- The use of condition expressions in auto transaction configurations is no longer supported. Separate columns for period based condition exists to support previous expressions such as month() == 12. Manual
configuration is required to migrate expressions to new condition columns.
- The use of custom pre- / post- transaction pipeline scripts require manual configuration in flows. Consult the "Planner customization patterns" document for details.

**Changes:**

- Performance improvement for the transaction processing (save in input modules). (#1680)
- Driver based: the account details report is no longer placed in the main screen of the module but is accessible as a pop-up using the $ sign in relevant rows - $ in the sum row will display the account details report for all rows in context.(#1701)
-The Back button is removed from all input modules (#1640)
- Support for company specific planning processes (#1669)
- Driver based - offset calculations requires multipls recalculations if one or more referenced measures are themselves calculated measures (#1739)
- Allow for more space in data-dense pages (#1557)
- Increaese filter widths to 352 pixles on right filter and 256 on left filter for Plan Overview and Finance Reports (#1690)
- Allocation - increase precission and scale for keys (factors) (#1723)
- Update input modules with legal entity from dimension (#1693)
- Navigation: Opening modules from different versions, loads exsisting page in browser (#1685)
- Historical reference columns - introduce relative date expressions to improve rolling forecast automation (#1684)
- L12M column in account details report (#1681)
- Personnel: FTE and HC columns are not configurable with respect to using override columns for employer tax, etc (#1642)
- Performance improvement - worksheet load (#1679)

**Fixes:**

- Plan overview -> account details. Incorrect format configuration used (#1738)
- ReportLineSetup wrong datatype in IncludeAccountsExp (#1736)
- Account / Personnel - not able to drag cell values (#1735)
- Discrepancies in Historical Reference Columns when using previous versions as historical reference (#1672)
- Plan overview - historic columns not currency converted on aggregate levels if dimensional columns are null (historic data source) (#1629)
- Driver based - change dimensionality gives error message if a specific row context is chosen using the $ row-icon (#1628)

## EPM Planner 5.5.1

**Changes:**

- None

**Fixes:**

- Improved performance Plan Overview and account details including drilldown (#1674)
- Driver based - summable measures incorrectly redistributed after a central key change when plan start in months 2,3 or 4 (#1672)
- Driverbased model: Double values in historic ref columns and plan proposal when using another version as source (#1660)
- Account details report may not display actuals (plan start is different from start of year) (#1656)
- Inaccuracies in Planner deviation column, dataset included OB transaction (#1648)
- Fixed issue with Duplicate Rows and expanded Table Size in Driver-Based Model (#1629)

## EPM Planner 5.5.0

**Changes:**

- **Changed default navigational pattern between Plan overview and input modules.** By default (click a report line or launch input module from module launcher drop down), navigation to an input module will now launch the module in a new browser tab in the same browser window as opposed to launching it in the same tab as has been the case up until now.

  The Plan overview will receive a message from the input module to refresh once data is saved and submitted and will automatically refresh. A spinner with text "Submitting data, you can resume editing" will display in the input module for the duration of the submit process.

  The Back button will be disabled as you may switch between tabs and will be removed in a future version. Re-launching an input module from the Plan overview will relaunch it in it's existing tab. It is still possible to right-click a report line and launch the associated input module in a new window. (#1485)

![pic](https://profitbasedocs.blob.core.windows.net/plannerimages/Planner550ChangeLogImage1.JPG)

- **Driver based module - ability to redistribute a total using percentages (%)** Up until now, you could redistribute data by altering the amounts in individual periods. New for this version is the ability to redistribute measures that apply for distribution using percentages. This is done in the Distribution pop-up (#1430):

![pic](https://profitbasedocs.blob.core.windows.net/plannerimages/Planner550ChangeLogImage2.JPG)

- Dimension names displayed in module specific configuration tables in individual versions even when Central dim options is  used. Central dim options allows for a central naming of dimensions. These dimensions are used for module-specific configuration and the names set centrally is now displayed in addition to the dimension key to avoid any confusion.

  In addition, a message is displayed in the version specific translation table to inform that any translations for dimension names must be set in the central Report Setup workbook if central dim options is used. (#1434)

![pic](https://profitbasedocs.blob.core.windows.net/plannerimages/Planner550ChangeLogImage3.JPG)

- Layout of the Allocation page in Finance Settings is improved by introducing a tab control with two sub screens, one for the Setup of the allocation rules and one for the QA part listing distribution keys and allocation transactions. In addition, input validation is introduced and enforced, meaning that duplicate rows can not be saved. This applies to giver-bearer and source accounts. The change is non-functional, a pure visual change to improve usability. (#1476, #1542)

![pic](https://profitbasedocs.blob.core.windows.net/plannerimages/Planner550ChangeLogImage4.JPG)

**ShortTermCash calculation** does not include opening balance when plan start periode is 1 (typical for yearly budget) (#1513).

Only actual OB cash is included. For a yearly budget, actual opening balance will normally not be available. When plan start periode is after first fiscal period, the opening balance is normally based on actual YTD and hence will be calculated correctly. However if you adjust the OB, the adjustments will not be included in calculation.

Changed: Functional: Yes; User interface: No; Data model/schema: No

Package(s) changed: EPM Planner v5.5 only

**Tax calculation** is wrong in certain cases (#1500)

Tax calculation is wrong in the following cases because the 'net before tax' will be different in the report and in the source for tax calculation in system. These are the cases:

- All Profit/Loss accounts are not mapped to an account in Account dimension. These will appear with internal text instead of account id in Trial Balance report. This is user error.
- Internal trading with counterpart will normally set up sales with expense as counter-account. This works correctly. If the counterpart is set as "not expense" and with a balance account, the system post the counter-amount as "purchase" and tag it as profit/loss. This is as implemented but not clear and limiting use and will be changed.
- Tax calculate for plan start period will include the year-to-date in net before tax. This is wrong (Bug)

The functional changes here are:

- Only accounts from Account dimension with type Profit/Loss will be included as basis ('net before tax') for calculating tax.
- Only plan period postings is included in the basis for tax calculation.
- Simulation engine will tag counter transaction as balance when counter account setting is 'Is Expense' = false.

Changed: Functional: Yes; User interface: No; Data model/schema: No

Package(s) changed: EPM Planner v5.5 only

**Internal Sales** - changes and additions to internal sales accounts and counter-accounts may not be applied to Plan Overview (#1509)

This is because to internal logic to detect changes to tables that should cause the mapping table to be updated.

Workaround: Use the Finance Reports simulated data where internal sales accounts will be correctly applied.

The change here is to always update the internal mapping table whenever Sales settings is saved.

Changed: Functional: Yes; User interface: No; Data model/schema: No

Package(s) changed: EPM Planner v5.5 only

**Internal Sales** - error message when trying to define the finance setting for Sales Intercompany Accounts (#1466)

The error message:

![pic](https://profitbasedocs.blob.core.windows.net/plannerimages/Planner550ChangeLogImage5.JPG)

The reason for this is that elimination Legal Entity counterpart mapping is not defined or updated with the counterpart and elimination entity involved. (See Dimensions workbook and page for Elim.Legal Entity Mapping).

The change here is to give a more instructive error message explaining the reason for the error.

Changed: Functional: Yes; User interface: No; Data model/schema: No

Package(s) changed: EPM Planner v5.5 only

**Wrong (duplicated) values in Plan Overview** when using additional dimension mapping to map same account(s) to different report lines (#1388).

This bug will apply to all "Input" reports that got one or more accounts split over more than 1 report line.

Transactions are mapped to report lines based on account only and hence if the reportline is split by  other dimensions, duplicate data will be introduced.

The change here is to ensure that original transactions keep the original reportline mapping. This will also apply to elimination transactions posted. For counterpart transactions posted on counter-account on counterpart legal entity and on elimination legal entity, it is assumed that this counter-account (normally a cost account) is not split across multiple accounts. In other words is is assumed that mapping counterpart account only on account is sufficient. This is a simplification to ensure that this change does impact performance.

Limitation: As state above is counter-accounts is mapped across multiple report lines using additional dimensions, this will not work.

Changed: Functional: Yes; User interface: No; Data model/schema: No

Package(s) changed: EPM Planner v5.5 only

**Enable additional content to Plan overview:**

It will now be possible to include company tax and interest from cash an overdraft to "Plan overview" without having to change the standard configuration (#1462).

![pic](https://profitbasedocs.blob.core.windows.net/plannerimages/Planner550ChangeLogImage6.JPG)

The above setting will ensure that e.g. tax data from last simulation is available to be included in the input report. Note that tax data is calculated for each legal entity and hence will not be available nor correct on department level.

It is recommended to stop Plan overview input report setup at EBIT level and leave the complete P&L report to "Finance Reports" (after simulation). The reason for this is that:

- Company Tax is calculated based on Net results including interest from cash and overdraft. This is not possible to calculate accurately without a cash balance per day/month from all transactions involving cash. This is what is done during simulation.
- Data will be from last simulation and hence will not be correct after P&L input is changed in Plan overview.

Changed: Functional: Yes; User interface: Yes; Data model/schema: Yes

Package(s) changed: EPM Planner v5.5 only

**Year end transfer is not posted on accounts:**

When using "Year end Alloc." you may see that year end transfers is not posed on accounts according to system fallback account mapping. This is because of an error in simulation dataflow (setting Debug flag to "True"; should be set to "False") (#1539).

Changed: Functional: Yes; User interface: No; Data model/schema: No

- Package(s) changed: EPM Planner v5.5 only

- Input validation introduced in Input Settings and Administration as well as Finance Settings. Mandatory fields are highlighted when null when new rows are added. (#1560)

- Logging is improved for dataflows such as version deployment and transaction pipelines (processing of Account, Personnel and Driver Based input). The version deployment log is visible in the Version Manager when deploying a new version and the the transaction pipeline logs are visible either in Operation manager (when run from an operation) or in the Hypergene InVision designer. (#1544)

- Historical reference data generated if configured regardless of the columns being visible. (#1503)

- Payroll engine: transactions with TransTypeID = 1 are now included (#1532)

- Departments can be set inactive for plan input. A new attribute AllowInput is added to the Department dimension (default true). When false, data for the department is not editable and actions such as Insert new row, delete row, etc do not take effect. (#1595)

**Bug fixes and GitLab reference list:**

- Plan proposal - Set adjustment factor. Month columns display incorrectly for NY+1 (#1569)
- Year End Due engine fails due to field value too large (#1568)
- Year and closing - missing metadata (#1564)
- Plan overview - Historic reference data at group nodes in multi currency solutions incorrect if group by dimension columns are null and not (#1538)
- Account Plan proposal historic grouping done based on visibility and not the group source data attribute (#1533)
- Account plan proposal translation missing (#1520)
- Additional report line - dimension mapping was not implemented for Account Plan proposal (#1515)
- Measure - map ledger account to measure - name of measure does not display correct translation (#1514)
- Account drill to details report - double depreciation rows when same asset planned in the CapEx module had different comments in different years (#1511)
- Personnel - vacation month salary reduction and pay raise not recalculated when altering FTE (#1505)
- Wrong historical numbers in Plan overview because actual historical data from Planner solution and actual data from Consolidation solution is added and resulting in doubled historical numbers (#1502)
- Driver based - mandatory filter ignored when making a selection in the measure and/or period drop downs (#1501)
- Account module and multi department input: altering period filter when row limit is exceeded leaves rows in an IsLocked state (#1499)
- PostUpgrade fails when upgrading solution as script Explode SalesAccountsIC does not handle collation conflicts (#1495)
- Input modules - row context attempted set when clicking the $ icon on the sum row yielding an invalid context (#1490)
- Deviation columns do not use sign factor in account module and account details report. (#1484)
- Account Plan Proposal - incorrect guidance text. (#1482)
- Loan module - unable to add a 0 principal override. (#1480, documentation clarification)
- Import from source fact to Personnel module incorrect for historic FTE and historic monthly salary (#1477)
- Import of ledger data using amount foreign. Clarification in data requirements documentation and introduction of the ability to choose whether to group source data or not for the currency foreign dimension (#1468)
- Various issues related to translations not working properly (#1436, #1450, #1459, #1467, #1483, #1487)
- Finance reports extension deployed locally in a Planner version fails (#1428)
- Custom button not working (Account and Personnel modules) (#1427)
- Custom button names restored on upgrade (#1426)
- Changing the name of a driver based model did not have effect in all filters and workbooks (#1612)
- Fixed assets, Investment and Loan financial engines use the current (and no longer long) time selection option. This reduces the amount of data output (#1114)
- Driver based: distribution of next year's total did not include the December next year period column. Applies to summable measures in a plan with start January 1st making December next year period column#24 (#1616)

## EPM Planner 5.4.1

**Breaking Changes:**

These are changes that is likely to cause existing configurations to present different content after upgrading a solution.

- **Generating OB** The way Opening Balance (OB) is generated is changed and may cause e.g. Trial Balance to not sum to 0. This may also require to revisit configuation and customization involving OB. (#1011)

  Posting on internal AccountKeyID 'EquityEarningsYTD' is now removed when generating OB.

  The following now applies for OB:

  Generating OB for beginning of year (fiscal period 1): OB is generated from previous years actuals. It is assumed that last years results is posted to balance (so that each sum of balance and sum of Profit & Loss is 0). If this is not the case, trial balance will not sum to 0 for OB. Users then have two options: 1) Ignore that trial balance for OB is not 0 or 2) Adjust OB by posting the difference (e.g. on balance account for other equity).

  When generating OB for forecast periods after beginning of year (fiscal period > 1): OB is generated including YTD for Profit & Loss accounts. Note that an opening balance is required for the planner posting engines to deal with due payments from e.g. receivables, payables, etc. configured in OB Due under Finance Settings.

  When generating OB for subsequent forecast years: Solution will close previous year and transfer results or loss to balance (default for new solutions). This is the default for new deployed solutions. For upgrade of existing solution and to maintain compatibility, the mechanism is not in effect by default. Users can use Year End Alloc in Finance Settings to configure how a profit should be used.

**Changes:**

- **Support for year end closing introduced**. This includes year end transfer of current year's profit/loss to the balance at year end (possible to disable) and an optional setup of rules related to year end due of tax, dividend, etc. For compatibility reasons, this functionality is disabled but can be switched on in the new page Year End Alloc. in the Finance Settings workbook. (#311)

- **Improved language support**. Danish and Swedish introduced as new languages. Account dimension includes new Danish and Swedish language properties. Self-service setting tables for maintaining solution specific language texts such as input module heading, measures, etc as well as reports, report lines, etc.

  Translations tabs are found in workbooks Report Setup (dimensions, reports, report lines) and Input Settings and Administration (module specific texts). NO and EN texts are copied to the new translation table during upgrade and the NO and EN columns are closed.

  Note that some administrative workbooks such as Data Import & Export, Operations, etc. still supports English and Norwegian only. (#1363)

- **Improvements Personnel module** (#1056 #1382)

  Attributes introduced to personnel module and associated source fact. Previously, dimensionality could be used for defining a row in the personnel module. This is often ok, but in some cases, it is not possible as changing such a value from the source will introduce a new key and duplicate the input row.

  A new type of column, attribute, is therefore introduced (Attr1, Attr2). Attributes are **updated** from source and not part of the dimensional key.

  Ability to control which fields are updated from source fact. Previously, all fields were updated when choosing to update existing data.

  Ability to define auto transaction rules based on attributes. Previously, dimensionality for department and employee alone was used.

  Ability to sort employees in input sheet drop down list so that new (dummy) employees appear at the top.

- **Ability to save input data without submitting** Introduce setting that allows for Save without submit in input modules Account, Personnel and Driver based. In some cases, submitting takes too long.

  Default is Auto submit data

  If Auto submit data is switched off, the Save button saves input data only and a Submit button appears the the user will have to click once the changes are done and saved. The Submit button will update reports. (#1387)

- Allocation - transaction report include Legal entity, account details report will show which allocation rule that has produced the data (#1373)

- Account drill to details report - details from the employee dimension is hidden and data is aggregated to other dimensionality present and any input comments are hidden. This applies to access from Plan Overview and modules other than Personnel as data may be sensitive (#1407)

**Bug fixes and GitLab reference list:**

- Incorrect button heading when adding custom button to driver based models (#1393)

- Auto transactions is not working for non-summable measures in driver based models (#1389)

- Additional dimension mapping cause double report lines (#1388)

- Bad performance in Account details and Account details drilldown report (#1359 #1384)

- Financial engine source - filter out transactions where accountid is null (#1381)

- Personnel module: changes in setting table require double save to take effect (#1355)

- Allocation transaction is deleted when Finance baseline is executed (#1400)

- Transaction pipelines for Account, Personnel and Driver based will truncate it's target table before piping data if run out of context, e.g. from an operation (#1399)

- Account details drill to detail report - employee information displayed when accessing report from Plan Overview (#1406)

## EPM Planner 5.4.0

**Breaking Changes:**

These are changes that change the way Profitbase generate planner results or other changes that will require attention when upgrading and creating new version copying from existing versions.

- Pension calculation calculated for vacation month (where vacation month is determined from vacation pay due) by "copy" pension calculated from following month. This assumes salary is not payed in vacation month. This feature was implmented in Planner 5.1.0.

    The change in Planner version 5.4.0 is that there is a "Plus Vacation Mth" flag in PensionDueTerm to control this feature. (#659)

    The default for this flag is:
  - On - when upgrading solutions to make behaviour compatible with behaviour before Planner 5.4.0
  - Off - for new deployments

- Pension cost posting is changed to follow salary cost transaction date. In previous versions, pension cost posting was following the salary payment due. Use Finance Settings PensionDueTerm to control pension payment due. This change will only apply when salary accrued is in different month from salary posting. (#983)

**Changes:**

- **Plan overview: Display transactions in company's home currency in account drill to details report** Behavior of drill to detail report changed to always display transactions converted to the home currency (i.e. similar to the Account Details report) (#1275).
- Input sheet dimension drop down lists are sorted by Description (#634)
- Plan Overview performance improvement on summary nodes and included counterparty transactions on leaf-nodes. Update and save SalesAccountIC to ensure that counteraccounts are updated. (#1317).
- Improved functionality when legal entity association to department is changed. Historically, a change of legal entity association to a department has caused inconsistencies. The following improvements are made: Plan over view datastore is reloaded in stead of updated, taking into account only the current association. Input data is attempted updated with the current legal entity / currency functional data leaving the input numbers as is so long as this do not cause duplicates (new association assumed to be a correction to an incorrect association in the first place). The Audit report will report any duplicate department / legal entity combinations (#225)
- Driver based module - introduced Update method for updating data. To be used in cases when it is desirable to let the user control which input rows should exist, i.e. only existing input rows are updated, no new rows are inserted. Controlled with base setting DriverBasedLoadExternalSource (#1320)
- Comments from input included in account drill-to-details report, available from plan overview and input modules (#479)
- Personnel module - ability to control which cost columns the social cost override (OVR) input columns should apply to (#501).
- Option to **automatically adjust from and to dates of historic reference columns** when deploying a new version using the "Start fresh from new start date (reload data)" option. This deployment option is typically used when deploying a new version that constitutes a new plan year (i.e. no roll forward) using an existing version (typically this year's plan) as the source version so that relevant config and settings are copied. The deployment will reload data and in order to have as correct historic reference information as possible (controls content in input sheets in Account and Driver Based), a new base setting (AutoAdjustHistRefSetup) is introduced that will (if set to TRUE which is also the default) auto adjust the from and to dates of any historic reference columns used based on the difference between the start date of the source version and the new start date. This adjustment is done prior to the reload so that the reload is affected by the adjusted from and to dates (#1306).
- **Allocation functionality** included in a **preview** state which means it is function-complete but testing, usability development and documentation/help is not complete but will be completed in later version(s). Implementation projects that require allocation functionality may however start using the functionality and report back on functional fit and encountered issues. (#130)
- Plan overview - Dim1..Dim4 introduced as filters (#1245)
- Investments - investment amounts eligible for Plan Overview if asset account is mapped to a Plan Overview report line (#1055). It is also possible to input investments in year prior to the plan year so that depreciation from any investments planned in the year leading up to the plan year but is not yet booked as a fixed asset is included in the plan (#978)
- Built-in cell sum, count and average function for input sheets and reports enabled (#1319)

   **Improved customization options** (ref. [Customization Patterns](https://profitbasedocs.blob.core.windows.net/enduserhelp/files/V5.3/Planner%20Customization%20Patterns.pdf)document for details):

- Ability to execute custom script logic in FinanceAll dataflows (#1308) The FinanceAll dataflows (FinanceAllBaseline and FinanceAllScenario) source transactions from input modules and executes financial engines producing a complete future ledger. In some cases it may be desirable to introduce custom logic before the financial engines starts and after the financial engines have finished. One use case may be to add scenario data to the sales price list from say a driver-based model before the execution of financial engines if simulation on price is required.
- Ability to configure a custom button and a custom row context menu in Plan overview and input modules for Account, Personnel and Driver based. The custom buttons and row context menus can open a custom workbook and signal the dimensional context from the calling workbook. (#1217)
- EPM Datamart destination store for imported **plan data** accessible via synonym SYN_Datamart_pbTransdataSourceCMExternal. Use Data Import to import plan data from external sources that are to be included as transactions (i.e. no input) in the plan to this store. SQL Script ImportFrompbTransdataSourceCMExternal in EPM Planner can be used to import the data to a planner version identified by a ModuleExtensionID (create an operation and include script). (#1344)

**Bug fixes and GitLab reference list:**

- TaxPct table is not working with more than one line which means that different tax percent cannot be set for legal entities. Must use the same tax percent (average) for all legal entities. Fixed by settings on ranked in put configuration (in designer) (#1295)
- Plan overview: Counterparty transactions not visible on default department (#1196)
- Pension in vacation month handeled different in finance reports and plan overview (#1224)
- Finance settings: Rules with different value in key columns does not work in SalesAccountsIC table (#1243)
- Loan principal override error: Could not read .. RowCount (#1300)
- Input on Balansesheet account create transaction with wrong account type (#1304)
- Run dataflow (async) when opening workbooks where the user is likely to run a dataflow (#1326)
- Finance Reports when selecting preview dataset and report different from INPUT report may give wrong results for other PL type reports and no results for BAL type reports (#1331)
- Version manager -> Version properties - data can be loaded when no filter is selected (#721)
- Investment module, postings not deleted after reloading a new version from a new start date (#1318)
- Account module: Periodic comparison report has incorrect monthly offset, relevant for non-standard fiscal regimes (#1321)
- Incorrect currency conversion at group levels on historic columns in Plan Overview (#1334)
- Finance Advanced Input - error received when saving payroll data at non-leaf department level (#1337)
- Personal Autotransaction is calculating wrong numbers with use of Headcount (#1294)
- Fixed Assets "Declining" validation error (#1301)
- Deviation columns - missing column totals and introduce deviation columns in account details report (#1299)
- Account details report - unable to use planner-internal historical datasets (#1283)
- Double output from preview dataset in finance reports (#1244)
- Plan overview - some reportlines appear with 0s in plan periods although plan contains data (after moving department to another legal entity) (#1242)
- Incorrect calculation of social cost from Driver based models. Occurs when saving input in a given context (#1238)
- Finance Advanced Input -> Setup: When no model filter is selected, dimension configuration for all models are displayed (#1222)
- Insert new row only possible when hovering over existing rows or sum/inline filter row (#1204)
- Finance Reports (local to Planner) reports error when drilling from trial balance to details (#1341)
- Driver based models - roll over to new year caused year 2 numbers to be nulled (#1329)
- Import Fixed Assets from external source is not using Group Account mapping (#1346)
