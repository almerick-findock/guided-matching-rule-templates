# Guided Matching Rule Templates


## What is the purpose of this repository?
This repository serves as a central location for contributing, organizing, and maintaining Guided Matching Rule Templates available through Guided Matching Setup.


## How can I contribute to Rule Templates?
Please follow these steps to contribute a new or update an existing Guided Matching rule template:

### 1. Clone the repository.
Fork and clone the repository to your local Git repository.

### 2. Create or open a bank-specific folder for your template.
All Rule Templates are stored under the `Transaction/BankFeed/` folder and are bank-specific. Each supported bank has its own subfolder named after the first four letters of the bank's BIC code. Before contributing a new rule template, create a new bank-specific folder if it does not exist. If a folder for the bank already exists, you can use the file inside that folder.
Example:
- Folder for ING: `Transaction/BankFeed/INGB`

### 3. Create or modify the template file.
Each bank-specific folder must contain a single `rules.json` file with all matching rules for that bank:
- Create a `rules.json` file inside the bank-specific folder if it does not exist.
**OR**
- Open the existing `rules.json` file from the bank-specific folder if you want to modify an existing templates.

### 4. Configure the rule in Salesforce.
Use Guided Matching Setup in your FinDock instance to create or update the matching rule for the given bank (Target). The configured rule will be used as a boilerplate for the rule template. Ensure the setup is complete and saved before proceeding to the next step.

### 5. Export the rule using Salesforce Inspector.
1. Open Salesforce Inspector (Chrome Extension).
2. Use the "Data Export" feature.
3. Use the SOQL query below to find the rule that you have just created or modified:
```
SELECT Id, cpm__Rules__c FROM cpm__Guided_Matching_Setup__c WHERE cpm__Target__c = '**Target**'
```
> Note: Replace **Target** with the Target value of the Guided Matching Setup that you modified in Step 3.
4. Copy the value of the `cpm__Rules__c` field into the file from **Step 3**. If the file already contains templates, add the value from the `cpm__Rules__c` field to the end of the file.
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


### 6. Clean up rules to format the template.
The value you copied in **Step 5** contains all the rules from your target's Guided Matching Setup in JSON format. It must be cleaned up before the template can be submitted to the repository.
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
After cleaning up your template file, commit your changes to your forked repository. Then, create a Merge Request to the main repository. Make sure to provide a clear description of the changes and reference the bank and rule you are contributing or updating. The repository maintainers will review your Merge Request and provide feedback or merge your contribution.