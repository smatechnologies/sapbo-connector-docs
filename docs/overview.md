---
sidebar_label: 'SAPBO Connector'
title: SAP Business Objects Connector
description: "Conceptual overview of the SMA OpCon SAP Business Objects Connector, including the Business Objects suite components it integrates with and the job types it supports."
tags:
  - Conceptual
  - Automation Engineer
  - SAPBO Connector
---

# SAP Business Objects Connector

## What is it?

The SMA OpCon SAP Business Objects Connector interacts with the Business Objects scheduler, allowing reports (Web Intelligence, Crystal Reports, and Publications) to be scheduled by OpCon. SAP Business Objects is a suite of front-end applications that allows business users to view, sort, and analyze business intelligence data.

The suite contains the following applications:

- **Crystal Reports** — Allows users to design and generate reports.
- **Dashboards** — Allows users to create interactive dashboards that contain charts and graphs for visualizing data.
- **Web Intelligence** — Provides a self-service for creating ad hoc queries and analysis of data both online and offline.
- **Publication** — Consists of a collection of Web Intelligence documents to be run as a single task.

You can answer prompts using definitions within the OpCon job definition. This allows a single report defined within Business Objects to be used multiple times from OpCon using different parameters.

When used in conjunction with OpCon Self Service, the connector allows users to start report tasks from the web by selecting answers to prompts from drop-down lists.

![SAP Business Objects Overview](/img/sapbo-connector-overview.png)

## Supported job types

The SAP Business Objects Connector supports a START job type that can be used to communicate with the SAP Business Objects batch environment.

- **START** — Schedules a report defined within the SAP Business Objects environment. The connector initiates the task and does not wait for task completion.

The job definitions are passed to the SAP Business Objects Connector as arguments. The job definition information received from OpCon is then mapped to the appropriate structures and the Business Objects Server is called. Each job type requires a valid user and password, which the connector uses to establish the connection to SAP Business Objects.

## Glossary

> **Business Objects scheduler** — The SAP Business Objects component that runs reports defined within the Business Objects environment.

> **Crystal Reports** — A Business Objects application for designing and generating reports.

> **Publication** — A collection of Web Intelligence documents that can be run together as a single task.

> **Web Intelligence** — A Business Objects application for creating ad hoc queries and analyzing data online or offline.

> **START job** — The job type the SAPBO Connector supports for scheduling a Business Objects report from OpCon.
