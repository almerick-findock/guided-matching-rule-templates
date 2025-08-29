# Guided Matching Rule Templates

## What is the purpose of this repository? 
This repository serves as a central location for contributing, organizing, and maintaining Guided Matching Rule Templates used in Guided Matching Setup. 

## How to contribute Bank Feed Rule Templates?
Please follow these steps to contribute a new or updated Guided Matching rule:


### 1. Clone the Repository
Fork and clone the repository to your local machine.


### 2. Create or open a bank-specific folder to store Bank Feed Rule Templates
All Bank Feed Rule Templates are stored under the `Transaction/BankFeed/` folder and are bank-specific. Each supported bank has its own subfolder named after the first four letters of the bank's BIC code. Before contributing a new rule template, create a new bank-specific folder if it does not exist. If a folder for the bank already exists, you can use the file inside that folder.

Examples: 
- Folder for ING: `Transaction/BankFeed/INGB`


### 3. Create or modify the Bank Feed Rule Template file
Each bank-specific folder should contain a single `rules.json` file with all Bank Feed Rule Templates for that bank:

- Create a `rules.json` file inside the bank-specific folder if it does not exist.

**OR**

- Open the existing `rules.json` file from the bank-specific folder if you want to modify existing Bank Feed Rule Templates.


### 4. Configure the Rule in Salesforce
Use the Guided Matching Setup within your Salesforce Org to create or update a Bank Feed Guided Matching rule. The configured rule will be used as a boilerplate for the Bank Feed Rule Template. Ensure the configuration is complete and saved before proceeding to the next step.


### 5. Export the Rule Using Salesforce Inspector
1. Open Salesforce Inspector (Chrome Extension).
2. Use the "Data Export" feature.
3. Use the SOQL query below to find the Bank Feed related rule that you have just created or modified:

```
SELECT Id, cpm__Rules__c FROM cpm__Guided_Matching_Setup__c WHERE cpm__Target__c = '**Target**'
```

> Note: Replace **Target** with the Target value of the Guided Matching Bank Feed Setup that you modified in Step 3.

4. Copy the value of the `cpm__Rules__c` field into the file from **Step 3**. If the file already contains Bank Feed Rule Templates, add the value from the `cpm__Rules__c` field to the end of the file.

The resulting file should look something like this:
```json
[
    {
        "package": "FinDock_BankFeed",
        "managedRuleSetName": "getPaymentReference",
        "ruleType": "managedRuleSet"
    },
    {
        "package": "FinDock_BankFeed",
        "managedRuleSetName": "getInstallmentsByEndToEndId",
        "ruleType": "managedRuleSet"
    },
    {
        "package": "FinDock_BankFeed",
        "managedRuleSetName": "getInstallmentsByPaymentReference",
        "ruleType": "managedRuleSet"
    },
    // ...other rules...
    {
        "iconName": "standard:canvas",
        "ruleType": "regexp",
        "inputFieldName": "cpm__Remittance_Information_Long__c",
        "regExp": "test",
        "regExpCapturingGroup": 0,
        "name": "Extract Payment Reference from Remittance Information",
        "fieldName": "cpm__Payment_Reference__c",
        "fieldType": "string",
        "seqNr": "2",
        "jobNr": "1",
        "id": "SbwO3L-bGM7fmhW"
    }
]
```


### 6. Clean up rules to format the Bank Feed Rule Template
The value you copied in **Step 5** contains all the rules from your Bank Feed configuration in JSON format. It must be cleaned up before the Bank Rule Template can be submitted to the repository.

For example, if you only want to include "Extract Payment Reference from Remittance Information" as a rule in the template:

1. Remove all other rules from the file. The resulting file should look like this:

```json
[
    {
        "iconName": "standard:canvas",
        "ruleType": "regexp",
        "inputFieldName": "cpm__Remittance_Information_Long__c",
        "regExp": "test",
        "regExpCapturingGroup": 0,
        "name": "Extract Payment Reference from Remittance Information",
        "fieldName": "cpm__Payment_Reference__c",
        "fieldType": "string",
        "seqNr": "2",
        "jobNr": "1",
        "id": "SbwO3L-bGM7fmhW"
    }
]
```

> Note: You can add multiple rules to a single Rule Template file, but they must be formatted as valid JSON.

2. Remove the "id" field from all rules in the file.

```json
[
    {
        "iconName": "standard:canvas",
        "ruleType": "regexp",
        "inputFieldName": "cpm__Remittance_Information_Long__c",
        "regExp": "test",
        "regExpCapturingGroup": 0,
        "name": "Extract Payment Reference from Remittance Information",
        "fieldName": "cpm__Payment_Reference__c",
        "fieldType": "string",
        "seqNr": "2",
        "jobNr": "1"
    }
]
```


### 7. Submit your changes for review
After cleaning up your Bank Feed Rule Template file, commit your changes to your forked repository. Then, create a Merge Request to the main repository. Make sure to provide a clear description of the changes and reference the bank and rule you are contributing or updating. The repository maintainers will review your Merge Request and provide feedback or merge your contribution.