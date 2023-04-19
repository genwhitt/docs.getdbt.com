---
title: Why does dbt compile need a data platform connection?
description: "`dbt compile` needs a data platform connection to perform introspective queries and generate the compiled SQL"
sidebar_label: "Why does dbt compile need a data platform connection?"
id: db-connection-dbt-compile
---

The [`dbt compile`](reference/commands/compile) command is a really important step in the dbt workflow. This is because it transforms the raw SQL in dbt models and tests into compiled SQL, which is optimized for running on a specific data platform. The compiled SQL includes SQL queries that are generated by dbt, such as query parts for incremental models, `ref` macros, and other [dbt-specific syntax](reference/node-selection/syntax).

To generate this compiled SQL, dbt needs to run introspective queries against the data platform to understand its schema and available resources. These introspective queries include populating the [relation cache](/guides/advanced/creating-new-materializations#update-the-relation-cache), resolving [macros](/docs/build/jinja-macros#macros), and checking if models are [incremental](/docs/build/incremental-models). Without a database connection, dbt cannot perform these introspective queries and will not be able to generate the compiled SQL needed for the next steps in the dbt workflow.

In addition, in a future where materializations can report more of what they're going to do before they do it, a compile task will still require a data platform connection. Although, if you only want to parse your project and write out [`manifest.json`](/reference/artifacts/manifest-json), that can be done with the [`dbt ls` command](/reference/commands/list), without a data platform connection. However, something to note is that the written-out manifest won't include compiled SQL.

In short, `dbt compile` needs a data platform connection to perform introspective queries and generate the compiled SQL needed for the next steps in the dbt workflow. Without a data platform connection, dbt cannot compile your SQL and will not be able to perform transformations on your data.
