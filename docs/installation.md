---
sidebar_label: 'Installation'
title: SAPBO Connector installation
description: "Steps to install and configure the SAP Business Objects Connector, including required software levels, the Windows Agent prerequisite, the SAPBOPath global property, the Enterprise Manager job subtype plug-in, and Connector.config settings."
tags:
  - Procedural
  - System Administrator
  - SAPBO Connector
---

# Installation

## What is it?

The SAP Business Objects Connector installation consists of multiple steps that are required to complete the installation successfully. The steps install a Windows Agent, the SAP Business Objects Connector, and the SAP Business Objects Windows job subtype, and then configure the connector.

:::note
The SAP Business Objects Connector requires a Windows Agent because it runs as a Windows batch job. You can install the connector on a central server, or you can install an agent on the SAP Business Objects server.
:::

## Supported software levels

The following software levels are required to implement the SAP Business Objects Connector:

- OpCon Release 19.0 or higher
- Embedded Java OpenJDK 11 (part of the installation)
- SAP Business Objects 4.2

## Installation overview

The installation process consists of four steps, performed in order:

1. Install the OpCon Windows Agent.
2. Install the SAP Business Objects Connector.
3. Add the SAP Business Objects Connector job subtype to Enterprise Manager.
4. Configure the SAP Business Objects Connector.

### Step 1 — OpCon Windows Agent installation

The SAP Business Objects Connector requires an SMA OpCon Windows Agent. You can install the Agent on either of the following:

- The SAM Server.
- The same system as the SAP Central Management Console server.

### Step 2 — SAP Business Objects Connector installation

To install the connector, complete the following steps:

1. Copy the supplied install file `SAPBOConnector-win.zip` to the installation directory.
2. Extract the contents into the installation directory.

After extraction, the root installation directory contains the following:

| Item | Purpose |
| ---- | ------- |
| `boxi.exe` | Connector executable |
| `Encrypt.exe` | Encryption utility for passwords stored in `Connector.config` |
| `Connector.config` | Connector configuration file |
| `java\` directory | Embedded Java software (OpenJDK 11) required to run the connector |
| `emplugins\` directory | Job subtype plug-in for Enterprise Manager |

### Step 3 — Create the SAPBOPath global property

Create a global property **SAPBOPath** that contains the full path of the installation directory.

:::tip
The **Connector Path** field on every SAPBO job definition references this property. Setting it once means you do not need to update individual jobs if the install path ever changes.
:::

### Step 4 — Job subtype installation

To install the Enterprise Manager job subtype plug-in, complete the following steps:

1. Copy the Enterprise Manager plug-in from the ***installation_dir***\\emplugins directory to the `dropins` directory of the Enterprise Manager installation.
2. If the `dropins` directory does not exist, create it off the root directory.
3. Restart Enterprise Manager.

A new Windows job subtype called **SAP Business Objects** is now visible.

:::caution
If the new job subtype is not visible, restart Enterprise Manager using **Run as Administrator**. After this, you can use Enterprise Manager normally.
:::

## SAP Business Objects Connector configuration

Configuring the SAP Business Objects Connector requires setting the required values in the `Connector.config` file. The `Connector.config` file contains the following:

- The address of the Business Objects Server that the connector uses to issue requests to Business Objects.
- Definitions for FTP and SMTP servers. The header name of each FTP and SMTP definition is used to extract the correct connection definitions.

You can define multiple FTP or SMTP connections by changing the header value (for example, `[FTP1]` defines the connection for server1 and `[FTP2]` defines the connection for server2).

:::warning
Any passwords entered into `Connector.config` must be encrypted using the `Encrypt.exe` utility provided with the connector. Plain-text passwords are not supported.
:::

### Encrypt utility

The Encrypt utility uses standard 64-bit encryption. It supports a `-v` argument and displays the encrypted value.

To encrypt a value on Windows, run the following command, substituting your value for `abcdefg`:

```text
Encrypt.exe -v abcdefg
```

### Connector.config configuration

Configure the `Connector.config` file in the installation directory by setting the required information. The file is divided into named sections, each introduced by a header in square brackets.

#### [CONNECTOR] section

| Property name | Value |
| ------------- | ----- |
| **CONNECTOR_NAME** | The name of the connector. This value should not be changed. |
| **DEBUG** | If the connector supports a debug mode, you can use this property to set the connector into DEBUG mode. Value either `ON` or `OFF` (default `OFF`). |

#### [BUSINESS OBJECTS] section

| Property name | Value |
| ------------- | ----- |
| **BUSINESS_OBJECT_SERVER_ADDRESS** | Defines the address of the SAPBO server. This includes the port number, which is the listening port defined during SAP Business Objects installation (default `6405`). |
| **BUSINESS_OBJECTS_DEFAULT_REPORT_FORMAT** | The default report format if the attribute `SCHEDULE-FORMAT` is not present. Valid values: `webi`, `pdf`, `xls`, or `csv`. |

#### [DISK] section

Used to define a DISK connection. Contains a valid user and password that is allowed to write files on the target disk.

| Property name | Value |
| ------------- | ----- |
| **DISK_USER** | A valid user code that has the required privileges to write onto the destination disk. |
| **DISK_USER_PASSWORD** | The password of the user. Encrypt this value using `Encrypt.exe`. |

#### [FTP] section

Used to define a connection to an FTP server. An FTP destination definition includes this name as the first parameter of the value definition so the correct FTP server definitions can be used (for example, `FTP=FTP1,..`). You can add multiple definitions by changing the header value.

| Property name | Value |
| ------------- | ----- |
| **FTP_SERVER_NAME** | The address of the FTP server. |
| **FTP_PORT_NUMBER** | The port used by the FTP server. |
| **FTP_USER** | A valid user code that has the required privileges to log in to the FTP server. |
| **FTP_USER_PASSWORD** | The password of the user. Encrypt this value using `Encrypt.exe`. |

#### [SMTP] section

Used to define a connection to an SMTP server. An SMTP destination definition includes this name as the first parameter of the value definition so the correct SMTP server definitions can be used (for example, `SMTP=SMTP1,..`). You can add multiple definitions by changing the header value.

| Property name | Value |
| ------------- | ----- |
| **SMTP_DOMAIN_NAME** | The domain name associated with the SMTP server. |
| **SMTP_SERVER_NAME** | The address of the SMTP server. |
| **SMTP_PORT_NUMBER** | The port used by the SMTP server. |
| **SMTP_USER** | A valid user code that has the required privileges to log in to the SMTP server. |
| **SMTP_USER_PASSWORD** | The password of the user. Encrypt this value using `Encrypt.exe`. |

### Example Connector.config file

```ini
[CONNECTOR]
CONNECTOR_NAME=SAP Business Objects Connector
DEBUG=ON

