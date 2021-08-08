## Description

This is the initialize setting for Elasticsearch (For MDS service) once the cluster is built completely. Includes index default mappings and settings. 

## Steps For Elasticsearch v7.8+

In case elasticsearch does not support **component_template** and **index_template** API, please follow [Alternative: Steps Before Elasticsearch 7.8](https://aylaasia.atlassian.net/wiki/spaces/PI/pages/826802312/Index+Default+Mappings+and+Settings#Alternative%3A-Steps-Below-Elasticsearch-v7.8)

### Step 1: Create saved_at pipeline 

Write saved_at field on document creation.

```
curl -X PUT -H 'content-type: application/json' http://localhost:9200/_ingest/pipeline/saved_at -d '{
  "description": "Adds saved_at timestamp to documents",
  "processors": [
    {
      "set": {
        "field": "_source.saved_at",
        "value": "{{_ingest.timestamp}}"
      },
      "date" : {
        "field" : "_source.saved_at",
        "target_field" : "_source.saved_at",
        "formats" : ["ISO8601"],
        "timezone" : "UTC"
      }
    }
  ]
}'
```

### Step 2: Create component_template for general datapoint event.

```
curl -X PUT -H 'content-type:application/json' 'http://localhost:9200/_component_template/component_template_dp_default_mapping' -d '
{
  "template": {
    "mappings": {
      "properties": {
        "created_at": {
          "type": "date",
          "format": "date_time_no_millis"
        },
        "mds_consumed_at": {
          "type": "date",
          "format": "date_time"
        },
        "direction": {
          "type": "keyword"
        },
        "display_name" : {
          "type" : "keyword"
        },
        "dsn": {
          "type": "keyword"
        },
        "echo": {
          "type": "boolean"
        },
        "generated_at": {
          "type": "long"
        },
        "generated_from": {
          "type": "keyword"
        },
        "id": {
          "type": "keyword"
        },
        "oem_id": {
          "type": "keyword"
        },
        "oem_model": {
          "type": "keyword"
        },
        "property_name": {
          "type": "keyword"
        },
        "scope": {
          "type": "keyword"
        },
        "updated_at": {
          "type": "date",
          "format": "date_time_no_millis"
        }
      }
    }
  }
}'

```

### Step 3: Create component_template for JSON message datapoint payload

```
curl -X PUT -H 'content-type:application/json' 'http://localhost:9200/_component_template/component_template_dp_dynamic_part_mapping' -d '
{
  "template": {
    "mappings": {
      "properties": {
        "value": {
          "type": "nested"
        }
      }
    }
  }
}'
```

### Step 4: Create index_template with patterns, settings and composes.

composed_of : composed of the mappings created above.

template.settings:

- number_of_shards is set to 2 based on amount of data estimation.
- index.default_pipeline defines the pipeline to be executed before document is written without specifying on POST manipulation.

```
curl -X PUT -H 'content-type:application/json' 'http://localhost:9200/_index_template/dp_template_1.0' -d '
{
  "index_patterns": ["dp_*"],
  "template": {
    "settings": {
      "number_of_shards": 2,
      "index.default_pipeline": "saved_at"
    }
  },
  "composed_of": ["component_template_dp_default_mapping","component_template_dp_dynamic_part_mapping"],
  "version": 1,
  "_meta": {
    "description": "dp_ index_template default mappings and settings"
  }
}'
```

 

## Alternative: Steps Below Elasticsearch v7.8

### Step 1: Create saved_at pipeline 

Write saved_at field on document creation.

```
curl -X PUT -H 'content-type: application/json' http://localhost:9200/_ingest/pipeline/saved_at -d '{
  "description": "Adds saved_at timestamp to documents",
  "processors": [
    {
      "set": {
        "field": "_source.saved_at",
        "value": "{{_ingest.timestamp}}"
      },
      "date" : {
        "field" : "_source.saved_at",
        "target_field" : "_source.saved_at",
        "formats" : ["ISO8601"],
        "timezone" : "UTC"
      }
    }
  ]
}'

```

### Step 2: Create template with patterns and settings.

This is an deprecated API and will be removed in the future.

```
curl -X PUT -H 'content-type:application/json' 'http://localhost:9200/_template/dp_template_1.0' -d '
{
  "index_patterns": [ "dp_*" ],
  "settings": {
    "number_of_shards": 2,
    "index.default_pipeline": "saved_at"
  },
  "mappings": {
    "properties": {
      "created_at": {
        "type": "date",
        "format": "date_time_no_millis"
      },
      "mds_consumed_at": {
          "type": "date",
          "format": "date_time"
      },
      "direction": {
        "type": "keyword"
      },
      "display_name" : {
        "type" : "keyword"
      },
      "dsn": {
        "type": "keyword"
      },
      "echo": {
        "type": "boolean"
      },
      "generated_at": {
        "type": "long"
      },
      "generated_from": {
        "type": "keyword"
      },
      "id": {
        "type": "keyword"
      },
      "oem_id": {
        "type": "keyword"
      },
      "oem_model": {
        "type": "keyword"
      },
      "property_name": {
        "type": "keyword"
      },
      "scope": {
        "type": "keyword"
      },
      "updated_at": {
        "type": "date",
        "format": "date_time_no_millis"
      },
      "value": {
        "type": "nested"
      }
    }
  },
  "version": 1
}'
```

## Testing

### Matched index pattern

#### Create one document

```
curl -X POST -H 'content-type:application/json' 'http://localhost:9200/dp_justin_test/_doc/1' -d '{ "dsn":"dsn1"}'
```

#### check mappings

- Output should AT LEAST contains the default mappings created above.
- Should contain saved_at field which is created by default pipeline automatically.

```
curl -X GET -H 'content-type:application/json' 'http://localhost:9200/dp_justin_test/_mapping?pretty'
```

#### check settings

```
curl -H 'content-type: application/json' 'http://localhost:9200/dp_justin_test/_settings?pretty'
```

Output should look like

```
{
  "dp_justin_test" : {
    "settings" : {
      "index" : {
        "number_of_shards" : "2", // <----------- this field should be "2"
        "provided_name" : "dp_justin_test",
        "default_pipeline" : "saved_at", // <---- this field should be "saved_at"
        "creation_date" : "1607414476411", 
        "number_of_replicas" : "1",
        "uuid" : "-LTK965eSkC7XRiXC7XwLQ",
        "version" : {
          "created" : "7090099"saved_at
        }
      }
    }
  }
}
```

### Not matched index pattern

```
curl -X POST -H 'content-type:application/json' 'http://localhost:9200/dp1_justin_test/_doc/1' -d '{ "dsn":"dsn1"}'
curl -X GET -H 'content-type:application/json' 'http://localhost:9200/dp1_justin_test/_mapping?pretty'
curl -H 'content-type: application/json' 'http://localhost:9200/dp1_justin_test/_settings?pretty'
```

Output should different from matched case.

### Clean up after tested

```
curl -X DELETE -H 'content-type:application/json' 'http://localhost:9200/dp_justin_test'
curl -X DELETE -H 'content-type:application/json' 'http://localhost:9200/dp1_justin_test'
```

Then execute below curl command should got error because index already deleted

```
curl -X GET -H 'content-type:application/json' 'http://localhost:9200/dp_justin_test/_mapping?pretty'
curl -X GET -H 'content-type:application/json' 'http://localhost:9200/dp1_justin_test/_mapping?pretty'
```

 

That’s it.

 

 

# Change Log

- 2021/01/11 add display_name