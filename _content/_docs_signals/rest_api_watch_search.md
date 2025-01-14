---
title: Search watch
html_title: Searching a watch with the REST API
slug: elasticsearch-alerting-rest-api-watch-search
category: signals-rest
order: 150
layout: docs
edition: preview
description: 
---

<!--- Copyright 2019 floragunn GmbH -->

# Search Watch API
{: .no_toc}

{% include toc.md %}


## Endpoint

```
GET /_signals/watch/_search
```

```
POST /_signals/watch/_search
```

Searches for watches. Search criteria and options can be specified in a manner similar to the Elasticsearch document search REST API.

Both the GET and the POST HTTP method can be used with the same effect.

## Query Parameters

**from:** Specifies an offset into the result list. Starting from this offset, the results will be returned. Optional.

**size:** Specifies the maximum number of watches to be returned. Optional, defaults to 10. If scrolling is enabled, this specifies the maximum number of watches to be returned in one scroll iteration. 

**scroll:** Configures the search to be scrollable. Optional. The value of this parameter must be a time duration value (like `1m` for one minute) during which the scroll will be available. If scrolling is enabled, the response will contain a `_scroll_id`. The value of this attribute must be passed to the standard ES REST endpoint `_search/scroll` to continue scrolling.



## Request Body

The request body specifies search options like the ElasticSearch document search REST API.

If no request body is specified, all watches configured for the currently selected tenant will be returned; limited to the amount specified to the `size` query parameter.

Important attributes of the request body are:

**query:** An ES document query.

**sort:** Specifies the attributes by which the result shall be sorted. 	


## Responses

### 200 OK

The search was successfully executed.

### 403 Forbidden

The user does not have the required to access the endpoint for the selected tenant.

## Multi Tenancy

The watch REST API is tenant-aware. Each Signals tenant has its own separate set of watches. The HTTP request header `sg_tenant` can be used to specify the tenant to be used. If the header is absent, the default tenant is used.

## Permissions

For being able to access the endpoint, the user needs to have the privilege `cluster:admin:searchguard:tenant:signals:watch/search` for the currently selected tenant.

This permission is included in the following [built-in action groups](security_permissions.md):

* SGS\_SIGNALS\_ALL 
* SGS\_SIGNALS\_WATCH\_MANAGE
* SGS\_SIGNALS\_WATCH\_READ

## Examples

### List all watches

```
GET /_signals/watch/_search?size=1000
```

**Response**

```
200 OK
```

```json
{
    "took": 3,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 0.13353139,
        "hits": [
            {
                "_index": ".signals_main_watches",
                "_type": "_doc",
                "_id": "bad_weather",
                "_score": 0.13353139,
                "_source": {
                    "checks": [
                        {
                            "request": {
                                "indices": [
                                    "kibana_sample_data_flights"
                                ],
                                "body": {
                                    "query": {
                                        "bool": {
                                            "must": [
                                                {
                                                    "query_string": {
                                                        "default_field": "DestWeather",
                                                        "query": "*hunder* OR *ightning*"
                                                    }
                                                },
                                                {
                                                    "range": {
                                                        "timestamp": {
                                                            "gte": "now-4h",
                                                            "lte": "now"
                                                        }
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                }
                            },
                            "name": "bad_weather_flights",
                            "type": "search",
                            "target": "bad_weather_flights"
                        }
                    ],
                    "active": true,
                    "_meta": {
                        "last_edit": {
                            "date": "2019-09-02T09:28:18.566Z",
                            "user": "admin"
                        }
                    },
                    "trigger": {
                        "schedule": {
                            "cron": [
                                "* */1 * * * ?"
                            ],
                            "timezone": "Europe/Berlin"
                        }
                    },
                    "log_runtime_data": false,
                    "actions": [
                        {
                            "throttle_period": "1h",
                            "subject": "Bad destination weather for {{data.bad_weather_flights.hits.total.value}} flights over last {{data.constants.window}}!",
                            "name": "email",
                            "from": "x@x.com",
                            "to": [
                                "notify@example.com"
                            ],
                            "text_body": "Time: {{_source.timestamp}}\n  Flight Number: {{_source.FlightNum}}\n  Origin: {{_source.OriginAirportID}}\n  Destination: {{_source.DestAirportID}}",
                            "type": "email",
                            "account": "default_mail"
                        }
                    ]
                }
            }
        ]
    }
}
```

### Search for all watches using an email action

```
POST /_signals/watch/_search?size=1000
```

```
{
    "query": {
        "match": {
            "actions.type": "email"
        }
    }
}
```

**Response**

```
200 OK
```

```json
{
    "took": 3,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 0.13353139,
        "hits": [
            {
                "_index": ".signals_main_watches",
                "_type": "_doc",
                "_id": "bad_weather",
                "_score": 0.13353139,
                "_source": {
                    "checks": [
                        {
                            "request": {
                                "indices": [
                                    "kibana_sample_data_flights"
                                ],
                                "body": {
                                    "query": {
                                        "bool": {
                                            "must": [
                                                {
                                                    "query_string": {
                                                        "default_field": "DestWeather",
                                                        "query": "*hunder* OR *ightning*"
                                                    }
                                                },
                                                {
                                                    "range": {
                                                        "timestamp": {
                                                            "gte": "now-4h",
                                                            "lte": "now"
                                                        }
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                }
                            },
                            "name": "bad_weather_flights",
                            "type": "search",
                            "target": "bad_weather_flights"
                        }
                    ],
                    "active": true,
                    "_meta": {
                        "last_edit": {
                            "date": "2019-09-02T09:28:18.566Z",
                            "user": "admin"
                        }
                    },
                    "trigger": {
                        "schedule": {
                            "cron": [
                                "* */1 * * * ?"
                            ],
                            "timezone": "Europe/Berlin"
                        }
                    },
                    "log_runtime_data": false,
                    "actions": [
                        {
                            "throttle_period": "1h",
                            "subject": "Bad destination weather for {{data.bad_weather_flights.hits.total.value}} flights over last {{data.constants.window}}!",
                            "name": "email",
                            "from": "x@x.com",
                            "to": [
                                "notify@example.com"
                            ],
                            "text_body": "Time: {{_source.timestamp}}\n  Flight Number: {{_source.FlightNum}}\n  Origin: {{_source.OriginAirportID}}\n  Destination: {{_source.DestAirportID}}",
                            "type": "email",
                            "account": "default_mail"
                        }
                    ]
                }
            }
        ]
    }
}
```
