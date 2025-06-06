---
sidebar_position: 15
---

# RESULT

> **NOTE! 🚨**
> All examples use inline queries built with the SOQL Lib Query Builder.
> If you are using a selector, replace `SOQL.of(...)` with `YourSelectorName.query()`.

## toId()

**Apex**

```apex
Id accountId = [SELECT Id FROM Account LIMIT 1].Id;
```

**SOQL Lib**

```apex
Id accountId = SOQL.of(Account.SObjectType).setLimit(1).toId();
```

## doExist

**Apex**

```apex
Integer accountsWithMoreThan100Employees = [
    SELECT COUNT()
    FROM Account
    WHERE NumberOfEmployees > 100
];

Boolean hasAccountsWithMoreThan100Employees = accountsWithMoreThan100Employees > 0;
```

**SOQL Lib**

```apex
Boolean hasAccountsWithMoreThan100Employees = SOQL.of(Account.SObjectType)
    .whereAre(SOQL.Filter(Account.NumberOfEmployees).greaterThan(100))
    .doExist();
```

## toValueOf

**Apex**

```apex
String accountName = [SELECT Name FROM Account WHERE Id = '1234'].Name;
```

**SOQL Lib**

```apex
String accountName = (String) SOQL.of(Account.SObjectType)
    .byId('1234')
    .toValueOf(Account.Name);
```

## toValuesOf

**Apex**

```apex
Set<String> accountNames = new Set<String>();

for (Account acc : [SELECT Name FROM Account]) {
    accountNames.add(acc.Name);
}
```

**SOQL Lib**

```apex
Set<String> accountNames = SOQL.of(Account.SObjectType)
    .byId('1234')
    .toValuesOf(Account.Name);
```

## toInteger

**Apex**

```apex
Integer amountOfExistingAccounts = [SELECT COUNT() FROM Account];
```

**SOQL Lib**

```apex
Integer amountOfExistingAccounts = SOQL.of(Account.SObjectType).toInteger();
```

## toObject

**Apex**

```apex
Account account = [
    SELECT Id, Name
    FROM Account
    WHERE Id = '1234'
];
```

**SOQL Lib**

```apex
Account account = (Account) SOQL.of(Account.SObjectType)
    .with(Account.Id, Account.Name)
    .byId('1234')
    .toObject();
```

## toList

**Apex**

```apex
Account account = [SELECT Id, Name FROM Account];
```

**SOQL Lib**

```apex
List<Account> accounts = SOQL.of(Account.SObjectType).with(Account.Id, Account.Name).toList();
```

## toAggregated

**Apex**

```apex
List<AggregateResult> result = [
    SELECT LeadSource
    FROM Lead
    GROUP BY LeadSource
];
```

**SOQL Lib**

```apex
List<AggregateResult> result = SOQL.of(Lead.SObjectType)
    .with(Lead.LeadSource)
    .groupBy(Lead.LeadSource)
    .toAggregated();
```

## toMap

**Apex**

```apex
Map<Id, Account> idToAccount = new Map<Id, Account>([SELECT Id FROM Account]);
```

**SOQL Lib**

```apex
Map<Id, Account> idToAccount = (Map<Id, Account>) SOQL.of(Account.SObjectType).toMap();
```

## toMap with custom key

**Apex**

```apex
Map<String, Account> nameToAccount = new Map<String, Account>();

for (Account acc : [SELECT Id, Name FROM Account]) {
    nameToAccount.put(acc.Name, acc);
}
```

**SOQL Lib**

```apex
Map<String, Account> nameToAccount = (Map<String, Account>) SOQL.of(Account.SObjectType)
    .toMap(Account.Name);
```

## toMap with custom relationship key

**Apex**

```apex
Map<String, Account> parentCreatedByEmailToAccount = new Map<String, Account>();

for (Account acc : [SELECT Id, Parent.CreatedBy.Email FROM Account]) {
    parentCreatedByEmailToAccount.put(acc.Parent.CreatedBy.Email, acc);
}
```

**SOQL Lib**

```apex
Map<String, Account> parentCreatedByEmailToAccount = (Map<String, Account>) SOQL.of(Account.SObjectType)
    .toMap('Parent.CreatedBy', User.Email);
```

## toMap with custom key and value

**Apex**

```apex
Map<String, String> accountNameToIndustry = new Map<String, String>();

for (Account acc : [SELECT Id, Name, Industry FROM Account]) {
    accountNameToIndustry.put(acc.Name, acc.Industry);
}
```

**SOQL Lib**

```apex
Map<String, String> accountNameToIndustry = SOQL.of(Account.SObjectType)
    .toMap(Account.Name, Account.Industry);
```

## toAggregatedMap

**Apex**

```apex
Map<String, List<Account>> industryToAccounts = new Map<String, List<Account>>();

for (Account acc : [SELECT Id, Name, Industry FROM Account]) {
    if (!industryToAccounts.containsKey(acc.Industry)) {
        industryToAccounts.put(acc.Industry, new List<Acccount>());
    }

    industryToAccounts.get(acc.Industry).put(acc);
}
```

**SOQL Lib**

```apex
Map<String, List<Account>> industryToAccounts = (Map<String, List<Account>>) SOQL.of(Account.SObjectType)
    .toAggregatedMap(Account.Industry);
```

## toAggregatedMap with relationship key

**Apex**

```apex
Map<String, List<Account>> parentCreatedByEmailToAccounts = new Map<String, List<Account>>();

for (Account acc : [SELECT Id, Name, Parent.CreatedBy.Email FROM Account]) {
    if (!parentCreatedByEmailToAccounts.containsKey(acc.Parent.CreatedBy.Email)) {
        parentCreatedByEmailToAccounts.put(acc.Parent.CreatedBy.Email, new List<Account>());
    }

    parentCreatedByEmailToAccounts.get(acc.Parent.CreatedBy.Email).put(acc);
}
```

**SOQL Lib**

```apex
Map<String, List<Account>> parentCreatedByEmailToAccounts = (Map<String, List<Account>>) SOQL.of(Account.SObjectType)
    .toAggregatedMap('Parent.CreatedBy', User.Email);
```

## toAggregatedMap with custom key and value

**Apex**

```apex
Map<String, List<Account>> accountNamesByIndustry = new Map<String, List<String>>();

for (Account acc : [SELECT Id, Name, Industry FROM Account]) {
    if (!accountNamesByIndustry.containsKey(acc.Industry)) {
        accountNamesByIndustry.put(acc.Industry, new List<String>());
    }

    accountNamesByIndustry.get(acc.Industry).put(acc.Name);
}
```

**SOQL Lib**

```apex
Map<String, List<String>> accountNamesByIndustry = SOQL.of(Account.SObjectType)
    .toAggregatedMap(Account.Industry, Account.Name);
```

## toQueryLocator

**Apex**

```apex
Database.QueryLocator queryLocator = Database.getQueryLocator('SELECT Id FROM ACCOUNT');
```

**SOQL Lib**

```apex
Database.QueryLocator queryLocator = SOQL.of(Account.SObjectType).toQueryLocator();
```
