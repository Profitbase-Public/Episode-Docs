# Upgrade notes EPM Planner

## Upgrade notes

- [EPM Planner 6.2.4](#epm-planner-624)
- [EPM Planner 6.2.3](#epm-planner-623)
- [EPM Planner 6.2.2](#epm-planner-622)
- [EPM Planner 6.2.1](#epm-planner-621)
- [EPM Planner 6.2.0](#epm-planner-620)
- [EPM Planner 6.1.1](#epm-planner-611)
- [EPM Planner 6.1.0](#epm-planner-610)
- [EPM Planner 6.0.1](#epm-planner-601)
- [EPM Planner 6.0.0](#epm-planner-600)
- [EPM Planner 5.5.0](#epm-planner-550)
- [EPM Planner 5.4.1](#epm-planner-541)

## EPM PLanner 6.2.4

See [EPM Planner 6.2.1](#epm-planner-621)

## EPM PLanner 6.2.3

See [EPM Planner 6.2.1](#epm-planner-621)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/90)

## EPM PLanner 6.2.2

See [EPM Planner 6.2.1](#epm-planner-621)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/87)

## EPM Planner 6.2.1
Supports upgrade to Planner 6.2.1 from Planner 5.5.1 or newer versions (see note below).

When upgrading to the next version after 6.2.1, upgrading from versions older than 6.0.3 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible. 

Furthermore, switching to flow-based transaction pipelines is currently mandatory and is done automatically as part of the upgrade process. However, the flow pipelines must be executed manually post upgrade in situations where the solution was not activated for flow pipelines prior to the upgrade.

Requirements: Hypergene InVision platform version 2026.1 AND Hypergene Flow 1.12.

It is highly recommended that all versions should either be upgraded or deleted (if no longer in use), regardless of their status

**NOTE:** 	

There is an ongoing process to migrate data flows to flows (data flows will be deprecated and replaced with flows). If you have used Planner dataflows in custom logic, be aware that the following data flows will no longer be available from the version preceding Planner 6.2.1:

	FinanceAllBaseline (dataflow) -> replace with 'FinanceAllBaselineFlow' (flow)
	FctAccInputPipeline (dataflow) -> replace with 'Account pipeline' (flow)
	FctSalaryInputPipeline (dataflow) -> replace with 'Salary pipeline' (flow)
	FctSalesInputPipeline (dataflow) -> replace with 'Driver based pipeline' (flow)
		
A number of InVision objects used pre-flow has been removed in this version, for example transaction tables and views used pre-flow. If you have custom logic, make sure to do a 'Update dependency Registry' analysis (InVision designer) to check whether any of your references to Planner are no longer valid. Contact Profitbase if you need assistance.
 
**NOTE:** 	The solution you want to upgrade MUST be connected to Flow BEFORE you upgrade, contact Profitbase to make sure that that is the case or how to connect.

**Involves packages:**

EPM Planner (Package upgraded to: 6.2.1)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/84)

**NOTE also specifically** that some solutions, typically deployed 2023 or prior, may have issues with upgrading the blueprint version due to a platform change relating to table naming. To check whether this may be an issue, open the InVision designer and browse a table such as CashInterestPct and study the table name presented in the browser. If the name is of the form Setting_CashInterestPct_5b199_0919202504015431 you are all right. If it is of the form Setting_CashInterestPct_090620211160304 (missing the middle bit such as _5b199_ in the table name that was all right), you are not all right and that particular blueprint cannot be upgraded. The workaround for this is to upgrade versions only (they will normally not have this problem assuming they have been deployed after 2023) and set one of them as the blueprint version once upgraded. If you need any help with this, please contact Profitbase.

## EPM Planner 6.2.0
Supports upgrade to Planner 6.2.0 from Planner 5.5.0 or newer versions (see note below).

When upgrading to the next version after 6.2.0, upgrading from versions older than 5.5.1 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible. 

Furthermore, switching to flow-based transaction pipelines is currently mandatory and is done automatically as part of the upgrade process. However, the flow pipelines must be executed manually post upgrade in situations where the solution was not activated for flow pipelines prior to the upgrade.

Requirements: Hypergene InVision platform version 2025.7 AND Hypergene Flow 1.11.

It is highly recommended that all versions should either be upgraded or deleted (if no longer in use), regardless of their status

**NOTE:** 	

There is an ongoing process to migrate data flows to flows (data flows will be deprecated and replaced with flows). If you have used Planner dataflows in custom logic, be aware that the following data flows will no longer be available from the version preceding Planner 6.2.0:

	FinanceAllBaseline (dataflow) -> replace with 'FinanceAllBaselineFlow' (flow)
	FctAccInputPipeline (dataflow) -> replace with 'Account pipeline' (flow)
	FctSalaryInputPipeline (dataflow) -> replace with 'Salary pipeline' (flow)
	FctSalesInputPipeline (dataflow) -> replace with 'Driver based pipeline' (flow)
		
 A number of InVision objects used pre-flow has been removed in this version, for example transaction tables and views used pre-flow. If you have custom logic, make sure to do a 'Update dependency Registry' analysis (InVision designer) to check whether any of your references to Planner are no longer valid. Contact Profitbase if you need assistance.

It is highly recommended that all versions should either be upgraded or deleted (if no longer in use), regardless of their status
 
**NOTE:** 	The solution you want to upgrade MUST be connected to Flow BEFORE you upgrade, contact Profitbase to make sure that that is the case or how to connect.

**Involves packages:**

EPM Planner (Package upgraded to: 6.2.0)
EPM Reporting (Package upgraded to: 1.3.0)
EPM Finance Reports (Package upgraded to: 1.2.3)
EPM Common (Package upgraded to: 6.1.1)
EPM Datamart (Package upgraded to: 6.1.1)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/80)

## EPM Planner 6.1.1

Supports upgrade to Planner 6.1.1 from Planner 5.4.1 or newer versions (see note below).

When upgrading to the next version after 6.1.1, upgrading from versions older than 5.5.0 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible. 

Furthermore, switching to flow-based transaction pipelines is currently mandatory and is done automatically as part of the upgrade process. However, the flow pipelines must be executed manually post upgrade in situations where the solution was not activated for flow pipelines prior to the upgrade.

Requirements: Hypergene InVision platform version 2025.5 AND Hypergene Flow 1.9.

**NOTE:** 	The solution you want to upgrade MUST be connected to Flow BEFORE you upgrade, contact Profitbase to make sure that that is the case or how to connect.

**Involves packages:**

EPM Planner (Package upgraded to: 6.1.1)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/74)

## EPM Planner 6.1.0

Supports upgrade to Planner 6.1.0 from Planner 5.4.0 or newer versions (see note below).

When upgrading to the next version after 6.1.0, upgrading from versions older than 5.4.1 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible. 

Furthermore, switching to flow-based transaction pipelines is currently mandatory and is done automatically as part of the upgrade process. However, the flow pipelines must be executed manually post upgrade in situation where the solution was not activated for flow pipelines prior to the upgrade.

Requirements: Hypergene InVision platform version 2025.3 AND Hypergene Flow 1.6. 

**NOTE:** 	The solution you want to upgrade MUST be connected to Flow BEFORE you upgrade, contact Profitbase to make sure that that is the case or how to connect.

**NOTE:**	Planner 6.1.0 has some breaking changes that may require you to make changes to your solution if you have made customizations. This applies to:</br></br>
		- The use of custom assumption views for driver based models. New fields EmployeeID, Attr1 and Attr2 has to be manually added with default values * in order to conform with new driver based assumption format.</br></br>
		- Compatibility Driver based flow interface:</br>
		--- LinkSeq: new parameter to flow, defaults to 0</br>
		--- Flow now incorporates FctSalesInputRecalculate sql script that will perform assumption lookup and recalculate calculated measures</br>

**Involves packages:**

EPM Planner (Package upgraded to: 6.1.0)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/71)

## EPM Planner 6.0.2

Supports upgrade to Planner 6.0.2 from Planner 5.3.0 or newer versions (see note below).

When upgrading to the next version after 6.0.2, upgrading from versions older than 5.4.0 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible. 
Furthermore, while switching to flow-based transaction pipelines is currently optional, it is strongly recommended to do so as soon as possible as dual pipeline support will be deprecated in the next upcoming Planner release 6.1.0.

Requirements: Hypergene InVision platform version 2024.4 AND Hypergene Flow 1.3. NOTE that if your solution is already upgraded to Planner 6.0.0 or 6.0.1, upgrading to Flow 1.3 will cause the pipeline flows to fail so that upgradeing to Flow 1.3 and Planner 6.0.2 should be synchronized.
Alternatively, existing flow pipelines may be patched prior to upgrading to Flow 1.3. Contact Profitbase for details on the patch procedure.

**NOTE:** 	The solution you want to upgrade MUST be connected to Flow BEFORE you upgrade, contact Profitbase to make sure that that is the case or how to connect.

**NOTE:**	Planner 6.0.x has some breaking changes that may require you to make changes to your solution. This applies to:
		- The use of auto transaction condition expressions. This is no longer supported, however separate condition columns exist for defining period-based expressions to cater for old expressions such as month() == 12.
		- The use of transaction pipeline custom scripts: requires you to connect these manually in the pre-/post- custom sub flows that are available in the standard flows. Contact Profitbase for details and/or consult the 
		latest version of the "Planner Customization Patterns" documentation).
		- Having switched to using flow pipelines, new PL transaction tables are used. This applies to pbTransdataPLSourcexyz, new tables suffixed with Flow. This may require you to alter custom logic.

**Notes:**

After upgrading to Planner 6.0.2, you can switch to using flow based transaction pipelines by:
	- Executing script "ActivateFlowPipelines"
	- Executing the dataflow "Recreate Views when PipelineExecutionMode changed"
	- Make a change (e.g. change a comment cell) in each of the distribution key settings for account, personnel and driver based and save. This can be done from the workbook Input settings and administration
	- Opening each of the associated flows from the InVison designer and execute them.
	- (it is possible to switch back to the traditional dataflow based pipelines by executing the script "DeActivateFlowPipelines" and executing the dataflow "Recreate Views when PipelineExecutionMode changed" and thereafter executing the dataflow based transaction pipelines)

**Involves packages:**

EPM Planner (Package upgraded to: 6.0.2)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/67)

## EPM Planner 6.0.1

Supports upgrade to Planner 6.0.1 from Planner 5.2.4 or newer versions (see note below).

When upgrading to the next version after 6.0.1, upgrading from versions older than 5.3.0 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible.

Requirements: Hypergene InVision platform version 2024.2 AND Hypergene Flow 1.2 and a Planner solution upgraded to 5.5.1 using the standard procedure

**NOTE:** The solution you want to upgrade MUST be connected to Flow BEFORE you upgrade, contact Profitbase to make sure that that is the case or how to connect.

**NOTE:** Planner 6.0.1 is currently not available for upgrade from the Profitbase store and has to be upgraded from file. Priority to be given to projects under implementation and existing solution with performance issues related to the transaction processing loop. It is required that the solution that you want to upgrade is upgraded to Planner 5.5.1 using the standard procedure beforehand and that you upgrade a single Planner version to 6.0.1 first.

**NOTE:** Planner 6.0.1 has some breaking changes that may require you to make changes to your solution. This applies to:

- The use of auto transaction condition expressions. This is no longer supported, however separate condition columns exist for defining period-based expressions to cater for old expressions such as month() == 12.
- The use of transaction pipeline custom scripts: requires you to connect these manually in the pre-/post- custom sub flows that are available in the standard flows. Contact Profitbase for details and/or consult the latest version of the "Planner Customization Patterns" documentation).
- Having switched to using flow pipelines, new PL transaction tables are used. This applies to pbTransdataPLSourcexyz, new tables suffixed with Flow. This may require you to alter custom logic.

**Notes:**

After upgrading to Planner 6.0.1, you can switch to using flow based transaction pipelines by:

- Executing script "ActivateFlowPipelines"
- Make a change (e.g. change a comment cell) in each of the distribution key settings for account, personnel and driver based and save. This can be done from the workbook Input settings and administration
- Opening each of the associated flows from the InVison designer and execute them.
- (it is possible to switch back to the traditional dataflow based pipelines by executing the script "DeActivateFlowPipelines" and thereafter executing the dataflow based transaction pipelines)

**Involves packages:**

- EPM Planner (Package upgraded to: 6.0.1)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/65)

## EPM Planner 6.0.0

Supports upgrade to Planner 6.0.0 from Planner 5.5.1 (see note below).

When upgrading to the next version after 6.0.0, upgrading from versions older than 5.2.4 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible.

Requirements: Hypergene InVision platform version 2024.2 AND Hypergene Flow 1.0 and a Planner solution upgraded to 5.5.1 using the standard procedure

**NOTE:** The solution you want to upgrade MUST be connected to Flow BEFORE you upgrade, contact Profitbase to make sure that that is the case or how to connect.

**NOTE:** Planner 6.0.0 is currently not available for upgrade from the Profitbase store and has to be upgraded from file. Priority to be given to projects under implementation and existing solution with performance issues related to the transaction processing loop. It is required that the solution that you want to upgrade is upgraded to Planner 5.5.1 using the standard procedure beforehand and that you upgrade a single Planner version to 6.0.0 first.

**NOTE:** Planner 6.0.0 has some breaking changes that may require you to make changes to your solution. This applies to:

- The use of auto transaction condition expressions. This is no longer supported, however separate condition columns exist for defining period-based expressions to cater for old expressions such as month() == 12.
- The use of transaction pipeline custom scripts: requires you to connect these manually in the pre-/post- custom sub flows that are available in the standard flows. Contact Profitbase for details and/or consult the latest version of the "Planner Customization Patterns" documentation).
- Having switched to using flow pipelines, new PL transaction tables are used. This applies to pbTransdataPLSourcexyz, new tables suffixed with Flow. This may require you to alter custom logic.

**NOTE:** Flow HR pipeline does not currently support long term planning. Support will be added in a later version.

**Notes:**
After upgrading to Planner 6.0.0, you can switch to using flow based transaction pipelines by:

- Executing script "ActivateFlowPipelines"
- Make a change (e.g. change a comment cell) in each of the distribution key settings for account, personnel and driver based and save. This can be done from the workbook Input settings and administration
- Opening each of the associated flows from the InVison designer and execute them.
- (it is possible to switch back to the traditional dataflow based pipelines by executing the script "DeActivateFlowPipelines" and thereafter executing the dataflow based transaction pipelines)

**Involves packages:**

- EPM Planner (Package upgraded to: 6.0.0)

**Known issues:** [GitHub Milestone](https://github.com/Profitbase/Planner/milestone/64)

## EPM Planner 5.5.0

Supports upgrade to Planner 5.5.0 from Planner 5.2.1, 5.2.2, 5.2.3, 5.2.4, 5.3.0, 5.4.0 and 5.4.1.

**NOTE: Upgrading from versions older than 5.2.1 is no longer supported!**

When upgrading to the next version after 5.5.0, upgrading from versions older than 5.2.2 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible.

Requirements: Hypergene InVision platform version 2023.7.

Notes:

- After upgrading to Planner 5.5.0 (with Datamart 5.4), it is strongly recommended to use the new FINANCE_ACTUAL for importing finance actual transactions. This option will be more efficient when doing incremental data import by fiscal period and legal entity. Please not the new mandatory column for FiscalPeriod is required when using this import.
- When upgrading to Planner 5.4.1, some workbooks in versions that are not upgraded may produce error messages and not render correctly due to the introduction of new languages. Make sure therefore to upgrade all the versions your users rely on.
- When upgrading to Planner 5.3.0, a new socalled extension package (EPM Finance Reports) is introduced. It will extend EPM Reporting and EPM PLanner with financial reporting capabilities in the for of the Finance Reports workbook. This extension package is automatically deployed when upgrading using the Package Management tool (recommended). If, however, upgrade is performed manually from individual files, the EPM Finance Reports extension package must manually be deployed to the solution containing EPM Reporting after EPM Reporting has been upgraded. It must also be deployed manually to the solution containing the EPM Planner blueprint after the EPM Planner package has been upgraded. In order for individual Planner versions to have its own Finance Reports workbook, process contents must be updated to include EPM Finance Reports. For processes that have EPM Finance Reports included as part of their content, the Finance Report workbook will be available within the versions created thereafter.
- When upgrading to Planner 5.2.4, there is a **breaking change** for customized pre- and post- pipeline scripts as the interface to these scripts have changed. This only applies to to solutions that have made use of these customization possibilities.
- Planner 5.2x makes use of a central dimension management and ledger repository. When upgrading to from a pre-5.2 version each upgraded versions will move the content and configuration from the version's dimensions to the primary dimension residing in the central repository.
It is thus recommended that the version containing the most correct versions of the dimensions is the LAST one to be upgraded. Alternatively you can publish dimension(s) from a given version to the primary dimension located in the EPM Reporting solution after the upgrade is done.
- It is highly recommended that all versions should either be upgraded or deleted (if no longer in use), regardless of their status
- When upgrading from version 5.2.3 or older, the schedules defined for scheduled operations in Operation Manager are removed by mistake (issue #1105). Make therefore sure to document any schedules prior to upgrading.

Involves packages:

- EPM Datamart (Package upgraded to: 5.4.0)
- EPM Common (Package upgraded to: 5.4.0)
- EPM Reporting (Package upgraded to: 1.1.0)
- EPM Finance Reports extension package (Package upgraded to 1.1.0)
- EPM Planner (Package upgraded to: 5.5.0)

## EPM Planner 5.4.1

Supports upgrade to Planner 5.4.1 from Planner 5.2.x, 5.3.0, 5.4.0.

**NOTE: Upgrading from versions older than 5.2.0 is no longer supported!**

When upgrading to the next version after 5.4.1, upgrading from versions older than 5.2.1 will no longer be supported. It is therefore strongly recommended to keep your Planner version as up to date as possible.

Requirements: Hypergene InVision platform version 2023.5.

Notes:

- When upgrading to Planner 5.4.1, some workbooks in versions that are not upgraded may produce error messages and not render correctly due to the introduction of new languages. Make sure therefore to upgrade all the versions your users rely on.
- When upgrading to Planner 5.3.0, a new socalled extension package (EPM Finance Reports) is introduced. It will extend EPM Reporting and EPM PLanner with financial reporting capabilities in the for of the Finance Reports workbook. This extension package is automatically deployed when upgrading using the Package Management tool (recommended). If, however, upgrade is performed manually from individual files, the EPM Finance Reports extension package must manually be deployed to the solution containing EPM Reporting after EPM Reporting has been upgraded. It must also be deployed manually to the solution containing the EPM Planner blueprint after the EPM Planner package has been upgraded. In order for individual Planner versions to have its own Finance Reports workbook, process contents must be updated to include EPM Finance Reports. For processes that have EPM Finance Reports included as part of their content, the Finance Report workbook will be available within the versions created thereafter.
- When upgrading to Planner 5.2.4, there is a **breaking change** for customized pre- and post- pipeline scripts as the interface to these scripts have changed. This only applies to to solutions that have made use of these customization possibilities.
- Planner 5.2x makes use of a central dimension management and ledger repository. When upgrading to from a pre-5.2 version each upgraded versions will move the content and configuration from the version's dimensions to the primary dimension residing in the central repository.
It is thus recommended that the version containing the most correct versions of the dimensions is the LAST one to be upgraded. Alternatively you can publish dimension(s) from a given version to the primary dimension located in the EPM Reporting solution after the upgrade is done.
- It is highly recommended that all versions should either be upgraded or deleted (if no longer in use), regardless of their status
- When upgrading from version 5.2.3 or older, the schedules defined for scheduled operations in Operation Manager are removed by mistake (issue #1105). Make therefore sure to document any schedules prior to upgrading.

Involves packages:

- EPM Datamart (Package upgraded to: 5.3.3)
- EPM Common (Package upgraded to: 5.3.2)
- EPM Reporting (Package upgraded to: 1.0.7)
- EPM Finance Reports extension package (Package upgraded to 1.0.2)
- EPM Planner (Package upgraded to: 5.4.1)
