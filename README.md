# SFDC-Quota-Management
SFDC Unmanaged Package for quota management.

Production: https://login.salesforce.com/packaging/installPackage.apexp?p0=04t5e000000zZv0&isdtp=p1
Sandbox: https://test.salesforce.com/packaging/installPackage.apexp?p0=04t5e000000zZv0&isdtp=p1


I put this package together for myself with the simple goal of being able to easily set quotas and compare against *ARR* as opposed of *Amount* on Opportunities. 

I have a lot of custom fields and reports built out in my instance using this functionality but they are not included in this package because the whole point is to make quota management in sfdc easy and customizable. I don't want to make users delete half the stuff I'm using before they can start using this package.

What's Included:
  Custom OBJECT:      Sales Quota (Sales_Quota__c)
                      Related to Opportunities & Users
                  
  Custom Report Type: Quota Attainment
                      Sales Quotas with or without Opportunities
  Example Report:     Quarterly Quota Attainment
  
  Flows:              Associate Opportunity with Quota (Deactivated)
                      Quota Creation	
  
Setup steps:
  Add any additional fields to the Custom Report Type (Quota Attainment) that you want to compare against your quotas. For example an ARR field.
  Add any additional fields to the Custome Object (Sales Quota) that you would like to measure your reps on (for example I also track win rate and ASP against targets which can be set on each individual Sales Quota Record)
  
  Create Sales Quota records.
    The Quota Creation screen flow will walk you through this process, simply run it and for each User you need a quota for put in the requested information and records will be generated automatically. Currently this only supports creating monthly quotas but can be used for quarterly or yearly quotas, just change how the date is grouped in your reports and you should be good to go. Alternatively you can create quota records manually or in excel and import with data loader if you rely on too many fields that aren'y included by default.

    Fill in any fields with relevant targets for whatever time period your quotas are live for (monthly, quarterly, etc.). My team is measured monthly so I have one Quota record per person per month. By default the package ships with 2 custom fields on Sales Quotas, "Quota Amount" - which should be used for the primary raw value you wish to measure against, and "Quota End Date" which should hold the **LAST DAY** of the quota period. This is required in order for the flow included in this package to properly associate opportunities with quotas. The other field to be aware of is the Owner should always be the sales rep in question. 
    
    To recap - a record should have, at the very least:
      Quota End Date
      Owner
      Quota Amount (technically optional)
      Any other metrics/custom fields you wish to measure on a per rep per time period basis

Once you have your quota records created, turn on the included flow, from then on any time that a close date, stage, or owner on any opportunity changes the flow will run and make sure the opportunity is associated with the correct quota. The flow is saved as a template so if you want it to run more or less often than that feel free to change it and deactivate the one I made. 

Update: 11/15/2022
As of Version 1.7 a field has been added to the Sales Quota object to track months carrying quota. Internally I set this when i create quotas from the screen flow but it gets trickier in this package since quotas can be quarterly or annual not just monthly. THere is a field on the user for the month they started carrying a quota. If you wish to use this feature set that field to the last day of the first month the rep will be carrying a quota (even if it's a $0 ramp quota). This can be useful for tracking ramp times across reps starting in different months. I also updated the flow that associates opportunities with quotas to auto-create a quota if none is found based on the users quota amount (stored on the user record when the screen flow is run).

NON STANDARD QUOTA PERIODS:
As of V 1.9 all screen flow logic has been comsolodated into a single path. If you need to measure quotas on a period that is not pre-configured (for example 2 or 3 times a year) simply add an option for it in the screen where you choose quota period and edit the EndDate formula to add in your picklist value and how many months it should represent and you're good to go!
