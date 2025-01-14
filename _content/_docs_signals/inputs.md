---
title: Inputs
html_title: Creating inputs for Signals Alerting
slug: elasticsearch-alerting-inputs
category: signals
subcategory: inputs
order: 400
layout: docs
edition: preview
description: 
---

<!--- Copyright 2019 floragunn GmbH -->

# Inputs
{: .no_toc}

Each watch can have one or more inputs. Inputs can be freely defined anywhere in the execution chain.

Each input will fetch data from a data source, and place it in the runtime data context under a configurable key for later usage.

At the moment, Signals supports the following input types:

* [Constants](inputs_static.md)
  * Define constants you can then use at multiple places in the execution chain
* [Elasticsearch](inputs_elasticsearch.md)
  * Use the full power of Elasticsearch queries and aggregations
* [HTTP](inputs_http.md)
  * Pull in data from a REST endpoint

All data from all inputs can be combined by using [Transformation](transformations_transformations.md) and [Calculations](transformations_calculations.md), used in [Conditions](conditions.md) and pushed to [action endpoints](actions.md).
 
