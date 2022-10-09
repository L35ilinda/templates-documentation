# Introduction 
The productivity solution is composed of an 
- An ETL package, 
- DataWarehouse Tables and views, 
- An Analysis Services Model & 
- Power BI dashboard

## Description of the Solution

*What business process are we measuring? What is the subject of the data model or report? i.e. Queries, Transactions, Work Done by users via AWD etc.*

*Against what main attributes are we measuring the subject against? e.g. transactions over time, per product, per user etc.*

## Purpose & Context of the Solution

*Background of the business need, or why does the solution exist?*
*What does the user do with the data or insight?*
*How/From Where do they consume the data?*
*When do they need to consume the data?*

*If it is a process that is consuming the data, please ensure to document the schema expected, naming conventions and the process to change these*

When an instruction is received it goes through various stages of the business process, eg indexing, QC, Capture, Auth etc. 
Each of these stages have work that must be done on each and each are represented by a composite if Queue & Status.

## Audience
*Who is the consumer of the data?*

- FNZ internal Team Leaders and Managers
- Client Team Leaders and Managers (only SCI & NinetyOne IP at time of writing).



### In Scope / Out of Scope
*e.g.*

*"This dataset only includes instructions that a closed"*

*"Only includes work that is facilited by within AWD"*

*"stages in the business process must be mapped correctly in AWD, i.e. each distinct busines process is mapped to one queue only and vice versa."*

*"Data does not include instructions that are open"*


## Definition of Work Completed

The measure of one piece of work completed is if a work item (instruction) is moved from one Queue & Status combination to another by a non-system user. In this case we say the user did one piece of work. 

# Getting Started
This solution is comprised of the following 
## Tool Overview
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

## Schedule
*How often does the data destination scheduled to refresh?*
*How often does the data source update*

*What mechanishm triggers the refresh? eg."sql server agent job in sqldtc" or "control-m job" please include job name*

## Expected Latency
*How often does the user expect to use the data?*

*How old should the data be when the user uses it?*


## Dependencies
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


# Data Quality Validations
*How is the data collected being validated?* 
*i.e. If the data in the data source changes and will make the data in the dashboard or report incorrect, how is this being catered for?*

*e.g.* 
- *Describe Primary keys,* 
- *Describe Foreign keys,* 
- *Describe the strictness of data types, etc*


# Security & Privacy
## Security
*How is access to the data controlled? eg. Active Directory, Azure Active Directory* 
*How do we add or remove users who can access the data?*

## Privacy
*How are we distributing the data? eg. via Embedded Power Bi Dashboards in Sharepoint, SSRS report, MS Teams etc*

*Considering the distribution and how the data is consumed, what Privacy controls are  implemented to control what data can be seen? eg. RLS via DAX in Power BI, RLS via DAX in Analysis Services*

# Important Notes

- All users that are on AWD that who's performance is to be measured must be mapped to a team with an accurate start date in the AWD Queue Mapping Tool (FNZ internal Tool)
