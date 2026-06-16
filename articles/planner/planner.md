# Planner

Profitbase Planner is a financial planning tool for annual budgeting and rolling forecast processes. It combines configurable input modules, a finance simulation engine, and detailed reporting to deliver accurate balance sheets and cash flow projections.

## Key areas

- [Finance Settings](workbooks/financial-planning/finance-settings.md) — financial settings and assumptions
- [Input Modules](modules/modules.md) — convenient input for year and/or period planning
- [Finance Reports](workbooks/financial-planning/finance-reports.md) — inspect results from simulation
- [Finance Simulation](workbooks/financial-planning/finance-simulation.md) — scenario planning with "best" and "worst" case results
- [Operation Manager](workbooks/administration/operation-manager.md) — run and schedule data processing flows
- [Data Management](workbooks/data-management/data-management.md) — exchange rates, dimensions, and transaction tables
- [Administration](workbooks/administration/administration.md) — user access and module configuration

## How the interface works

Profitbase Planner is organized into [Workbooks](workbooks/workbooks.md), accessible through a side menu. Click the hamburger icon in the upper left corner to open navigation.

![Planner navigation menu](https://profitbasedocs.blob.core.windows.net/plannerimages/menu.jpg)

The menu reflects the work processes (budget, forecast, etc.) and versions you have created. Each version is a self-contained mini solution with its own data and workbooks. The workbooks and data you see depend on your user role — you may only have access to a subset of the full menu. This documentation covers all workbooks regardless of access level.

> [!NOTE]
> Applies to Planner 5 or later. Different work processes can be defined, such as budget, forecast, and similar. For each work process, one or more versions can be created.

## Input modules

The success of a rolling forecast solution depends on how easy it is for users to provide input. Planner offers a rich set of modules to cover everything from simple on-account input to complex driver-based planning:

- [Input on Account](modules/account.md) — traditional account-level input, straightforward for finance users
- [Payroll](modules/personnel.md) — input by employee or category, headcount, full-time percentage, and benefit costs
- [CapEx](workbooks/financial-planning/capex.md) — investment planning
- [Loan](workbooks/financial-planning/loan.md) — loan planning including ongoing loans
- [Driver Based](modules/driver-based.md) — model income and costs from a single driver such as quantity sold *(Planner 5+)*

All input modules are optional, configurable, and can be launched from the profit and loss summary report.

### Example: driver-based input

![Driver based input module](https://profitbasedocs.blob.core.windows.net/plannerimages/salesmodule.jpg)

The driver-based module shows the quantity driver at the top, which controls calculations below. Users can add detail-level adjustments in the middle section. The bottom table displays results posted to financial accounts.

## Planning and simulation workflow

Input combined with finance settings controls the outcome of the finance simulation engine, which calculates balances and generates accurate cash flow. You can also run [scenario simulations](workbooks/financial-planning/finance-simulation.md) to explore best-case and worst-case results. Results can be inspected in Finance Reports or exported for use in external reporting tools.

### Profit and loss summary

![Profit and loss summary report](https://profitbasedocs.blob.core.windows.net/plannerimages/summary.jpg)

### Cash flow report

![Cash flow report](https://profitbasedocs.blob.core.windows.net/plannerimages/cashtotal.jpg)

Simulated cash can be broken down by operational activity to see the effects of VAT terms, vacation pay terms, and other factors on liquidity.
