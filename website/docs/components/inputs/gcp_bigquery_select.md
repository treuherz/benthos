---
title: gcp_bigquery_select
type: input
status: beta
categories: ["Services","GCP"]
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the corresponding source file under internal/impl/<provider>.
-->

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

:::caution BETA
This component is mostly stable but breaking changes could still be made outside of major version releases if a fundamental problem with the component is found.
:::
Executes a `SELECT` query against BigQuery and creates a message for each row received.

Introduced in version 3.63.0.

```yml
# Config fields, showing default values
input:
  label: ""
  gcp_bigquery_select:
    project: "" # No default (required)
    table: bigquery-public-data.samples.shakespeare # No default (required)
    columns: [] # No default (required)
    where: type = ? and created_at > ? # No default (optional)
    job_labels: {}
    priority: ""
    args_mapping: root = [ "article", now().ts_format("2006-01-02") ] # No default (optional)
    prefix: "" # No default (optional)
    suffix: "" # No default (optional)
```

Once the rows from the query are exhausted, this input shuts down, allowing the pipeline to gracefully terminate (or the next input in a [sequence](/docs/components/inputs/sequence) to execute).

## Examples

<Tabs defaultValue="Word counts" values={[
{ label: 'Word counts', value: 'Word counts', },
]}>

<TabItem value="Word counts">


Here we query the public corpus of Shakespeare's works to generate a stream of the top 10 words that are 3 or more characters long:

```yaml
input:
  gcp_bigquery_select:
    project: sample-project
    table: bigquery-public-data.samples.shakespeare
    columns:
      - word
      - sum(word_count) as total_count
    where: length(word) >= ?
    suffix: |
      GROUP BY word
      ORDER BY total_count DESC
      LIMIT 10
    args_mapping: |
      root = [ 3 ]
```

</TabItem>
</Tabs>

## Fields

### `project`

GCP project where the query job will execute.


Type: `string`  

### `table`

Fully-qualified BigQuery table name to query.


Type: `string`  

```yml
# Examples

table: bigquery-public-data.samples.shakespeare
```

### `columns`

A list of columns to query.


Type: `array`  

### `where`

An optional where clause to add. Placeholder arguments are populated with the `args_mapping` field. Placeholders should always be question marks (`?`).


Type: `string`  

```yml
# Examples

where: type = ? and created_at > ?

where: user_id = ?
```

### `job_labels`

A list of labels to add to the query job.


Type: `object`  
Default: `{}`  

### `priority`

The priority with which to schedule the query.


Type: `string`  
Default: `""`  

### `args_mapping`

An optional [Bloblang mapping](/docs/guides/bloblang/about) which should evaluate to an array of values matching in size to the number of placeholder arguments in the field `where`.


Type: `string`  

```yml
# Examples

args_mapping: root = [ "article", now().ts_format("2006-01-02") ]
```

### `prefix`

An optional prefix to prepend to the select query (before SELECT).


Type: `string`  

### `suffix`

An optional suffix to append to the select query.


Type: `string`  


