# Data Requirements

## Dimensions

All dimensions can either be created using the dimension editor inside the CFO Platform or imported from a source system. The default is to import dimensions in the Parent-Child format.

Please note that any non-visual characters such as as carriage return, line feed and so on should not be used for dimension member ids nor descriptions. For the ID's it is recommended to only use numbers and letters a-z. IDs are limited to 50 characters. Name columns 200 characters.

| Dimension    | Mandatory | Comment  |
| :---         | :---:    | :---     |
| [Account](#account-details) | Mandatory | |
| [Currency](#currency-details) | Mandatory | |
| [Department](#department-details) | Mandatory | |
| [Legal Entity](#legal-entity-details) | Mandatory | |
| [Activity](#standard-dimension-table) | Optional  | |
| [Asset Group](#standard-dimension-table) | Optional | |
| [Dim1](#standard-dimension-table) | Optional | |
| [Dim2](#standard-dimension-table) | Optional | |
| [Dim3](#standard-dimension-table) | Optional | |
| [Dim4](#standard-dimension-table) | Optional | |
| [Employee](#standard-dimension-table) | Optional | Mandatory if Personnel module is used.  |
| [Market](#standard-dimension-table) | Optional | |
| [Product](#standard-dimension-table) | Optional | |
| [Project](#standard-dimension-table) | Optional | |
| [Supplier](#standard-dimension-table) | Optional | |

## Facts / Transactions

| Fact    | Mandatory | Comment  |
| :---         | :---:    | :---     |
| [Ledger Fact](#ledger-fact) | Mandatory | |
| [Personnel Fact](#personnel-fact) | Optional | |
| [Driver Based Fact](#driver-based-fact) | Optional | |
| [Fixed Assets](#fixed-assets) | Optional | |

## Other data

| Name    | Mandatory | Comment  |
| :---         | :---:    | :---     |
| Currency Exchange Rates | Mandatory | Should be the same as used by the ERP system. |
| Report Setup | Mandatory | |
| Time | Mandatory | Usually generated inside the solution.  |

## Account Details

This should be the common / corporate charter of accounts.

| Column Name    | Description | Mandatory  | Comment |
| :--- | :--- | :--- | :--- |
| ID | ID for the Account | Mandatory | Must be unique. |
| Name | Account Name | Mandatory | |
| ParentID | ID for the parent node | Optional | Must be a valid ID or NULL. Any ID with ParentID = NULL is placed at the root level. |
| SignFactor | Should be either 1 or -1. | Mandatory | Tells the sign for the transaction. For example expenses recorded as positive number gives SignFactor 1 while sales recorded as negative numbers gives SignFactor -1. |
| AccTypeID | Grouping for Profit & Loss and Balance accounts | Mandatory | Valid values are PL and BAL. |
| AllowInput | Marks the accounts that will be allowed plan input | Mandatory | Valid values are 1 and 0. |

## Currency Details

| Column Name    | Description | Mandatory  | Comment |
| :--- | :--- | :--- | :--- |
| ID | ID for the Currency | Mandatory | Must be unique. |
| Name | Usually the same as ID for currency | Mandatory | |

## Department Details

Please note that the LegalEntityIDs should be different from the DepartmentIDs. If such cases of equality exist, the best practice would be to prefix the source DepartmentIDs with LegalEntityID before making use of them in Planner. For example, if both a LegalEntityID and a DepartmentID equals ‘pro’, best practice would be to rename the DepartmentID to ‘pro.pro’ thus making it unique.

It is important that DepartmentIDs of the dimension corresponds to the IDs used for actual general ledger data.

| Column Name    | Description | Mandatory  | Comment |
| :--- | :--- | :--- | :--- |
| ID | ID for the Department | Mandatory | Must be unique. |
| Name | Name for the Department | Mandatory | |
| ParentID | ID for the parent node | Optional | Must be a valid ID or NULL. Any ID with ParentID = NULL is placed at the root level. |
| LegalEntityID | ID for the Legal Entity that the department belongs to. | Mandatory | All departments must be tagged with a valid LegalEntityID. |

## Legal Entity Details

| Column Name    | Description | Mandatory  | Comment |
| :--- | :--- | :--- | :--- |
| ID | ID for the Legal Entity | Mandatory | |
| Name | Name for the Legal Entity | Mandatory | |
| ParentID | ID for the parent node | Optional | Must be a valid ID or NULL. Any ID with ParentID = NULL is placed at the root level |
| FunctionalCurrencyID | Home currency for this Legal Entity | Mandatory | |
| OperationTypeID | Type of Legal Entity | Mandatory | Valid options are Main and Elimination. |
| DefaultDepartmentID | Default department for the Legal Entity | Mandatory | Used for situations where a department is not normally given, such as opening balances. |

## Standard dimension table

### The following dimensions use the same format

- Activity
- Asset Group
- Dim1
- Dim2
- Dim3
- Dim4
- Employee
- Market
- Product
- Supplier

| Column Name    | Description | Mandatory  | Comment |
| :--- | :--- | :--- | :--- |
| ID | ID for the dimension member | Mandatory | This ID must be unique |
| Name | Friendly name for the member | Mandatory | |
| ParentID | ID for the parent node | Optional | Must be a valid ID or NULL |

## Ledger Fact

***Mandatory columns***  
***Each dataset must be sent with different file name prefix. See SYS_DatasetID comment for details. *** 
| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| AccountID | Group Account | varchar(50) | Group Account that must correspond to a member in the Account Dimension. Either AccountID or AccountSrcID must be filled. |
| AccountSrcID | AccountID used by the legal entity | varchar(50) | If transcations are already on group accounts this can be skipped |
| SYS_DatasetID | Identitifier of the transaction dataset | Actual Budget Forecast | In order to send different dataset a new file prefix must be used. For example budgetYYYYMM-LegalEntityID.csv for budget while using gltYYYYMM-LegalEntityID.csv for Actual |
| LegalEntityID | Legal Entity ID | varchar(50) | |
| DepartmentID | Department ID | varchar(50) | |
| TransTypeID | Transaction Type |  1 or 0 | Should be 0 for Opening Balance, 1 for regular transactions |
| CurrencyFunctionalID | Functional Currency Code | USD / NOK / EUR.. | |
| AmountFunctionalID | Functional Amount | decimal(18,4) | |
| Transdate | Booking date | datetime | |
| AccTypeID | Classify account type for transaction | PL / BAL | PL for Profit/Loss Accounts. BAL for Balance Accounts |
| FiscalPeriod | Specify the fiscal period for the transaction | YYYYMM | An integer representing the fiscal period |

***Mandatory Consolidation, Optional Planner***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| CurrencyForeignID | Original Transaction currency | USD / NOK / EUR... | Mandatory for Consolidation. Any valid currency code. |
| AmountForeign | Transaction amount | decimal(18,4) | Mandatory for Consolidation |
| CpLegalEntityID | Counter Legal Entity | varchar(50) | Mandatory for Consolidation. Also for Planner if counterpart is used for budget/forecast |

***Optional columns***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| ProductID | Product ID | varchar(50) | |
| MarketID | Market ID | varchar(50) | |
| SupplierID | Supplier ID | varchar(50) ||
| EmployeeID | Employee ID | varchar(50) ||
| Dim1 | Dim1 | varchar(50) ||
| Dim2 | Dim2 | varchar(50) ||
| Dim3 | Dim3 | varchar(50) ||
| Dim4 | Dim4 | varchar(50) ||
| ProjectID | Project ID | varchar(50) ||
| ActivityID | Activity ID | varchar(50) ||

## Personnel Fact

***Mandatory Columns***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| DepartmentID | Department ID | varchar(50) | |
| EmployeeID | Employee ID | varchar(50) ||
| FTE | Curremt Full Time Equivalent position of the employee or group/function at the given department | decimal(18,4) ||
| HC | The current Headcount of the employee or group/function. | decimal(18,4) | |
| MonthlySalary | The current monthly salary for a full time FTE for the employee at the given department | decimal(18,4) ||

***Optional Columns***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| ProjectID | Project ID | varchar(50) ||
| ActivityID | Activity ID | varchar(50) ||
| Dim1ID | Dim1 | varchar(50) ||
| Dim2ID | Dim2 | varchar(50) ||
| Dim3ID | Dim3 | varchar(50) ||
| Dim4ID | Dim4 | varchar(50) ||
| Bonus | The current monthly benefit for a full time FTE | decimal(18,4) ||
| Overtime | The current monthly benefit for a full time FTE | decimal(18,4) ||
| Misc1 | Free use column for additonal values needed | decimal(18,4) ||
| Misc2 |  | decimal(18,4) ||
| Misc3 |  | decimal(18,4) ||
| Misc4 |  | decimal(18,4) ||
| Misc5 |  | decimal(18,4) ||
| EmployerTaxPctOvr | Override vale for employer tax rate | decimal(18,4) | Note that an override value will set aside any setting regime that otherwise might apply to this row. |
| PensionEmployerPctOvr | Override value for pension rate (employer’s part) | decimal(18,4) | Note that an override value will set aside any setting regime that otherwise might apply to this row. |
| PensionEmployeesPctOvr | Override value for pension rate (employee’s part) | decimal(18,4) | Note that an override value will set aside any setting regime that otherwise might apply to this row. |
| VacationPayPctOvr | Override value for vacation pay rate | decimal(18,4) | Note that an override value will set aside any setting regime that otherwise might apply to this row. |
| Attr1 | Free use column for additonal dimmensions needed | varchar(50) | |
| Attr2 | Free use column for additonal dimmensions needed | varchar(50) | |

## Driver Based Fact

***Mandatory Columns***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| DepartmentID | Department ID | varchar(50) | |
| ProductID | Product ID | varchar(50) | |
| MarketID | Market ID | varchar(50) ||
| SystemModelAccountID | The measure ID | varchar(50) ||
| Value | Amount, Quantity, Percentage depending on the nature of the measure | decimal(18,4) | Any amounts must be in the functional currency of the legal entity it belongs to. |
| Year | Year | int | 4-digit year, for example 2025 |
| Month | Month | int | Month Number, 1-12 |

***Optional Columns***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| SupplierID | Supplier ID | varchar(50) ||
| ProjectID | Project ID | varchar(50) ||
| ActivityID | Activity ID | varchar(50) ||
| Dim1 | Dim1 | varchar(50) ||
| Dim2 | Dim2 | varchar(50) ||
| Dim3 | Dim3 | varchar(50) ||
| Dim4 | Dim4 | varchar(50) ||
| CPLegalEntityID | Counterpart Legal Entity ID | varchar(50) ||

## Fixed Assets

***Mandatory Columns***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| LegalEntityID | Legal Entity ID | varchar(50) ||
| TextID | Text ID | varchar(50) ||
| AccountID | Account ID | varchar(50) ||
| AssetCost | Asset Cost | decimal(18,4) ||
| StartDate | Start Date for Depreciation | datetime ||
| DepreciationPct | Depreciation % | decimal(18,4) ||
| AccountDepreciation | Account for depreciation transactions | varchar(50) ||
| CounterAccount | Counter account for depreciation | varchar(50) ||

***Optional Columns***

| Column Name | Description | Valid values | Comment |
| :--- | :--- | :--- | :--- |
| DepartmentID | Department ID | varchar(50) ||
| CurrencyForeignID | Foreign Currency ID | USD / NOK / EUR.. | Any valid currency code |
| DeprAccum | Accumulated Depreciation | decimal(18,4) ||
| ResidualValue | Residual Value | decimal(18,4) ||
| IsDeclining | Declining Depreciation | 1 / 0 ||
| IsFullTaxY1 | Full Tax first year of Depreciation | 1 / 0 ||
| DisposalDate | Disposal Date | datetime ||
| DisposalAmount | Disposal Amount | decimal(18,4) ||
| VATPct | VAT % | decimal(18,4) ||
| DaysOfCredit | Days of Credit | int ||
| AccountAgioDisagio | Agio/Disagio Account | varchar(50) ||
| AccountProfitLoss | Profit/Loss Account | varchar(50) ||
| ProjectID | Project ID | varchar(50) ||
| ActivityID | Activity ID | varchar(50) ||
| Dim1 | Dim1 | varchar(50) ||
| Dim2 | Dim2 | varchar(50) ||
| Dim3 | Dim3 | varchar(50) ||
| Dim4 | Dim4 | varchar(50) ||
| Comments | Comments | varchar(200) ||
