# Introduction 
The productivity solution is composed of an 
- An ETL package, 
- DataWarehouse Tables and views, 
- An Analysis Services Model & 
- Power BI dashboard

## Purpose of the solution

The purpose of the dashboard is the measure work done in the Workflow systems AWD. 

## Audience:
- FNZ internal Team Leaders and Managers
- Client Team Leaders and Managers (only SCI & NinetyOne IP at time of writing).

## Context:
When an instruction is received it goes through various stages of the business process, eg indexing, QC, Capture, Auth etc. 
Each of these stages have work that must be done on each and each are represented by a composite if Queue & Status.

## Definition of Work Completed

The measure of one piece of work completed is if a work item (instruction) is moved from one Queue & Status combination to another by a non-system user. In this case we say the user did one piece of work. 

# Getting Started
This solution is comprised of the following 
## Tool Overview:
### Tools Used:
- SSIS (on prem)
- SQL Server (on prem)
- Azure Analysis Services
- Power BI

### Programming/Scripting Languages:
- SQL
- Python
- C#
- PowerShell/Bash
- Dax

## Schedule:
*How often does the data destination scheduled to refresh?*
*How often does the data source update*

*What mechanishm triggers the refresh? eg."sql server agent job in sqldtc" or "control-m job" please include job name*

## Expected Latency
*How often does the user expect to use the data?*
*How old should the data be when the user uses it?*


## Dependencies:
### Data Sources
- *SILGRY-SQLDTC.DataWarehouseStage*
- *SILGRY-SQLDTC.DataWarehouse*
- *SIL-AWD-RPT.AWDDB*

### Data Destinations
- *SILGRY-SQLDTC.DataWarehouseStage*
- *SILGRY-SQLDTC.DataWarehouse*
- *SIL-AWD-RPT.AWDDB*

## Notifications

*After the feed runs, who should receive a message if…*

- *The data source is missing*
- *The ETL job failed and returned an error?* 
- *The ETL job ran successfully but threw an error?*
- *The ETL job ran successfully but failed a business rule validation? For example, Customer sales must be for an existing customer*
- *The ETL job ran successfully but failed a data quality validation?  For example, Invalid state code such as CAN, Invalid zip codes, Invalid gender.*
- *The ETL job ran successfully without errors?* 


# Security & Privacy
## Security
*How is access to the data controlled? eg. Active Directory, Azure Active Directory* 
*How do we add or remove users who can access the data?*

## Privacy
*How are we distributing the data? eg. via Embedded Power Bi Dashboards in Sharepoint, SSRS report, MS Teams etc*

*Considering the distribution and how the data is consumed, what Privacy controls are  implemented to control what data can be seen? eg. RLS via DAX in Power BI, RLS via DAX in Analysis Services*

# Important Notes
**NB: If work is not facilited by AWD or stages in the business process are not mapped correctly in AWD. This dashboard will not work correctly and cannot accommodate this work.**

**NB: All users that are on AWD that who's performance is to be measured must be mapped to a team with an accurate start date in the AWD Queue Mapping Tool (FNZ internal Tool)**
