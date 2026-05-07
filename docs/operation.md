---
sidebar_label: 'Operation'
title: SAPBO Connector operation
description: "How to define and run SAP Business Objects jobs from OpCon Enterprise Manager, including the START job type, report attributes, prompts, destinations, failure criteria, and connector logging."
tags:
  - Reference
  - Automation Engineer
  - SAPBO Connector
---

# Operation

## What is it?

Enterprise Manager includes job subtype definitions for the SAP Business Objects Scheduler. You can access the job subtype by selecting the SAP Business Objects job subtype from the list shown when the Windows Job Type is selected.

:::note
The SAP Business Objects Connector does not create job definitions in the SAP Business Objects database. It only references existing reports.
:::

## Defining a SAP Business Objects job

The SAP Business Objects Connector currently supports a single **START** job type that you can select.

To open the SAP Business Objects Definition screen, complete the following steps:

1. Create or open a job in Enterprise Manager.
2. Select a Job Type of **Windows**.
3. Select a Job Sub-Type of **SAP Business Objects**.

The SAP Business Objects Definition screen appears.

The job definition consists of two areas:

- A **General Definition** area required by all jobs.
- A type-specific area that depends on the selected job type.

### General Definition fields

| Field | Description |
| ----- | ----------- |
| **Connector Path** | Required. The installed location of the connector. This should not be changed; the location should be defined in the **SAPBOPath** property. If more than one SAP Business Objects Connector is installed on the same system, define a new global property and update the entry in this field. |
| **Function** | Required. The function to run. Currently only a **START** function is available. The selected function determines which TABs are available for additional information. |
| **User Name** | Required. The name of a SAP Business Objects user that has the required privileges to run the job. |
| **User Password** | Required. The password of the defined user. Use encrypted global properties for the password — the value is decrypted by the Windows Agent. |
| **CMS Authentication** | Required. The authentication mode associated with the user defined in the **User Name** field. Select a value from the list that matches your installation: **Enterprise**, **LDAP**, **WinAD**, or **SAPR3**. |

### START job fields

The **START** job type schedules a report within the SAP Business Objects environment. When the **START** job type is selected, the **Start** and **Failure Criteria** TABs are available for defining additional information, with the **Start** TAB displayed.

| Field | Description |
| ----- | ----------- |
| **Report Type** | Required. The type of Business Objects report to schedule. Currently, **Crystal Reports**, **Publications**, and **Web Intelligence** reports are supported. |
| **Report ID** | Required. The ID of the Business Objects report to schedule. You can extract this information from the Business Objects system by examining the properties of the report on the Business Objects system. |

:::info
The **Report Attributes**, **Report Prompts**, and **Report Destinations** TABs are only supported for **Web Intelligence** reports.
:::

## Report Attributes

The **Report Attributes** TAB defines attributes that are passed to the report. Attributes are entered as name/value pairs.

To manage attributes:

| Action | Steps |
| ------ | ----- |
| Add | Enter the attribute name in the **Attribute** field and the attribute value in the **Value** field, then select the **Add** button. |
| Modify | Select the attribute in the table — the entry is displayed in the **Attribute** and **Value** fields. Modify the value, then select the **Update** button. |
| Remove | Select the attribute in the table — the entry is displayed in the **Attribute** and **Value** fields. Select the **Remove** button. |

### Special attributes

Two special attributes can be used to define the title of the report and the format of the report:

- **SCHEDULE-TITLE** — Free format title. If omitted, a title consisting of the report ID and a date timestamp is used (for example, `7793_201512011535`).
- **SCHEDULE-FORMAT** — Output format (`webi`, `pdf`, `xls`, or `csv`). If omitted, the default value defined in the `Connector.config` file is used.

## Report Prompts

The **Report Prompts** TAB submits a response to a prompt when a report is scheduled. Each entry consists of the message associated with the prompt and the value to be submitted. You can define multiple prompts for a schedule.

Prompt types can be character, integer, or date/time values. The values are passed as strings in the XML submitted to the web service, and the web service converts them to the appropriate format.

To manage prompts:

| Action | Steps |
| ------ | ----- |
| Add | Start typing in the **Prompt** field, enter the message and value pairs, then select the **Add** button. |
| Modify | Select the prompt in the **Prompt** list — the entry is displayed in the **Prompt** field. Modify the value, then select the **Update** button. |
| Remove | Select the prompt in the **Prompt** list — the entry is displayed in the **Prompt** field. Select the **Remove** button. |

### Multiple values for one prompt

You can enter more than one value for a prompt by separating the values with a semicolon. For example, to add a second Category value to the `Enter values for Category:` prompt:

- **Prompt:** `Enter values for Category:`
- **Value:** `Evening Wear;Jackets`

Example:

```text
Enter values for Lines:                     Dresses
Enter values for Category:                  Evening Wear;Jackets
```

## Report Destinations

The **Report Destinations** TAB defines destinations for the report. The destinations override the default values associated with the report. Destinations are entered as type/value pairs using a destination type identifier and the value.

**Report Destinations** supports the following destination types:

- **DiskUnmanaged** — A directory on disk
- **FTP** — An FTP server defined in `Connector.config`
- **Managed (Inbox)** — A Business Objects user inbox
- **SMTP** — An SMTP server defined in `Connector.config`

### DiskUnmanaged

The **DiskUnmanaged** TAB defines disk directories for the report.

