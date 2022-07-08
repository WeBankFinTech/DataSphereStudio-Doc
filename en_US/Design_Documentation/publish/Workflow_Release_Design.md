## Workflow release design

## I. Overview

In actual production applications, the development center is responsible for debugging the business-related workflows. After the debugging is completed, the workflows will be released to the scheduling system for scheduled batch scheduling jobs to realize business automation.

### 2. Workflow release architecture design

Workflow publishing is a relatively complex process, which involves functions such as importing, exporting, and publishing workflows.

##### 1. Publish the process call link diagram:

![img](images/workflow-publish.png)

##### 2. Description of important steps

- Export: Export from Dev Center (dev), including workflow and 3rd party node export. Compress the generated workflow json file into a zip package through the bml service and upload it to the bml file service center.
- Import: Import to production center (prod). Download the zip file saved in the bml file service center, parse the json file, obtain the workflow arrangement information, and save it to the database.
- Publish: Convert the dss workflow arrangement information obtained by importing into the workflow arrangement information available to the scheduling system, compress it into a zip package, and publish it to the wtss scheduling system.
