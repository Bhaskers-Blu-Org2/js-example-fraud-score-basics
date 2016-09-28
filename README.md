js-example-fraud-score-basics
=============================

Simple code example demonstrating the use of DeployR as a real-time, R analytics scoring engine.

A more complete `js-example-fraud-score` example application can be found [here](https://github.com/microsoft/js-example-fraud-score).

- [About The Example](#about)
- [Example R Analytics](#example-r-analytics)
- [Example Application](#example-application)
- [Running The Example](#running-the-example)
- [DeployR Integration Details](#deployr-integration-details)
- [License](#license)

## About

This example demonstrates the use of
[DeployR](https://msdn.microsoft.com/en-us/microsoft-r/deployr-about) as a real-time, R
analytics scoring engine. The example scenario mimics a real world application where employees at a fictitious bank can request _fraud scores_ for bank account records to help detect fraudulent account activity.

This example is built using the DeployR [RBroker
Framework](https://msdn.microsoft.com/en-us/microsoft-r/deployr-rbroker-framework), the simplest way
to integrate R analytics inside any Java, JavaScript or .NET
application. This example consists of two distinct parts:

1. Example R Analytics
2. Example Application

The final section of this document provides additional details regarding the
DeployR integration implemented for this example.

## Example R Analytics

```
Source: analytics/*
```

This example uses an R model built to score fictitious bank account data
to help uncover fraudulent account activity. The model used is found
here:

```
analytics/fraudModel.rData
```

The model uses three variables associated with individual bank accounts:

1. The number of transactions on the account
2. The account balance
3. The credit line on the account

This example makes use of a scoring function that uses the model to help
determine the likelihood of _fraud_ on a given account based on these
data inputs. The scoring function is found here:

```
analytics/ccFraudScore.R
```

The R scripts and data models used by this example application are
bundled by default within the DeployR repository, inside the
example-fraud-score directory owned by testuser.

However, if for any reason your DeployR repository does not contain
these files you can add them using the DeployR Repository Manager as
follows:

1. Log in as testuser into the Repository Manager
2. Create a new repository directory called example-fraud-score
3. Upload analytics/fraudModel.rData to the example-fraud-score
   directory
4. Upload analytics/ccFraudScore.R to the example-fraud-score directory

## Example Application

Source:

```
fraud-score.js
```

The example application is implemented as a simple [Node.js application](https://nodejs.org), using the [RBroker Framework](https://msdn.microsoft.com/en-us/microsoft-r/deployr-tools-and-samples#rbroker-framework) for integration with DeployR.

The application works as follows:

1. Creates an instance of RBroker, representing a pool of R sessions on the DeployR server.
2. Preloads the fraud-score model into the workspace for each R session in the pool.
3. Registers asynchronous callback listeners for task completion and error events in the application.
4. Once RBroker instance initialized, submits sample fraud-score tasks for execution.
5. Demonstrates task completion and error handling.
5. Releases the instance of RBroker and exits.

We recommend reading the comments provided within the `fraud-score.js` source file for further details. 


## Running the Example

Use the [DeployR CLI](https://github.com/microsoft/deployr-cli) to download and run the `js-example-fraud-score-basics` example. You can observe the console output generated by the application in your terminal window.


Alternatively you can run it inline:

1. npm install
2. Edit `/config/broker.json` to set your DeployR server endpoint `host` and
   user `credentials`.
3.`$  node fraud-score.js`
4. View stdout for logging statements

## DeployR Integration Details

#### R Analytics Dependencies

DeployR-powered applications typically depend on repository-managed R analytics
scripts, models and/or data files. See the DeployR [Repository Manager](https://msdn.microsoft.com/en-us/microsoft-r/deployr-repository-manager/deployr-repository-manager-about) for details on how best to manage your own R analytics dependencies.

This example depends on two repository-managed files:

1. /testuser/example-fraud-score/fraudModel.rData
2. /testuser/example-fraud-score/ccFraudScore.R

Both files, an R model and scoring function respectively, are owned by _testuser_ and can be found in the _example-fraud-score_ repository-managed directory owned by _testuser_.

These example file dependencies ship, pre-deployed in the DeployR repository so there is no further action for you to take in order for this example to use them. Typically a `data scientist` would first develop and then provide these analytics files to an `application developer` for integration.

#### RBroker Framework - Pooled Task Runtime

This examples uses the [RBroker Framework](https://msdn.microsoft.com/en-us/microsoft-r/deployr-tools-and-samples#rbroker-framework) to integrate _DeployR_ real-time scoring capabilities inside the example application.

Specifically, this example uses the [Pooled Task Runtime](https://msdn.microsoft.com/en-us/microsoft-r/deployr-rbroker-framework#pooled-task-runtime) provided by the _RBroker Framework_.

#### RBroker Framework - Throughput

We recommend experimenting with the size of the pool and the number of tasks executed and observe the effects on throughput. To change the size of the pool used by the example application update the `maxConcurrentTaskLimit` propety in the `config/broker.json` file. To change the number of tasks executed by the example application update the `tasksize` property in the `config/app.json` file.

See the following sections of the _RBroker Framework_ 
tutorial for related details:

- [Client Application Profiling](https://msdn.microsoft.com/en-us/microsoft-r/deployr-rbroker-framework#client-application-profiling)
- [Grid Resource Management](https://msdn.microsoft.com/en-us/microsoft-r/deployr-rbroker-framework#gridprimer) 

## License ##

Copyright (C) 2010-2016, Microsoft Corporation

This program is licensed to you under the terms of Version 2.0 of the
Apache License. This program is distributed WITHOUT
ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING THOSE OF NON-INFRINGEMENT,
MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE. Please refer to the
Apache License 2.0 (http://www.apache.org/licenses/LICENSE-2.0) for more 
details.
