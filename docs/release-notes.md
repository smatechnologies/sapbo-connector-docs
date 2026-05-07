---
sidebar_label: 'Release notes'
title: SAPBO Connector release notes
description: "Version history and change details for the SAP Business Objects Connector, including new features, improvements, and bug fixes."
tags:
  - Reference
  - System Administrator
  - SAPBO Connector
---

# SAPBO Connector release notes

## 21

### 21.0.1

#### General

The release removes log4j and replaces it with slf4j and logback.

#### Migration considerations

This release includes the new format installer where the files are extracted from the zip file into the desired directory. It contains an embedded Java version for the connector, so there is no reliance on installed Java versions. The connector also implements new encryption capabilities, and the documentation explains how to use the new Encrypt.exe mechanism. The configuration file name has also been changed from Agent.config to Connector.config.

#### Fixes

:white_check_mark: **CONNUTIL-543**: CVE-2021-44228 adjustment removing log4j as the logging component.

:white_check_mark: **CONNUTIL-558**: Added libraries to implement missing jaxb classes moved during Java 11 implementation.
