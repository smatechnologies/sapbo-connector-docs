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

:::note

The connector has received maintenance since 21.0.1, including an update to the embedded Java runtime and changes to the build and code-signing pipeline. These changes are not individually versioned in this release history. Check with your support contact to confirm the current release for your environment.

:::

## 21

### 21.0.1

**Released:** 2022 February

#### Fixes

- **CONNUTIL-558**: Added libraries to implement missing JAXB classes that moved during the Java 11 implementation.

### 21.0.0

**Released:** 2022 January

#### Fixes

- **CONNUTIL-543**: Removed log4j as the logging component and replaced it with slf4j and logback, addressing CVE-2021-44228.

## 19

### 19.1.1

**Released:** 2020 September

#### What's new

- **New installer format.** The connector files are extracted from a zip file into the directory of your choice rather than being placed by an installer.
- **Embedded Java runtime.** The connector ships with its own Java version, so it no longer depends on the Java installed on the host.
- **Password encoding.** Use the `Encrypt.exe` utility to encode passwords for the configuration file. See [Installation](./installation.md) for what the utility does and does not protect.

#### Migration considerations

- Rename your existing `Agent.config` to `Connector.config`, or recreate it under the new name. The connector does not read the old name.
- Extract the package into the installation directory of your choice. There is no installer to run.
- Re-encode any passwords carried over from an earlier release, and remove any dependency this connector had on a host-installed Java version.
