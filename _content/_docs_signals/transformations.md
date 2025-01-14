---
title: Transformations
html_title: Creating Transformations and Calculations for Signals Alerting
slug: elasticsearch-alerting-transformations-calculations
category: signals
subcategory: transformations
order: 500
layout: docs
edition: preview
description: 
---

<!--- Copyright 2019 floragunn GmbH -->

# Transformations and Calculations
{: .no_toc}

{% include toc.md %}

In many cases, you will want to run transformations and calculations on the raw data pulled in by [inputs](inputs.md).

For example, you may want to calculate an average value over some fields, or clean up data before [storing it back to Elasticsearch](actions_index.md).

You can use painless scripts to either

* transform your data in any execution context
* calculate values and store them in a new execution context

Both transformations and calculations are implemented as inline or stored painless scripts that operate on the execution runtime data. You can use the full power of painless to massage the data any way you want.