[BUSINESS OBJECTS]
BUSINESS_OBJECTS_SERVER_ADDRESS= VM-TEST-BOXI:6405
BUSINESS_OBJECTS_DEFAULT_REPORT_FORMAT=pdf

[DISK]
DISK_USER=test
DISK_USER_PASSWORD=6233426a6232353463484d3d

[FTP]
FTP_SERVER_NAME=ftpserver1
FTP_PORT_NUMBER=21
FTP_USER=test
FTP_USER_PASSWORD=6233426a6232353463484d3d

[SMTP]
SMTP_DOMAIN_NAME=domain
SMTP_SERVER_NAME=smtpserver1
SMTP_PORT_NUMBER=25
SMTP_USER=test
SMTP_USER_PASSWORD=6233426a6232353463484d3d
```

## FAQs

**Q: Where can I install the SAP Business Objects Connector?**
A: You can install it on a central server or on the SAP Business Objects server. The connector requires a Windows Agent on the same system because it runs as a Windows batch job.

**Q: How do passwords get encrypted in `Connector.config`?**
A: All passwords stored in `Connector.config` must be encrypted using the `Encrypt.exe` utility provided with the connector. Run `Encrypt.exe -v <value>` and place the resulting encrypted value in the configuration file.

**Q: Why is the SAP Business Objects job subtype not visible in Enterprise Manager after I copy the plug-in?**
A: Restart Enterprise Manager. If it is still not visible, restart Enterprise Manager using **Run as Administrator**.

**Q: What does the SAPBOPath global property do?**
A: It contains the full path to the connector installation directory and is referenced by job definitions through the **Connector Path** field.

## Glossary

> **Connector.config** — The configuration file in the connector installation directory that defines the Business Objects Server address, FTP and SMTP server connections, and connector-level settings such as DEBUG mode.

> **dropins directory** — The Enterprise Manager subdirectory where the SAP Business Objects job subtype plug-in is placed so Enterprise Manager loads it on startup.

> **Encrypt.exe** — The utility supplied with the connector that produces a 64-bit encrypted value for any password stored in `Connector.config`.

> **SAPBOPath** — The global property that holds the full path of the SAP Business Objects Connector installation directory.

## Related topics

- [SAPBO Connector overview](./overview.md)
- [Operation](./operation.md)
- [Release notes](./release-notes.md)
