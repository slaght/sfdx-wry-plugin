sfdx-wry-plugin
===============

Utilities for SFDX JSON data files

[![Version](https://img.shields.io/npm/v/sfdx-wry-plugin.svg)](https://npmjs.org/package/sfdx-wry-plugin)
[![Appveyor CI](https://ci.appveyor.com/api/projects/status/github/billryoung/sfdx-wry-plugin?branch=master&svg=true)](https://ci.appveyor.com/project/heroku/sfdx-wry-plugin/branch/master)
[![Codecov](https://codecov.io/gh/billryoung/sfdx-wry-plugin/branch/master/graph/badge.svg)](https://codecov.io/gh/billryoung/sfdx-wry-plugin)
[![Known Vulnerabilities](https://snyk.io/test/github/billryoung/sfdx-wry-plugin/badge.svg)](https://snyk.io/test/github/billryoung/sfdx-wry-plugin)
[![Downloads/week](https://img.shields.io/npm/dw/sfdx-wry-plugin.svg)](https://npmjs.org/package/sfdx-wry-plugin)
[![License](https://img.shields.io/npm/l/sfdx-wry-plugin.svg)](https://github.com/billryoung/sfdx-wry-plugin/blob/master/package.json)

<!-- toc -->
* [Install](#install)
* [Docs](#docs)
* [Commands](#commands)
<!-- tocstop -->

# Install
<!-- install -->
sfdx plugins:install sfdx-wry-plugin

# Docs
<!-- docs -->
Allows you to use Record Type developer names in JSON files for sfdx's data:tree:import.

The intent is that you first run tool on your JSON data files.  It creates a copy with scratch org specific ID values put in place.  Then you run data:tree:import against that output copy.

Within your JSON files, Record Types can be referenced in one of two ways:
First way, you can use a syntax of $R{RecordType.SobjectType.DeveloperName} such as $R{RecordType.Account.Vendor}
This is a very concise way, good for adding in to pre-existing or manually maintained JSON data files.

Second way, in the data:tree:export command include "RecordType.DeveloperName" in the select fields along with other fields, such as "select Name, BillingState, RecordType.DeveloperName from Account".  This leaves you with a very verbose RecordType reference in the JSON file, but one that this tool can recognize and replace.

 

# Commands
<!-- commands -->
* [`sfdx wry:data:count`](#sfdx-wrydatacount)
* [`sfdx wry:file:replace`](#sfdx-wryfilereplace)
* [`sfdx wry:hello:org [FILE]`](#sfdx-wryhelloorg-file)
* [`sfdx wry:org:limits`](#sfdx-wryorglimits)

## `sfdx wry:data:count`

Counts rows in data objects

```
USAGE
  $ sfdx wry:data:count

OPTIONS
  -o, --objects=objects                           List of objects to check (CSV seperated for multiple) (defaults to
                                                  ALL)

  -u, --targetusername=targetusername             username or alias for the target org; overrides default target org

  --apiversion=apiversion                         override the api version used for api requests made by this command

  --json                                          format output as json

  --loglevel=(trace|debug|info|warn|error|fatal)  logging level for this command invocation

EXAMPLES
  $ sfdx wry:data:count --targetusername myScratchOrg@example.com -o Account,Opportunity,User
  Account: 14 records
  Account.Vendor: 2 records
  Account.Customer: 12 records
  Opportunity: 38 records
  User: 5 records
  
  $ sfdx wry:data:count -u myScratchOrg@example.com -o ALL
  Account: 14 records
  Account.Vendor: 2 records
  Account.Customer: 12 records
  Opportunity: 38 records
  User: 5 records
```

_See code: [src/commands/wry/data/count.ts](https://github.com/billryoung/sfdx-wry-plugin/blob/v0.0.9/src/commands/wry/data/count.ts)_

## `sfdx wry:file:replace`

Replaces RecordType and UserRole references with Id values

```
USAGE
  $ sfdx wry:file:replace

OPTIONS
  -f, --force                                     Force overwrite output directory (not supported yet)
  -i, --inputdir=inputdir                         Directory containing JSON data files

  -o, --outputdir=outputdir                       Target directory for new versions of data files (default:
                                                  $inputdir.out)

  -u, --targetusername=targetusername             username or alias for the target org; overrides default target org

  --apiversion=apiversion                         override the api version used for api requests made by this command

  --json                                          format output as json

  --loglevel=(trace|debug|info|warn|error|fatal)  logging level for this command invocation

EXAMPLES
  $ sfdx wry:file:replace --targetusername myScratchOrg@example.com --inputdir dataFiles --outputdir dataFiles.replaced  

     Copying dataFiles/* to dataFiles.replaced/*
     Account.json: Replacing $R{RecordType.Account.Vendor} with 0125C000000IGVIQA4
     Account.json: Replacing $R{RecordType.Account.Customer} with 0125C000000IGVSQA4
     NewUser.json: Replacing $R{UserRole.CEO} with 00E5C000000UZSmUAO
     QAUser.json: Replacing $R{UserRole.MarketingTeam} with 00E5C000000UZSxUAO
     InternalUsers.json: Replacing $R{UserName.3} with sfdghk4@test.com
     EmployeeUsers.json: Replacing $R{Profile.Staff} with 00e3R000000Um4lQAC
  
  $ sfdx wry:file:replace -u myScratchOrg@example.com -i data  
     Copying data/* to data.out/*
     Account.json: Replacing $R{RecordType.Account.Vendor} with 0125C000000IGVIQA4
     Account.json: Replacing $R{RecordType.Account.Customer} with 0125C000000IGVSQA4
     NewUser.json: Replacing $R{UserRole.CEO} with 00E5C000000UZSmUAO
     QAUser.json: Replacing $R{UserRole.MarketingTeam} with 00E5C000000UZSxUAO
     InternalUsers.json: Replacing $R{UserName.3} with sfdghk4@test.com
     EmployeeUsers.json: Replacing $R{Profile.Staff} with 00e3R000000Um4lQAC
```

_See code: [src/commands/wry/file/replace.ts](https://github.com/billryoung/sfdx-wry-plugin/blob/v0.0.9/src/commands/wry/file/replace.ts)_

## `sfdx wry:hello:org [FILE]`

Prints a greeting and your org id(s)!

```
USAGE
  $ sfdx wry:hello:org [FILE]

OPTIONS
  -f, --force                                      example boolean flag
  -n, --name=name                                  name to print
  -u, --targetusername=targetusername              username or alias for the target org; overrides default target org
  -v, --targetdevhubusername=targetdevhubusername  username or alias for the dev hub org; overrides default dev hub org
  --apiversion=apiversion                          override the api version used for api requests made by this command
  --json                                           format output as json
  --loglevel=(trace|debug|info|warn|error|fatal)   logging level for this command invocation

EXAMPLES
  $ sfdx hello:org --targetusername myOrg@example.com --targetdevhubusername devhub@org.com
     Hello world! This is org: MyOrg and I will be around until Tue Mar 20 2018!
     My hub org id is: 00Dxx000000001234
  
  $ sfdx hello:org --name myname --targetusername myOrg@example.com
     Hello myname! This is org: MyOrg and I will be around until Tue Mar 20 2018!
```

_See code: [src/commands/wry/hello/org.ts](https://github.com/billryoung/sfdx-wry-plugin/blob/v0.0.9/src/commands/wry/hello/org.ts)_

## `sfdx wry:org:limits`

Reports limits in a Salesforce instance

```
USAGE
  $ sfdx wry:org:limits

OPTIONS
  -l, --limits=limits                             List of limits to check (CSV seperated for multiple) (defaults to ALL)
  -u, --targetusername=targetusername             username or alias for the target org; overrides default target org
  --apiversion=apiversion                         override the api version used for api requests made by this command
  --json                                          format output as json
  --loglevel=(trace|debug|info|warn|error|fatal)  logging level for this command invocation

EXAMPLES
  $ sfdx wry:org:limits --targetusername myScratchOrg@example.com
  ConcurrentAsyncGetReportInstances: 200 of 200
  ConcurrentSyncReportRuns: 20 of 20
  DailyAnalyticsDataflowJobExecutions: 40 of 40
  DailyApiRequests: 14419 of 15000
  ...

  $ sfdx wry:org:limits -u myScratchOrg@example.com -l MassEmail,DailyApiRequests,SingleEmail
  MassEmail: 10 of 10
  DailyApiRequests: 14408 of 15000
  SingleEmail: 15 of 15
```

_See code: [src/commands/wry/org/limits.ts](https://github.com/billryoung/sfdx-wry-plugin/blob/v0.0.9/src/commands/wry/org/limits.ts)_
<!-- commandsstop -->