| Field | Description |
| ----- | ----------- |
| **Directory** | The directory where the report is placed (for example, `c:\temp`). |

### FTP

The **Ftp** TAB defines FTP destinations. The FTP definitions are used in conjunction with a defined FTP server in the `Connector.config` file.

| Field | Description |
| ----- | ----------- |
| **Indicator** | An FTP destination identifier. The value `FTP1` is matched with the value of an FTP Server definition in the `Connector.config` file (for example, `[FTP1]`). The values associated with the `FTP1` definition in `Connector.config` are used to define the FTP Server. |
| **Directory** | The directory where the generated file is placed on the FTP server. |

### Managed (Inbox)

The **Managed (Inbox)** TAB defines SAP Business Objects users that receive a copy of the report in their mailbox.

| Field | Description |
| ----- | ----------- |
| **Selected Users** | The list of users that receive the report. See the procedures below to add, modify, or remove a user. |

To manage **Selected Users**:

| Action | Steps |
| ------ | ----- |
| Add | Enter the **user ID** number in the **Users** field, then select the **Add** button. |
| Modify | Select the user in the **Selected Users** list — the user appears in the **Users** field. Modify the value, then select the **Update** button. |
| Remove | Select the user in the **Selected Users** list — the user appears in the **Users** field. Select the **Remove** button. |

:::note
The web services interface does not currently provide the capability to use a name instead of the user ID.
:::

### SMTP

The **Smtp** TAB defines SMTP destinations. The SMTP definitions are used in conjunction with a defined SMTP server in the `Connector.config` file.

| Field | Description |
| ----- | ----------- |
| **Indicator** | An SMTP destination identifier. The value `SMTP1` is matched with the value of an SMTP Server definition in the `Connector.config` file (for example, `[SMTP1]`). The values associated with the `SMTP1` definition in `Connector.config` are used to define the SMTP Server. |
| **Subject** | The information placed in the subject field of the email message. |
| **Message** | A message added to the email message body. If no message is submitted, the default value `Created by OpCon from SMA Solutions` is used. |
| **Email Addresses** | The list of email addresses that receive the report. See the procedures below to add, modify, or remove an address. |

To manage **Email Addresses**:

| Action | Steps |
| ------ | ----- |
| Add | Enter the email address in the **Email Address** field, then select the **Add** button. |
| Modify | Select the address in the **Email Address** list — the address appears in the **Email Address** field. Modify the value, then select the **Update** button. |
| Remove | Select the address in the **Email Address** list — the address appears in the **Email Address** field. Select the **Remove** button. |

## Job-finished processing

The **START** job type requires a Failure Criteria definition. A SAP Business Objects scheduled task has the following possible return codes:

| Code | Status | Meaning |
| ---- | ------ | ------- |
| 200  | FINISHED_OK | The job has been successfully initiated. |
| 201  | FINISHED_OK | The job has been successfully initiated. |
| 1    | ERRORED | An exception occurred during job initiation. |

:::tip
To check for a successful completion, set the Failure Criteria to **NE (Not Equal) to 200** — this is the completed-successfully (OK) code returned when using RESTful web services.
:::

A returned code from SAP Business Objects of *warning* is logged and mapped to an ERRORED condition.

## Logging

The default logging implemented by the connector consists of a maximum cycle of five log files. The log files contain information about the SAP Business Objects Connector and any jobs run by the SAP Business Objects Connector.

- **Location:** ***installation_dir***\\log
- **File names:** `sapbo.log` through `sapbo.log.5`
- **Behavior:** Information is appended into the log files. You can view error messages and return codes in these log files.

## FAQs

**Q: Does the SAPBO Connector create new reports in SAP Business Objects?**
A: No. The connector schedules reports that already exist in the SAP Business Objects environment. It does not create job definitions in the SAP Business Objects database.

**Q: Which Business Objects report types are supported?**
A: The **START** job type supports **Crystal Reports**, **Publications**, and **Web Intelligence** reports.

**Q: Which job types support Report Attributes, Report Prompts, and Report Destinations?**
A: These TABs are only supported for **Web Intelligence** reports.

**Q: How do I check that a job finished successfully?**
A: Set the Failure Criteria to **NE (Not Equal) to 200**. Codes 200 and 201 indicate that the job has been successfully initiated; code 1 indicates an exception occurred during job initiation.

**Q: Where do connector log files live?**
A: In the ***installation_dir***\\log directory, in a rotating set named `sapbo.log` through `sapbo.log.5`.

## Glossary

> **CMS Authentication** — The authentication mode associated with the SAP Business Objects user. Valid values are **Enterprise**, **LDAP**, **WinAD**, and **SAPR3**.

> **Failure Criteria** — The OpCon job setting that determines what return code(s) cause a job to be considered failed. For SAPBO START jobs, set this to NE 200 to detect a successful initiation.

> **Report ID** — The Business Objects identifier for a report, found by examining the properties of the report on the Business Objects system.

> **SCHEDULE-FORMAT** — A special attribute that overrides the default report output format (`webi`, `pdf`, `xls`, or `csv`).

> **SCHEDULE-TITLE** — A special attribute that sets a free-format title for the scheduled report. If omitted, a title consisting of the report ID and a date timestamp is used.

> **START** — The SAPBO Connector function that schedules a Business Objects report. The connector initiates the task and does not wait for task completion.

## Related topics

- [SAPBO Connector overview](./overview.md)
- [Installation](./installation.md)
- [Release notes](./release-notes.md)
