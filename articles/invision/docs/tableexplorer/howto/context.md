
# How to use the Context Filter

Tables can be marked with a **context**, allowing the **Table Explorer** to display only the relevant ones in any given context. A context typically maps to a business domain like **ESG**, **Sales forecasting**, **Budget** etc.

<br/>

## Example: Context "Budgeting and forecast"

When adding a **Table Explorer** to a Workbook [Page](./../../workbooks/pages.md), setting the **Context filter** to `"Budgeting and forecasting"` will limit the view to only those tables that:

- Are marked as **Display in Table Explorer(s)**
- Have been marked with the context **"Budgeting and forecasting"**

As shown in ***Figure A***, tables such as **Customer Support**, **Facilities Managment**, **Finance & Accounting**, **HR** and **IT** have been assigned this context and are therefore visible.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_web_contextfilter_1.png)
*Figure A: Tables filtered by context "Budgeting and forecasting"*

<br/>

## Example: Context "Tax"

Similarly, configuring the **Context filter** to `"Tax"` will display only those tables that:

- Are enabled for **Display in Table Explorer(s)**
- Are marked with the context **"Tax"**

In ***Figure B***, tables like **Tax Filing**, **Tax Forecast**, **Tax Liability** and **Tax Payment History** are shown, as they match the selected context.


![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_web_contextfilter_2.png)
*Figure B*


<br/>

## Example: Multiple contexts, e.g. "Budgeting and forecast" and "Tax"

Table Explorers may filter multiple contexts to display relevant tables. To define the different scopes, use semicolon to separate multiple contexts in the "Context filter" property. E.g. for filtering "Budgeting and forecast" and "Tax":

- Tables must be enabled for **Display in Table Explorer(s)**
- Tables must be marked with either the context **"Tax"** or **"Budgeting and forecasting"**
- Table Explorer placed on the Workbook Page must have a "context filter" of  **"Budgeting and forecasting;Tax"**

Shown in ***Figure C*** will list tables from both **"Budgeting and forecasting"** and **"Tax"** scoped tables.

![pic](https://profitbasedocs.blob.core.windows.net/images/table_explorer_web_contextfilter_3.png)
*Figure C*