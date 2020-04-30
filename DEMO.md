POST /users/_doc/1
{
  "name" : "Jose  Barradas",
  "age" : 30
}

POST /users/_doc/2
{
  "name" : "Thiago Barradas",
  "age" : 29
}

POST /users/_doc/2
{
  "name" : "Thiago Barradas",
  "age" : 15
}

GET /users/_doc/1
GET /users/_doc/2

DELETE /users/_doc/<id>

GET /users/_search

GET /users/_mapping

PUT /users2
{
  "settings": {
    "number_of_shards": 5,
    "number_of_replicas": 0
  }, 
  "mappings": {
    "properties": {
      "status": {
        "type": "keyword"
      },
      "age": {
        "type": "long"
      },
      "name": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      }
    }
  }
}

GET /_cat/shards?v
GET /_cat/indices?v

GET /hits*/_search

items: 2
pagina: 3
from = (pagina - 1) * items

GET /hits-2020.04.29/_mapping

POST /hits*/_search
{
  "size" : 2,
  "from" : 0,
  "sort": [
    {
      "Year.keyword": {
        "order": "desc"
      }
    }
  ]
}

https://www.elastic.co/guide/en/elasticsearch/reference/current/term-level-queries.html

POST /hits*/_search
{
  "size" : 2,
  "from" : 0,
  "query" : {
    "range" : {
      "Number" : {
        "gt" : 2,
        "lt" : 5
      }
    } 
  }
}

POST /hits*/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "Number": {
              "gte": 1,
              "lte": 200
            }
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "now-24h",
              "lte": "now"
            }
          }
        },
        {
          "term": {
            "Year.keyword": "1965"
          }
        }
      ]
    }
  }
}

POST /hits*/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "Number": {
              "gte": 1,
              "lte": 500
            }
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "now-24h",
              "lte": "now"
            }
          }
        }
      ],
      "must" : [
        {
          "match" : {
            "Album" : {
                "query" : "Highwyay",
                "fuzziness": 1, 
                "minimum_should_match": 1
            }
          }
        }
      ]
    }
  }
}


POST /hits*/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "Number": {
              "gte": 1,
              "lte": 500
            }
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "now-24h",
              "lte": "now"
            }
          }
        }
      ],
      "must" : [
        {
          "match" : {
            "Album" : {
                "query" : "wahtever",
                "fuzziness": 1, 
                "minimum_should_match": 1
            }
          }
        }
      ],
      "must_not" : [
        {
          "match" : {
            "Album" : {
                "query" : "Highwyay",
                "fuzziness": 1, 
                "minimum_should_match": 1
            }
          }
        }
      ]
    }
  }
}

POST /hits*/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "Number": {
              "gte": 1,
              "lte": 500
            }
          }
        },
        {
          "range": {
            "@timestamp": {
              "gte": "now-24h",
              "lte": "now"
            }
          }
        }
      ],
      "must": [
        {
          "match": {
            "Artist": {
              "query": "beatles",
              "fuzziness": 1,
              "minimum_should_match": 1
            }
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "Artist": {
              "query": "calipso",
              "fuzziness": 1,
              "minimum_should_match": 1
            }
          }
        }
      ],
      "should": [
        {
          "match": {
            "Subgenre": "beat"
          }
        },
        {
          "match": {
            "Subgenre": "Psychedelic"
          }
        }
      ],
      "minimum_should_match": 0
    }
  }
}

DELETE /dio-experts

PUT /dio-experts
{
  "aliases": {},
  "mappings": {
    "properties": {
      "content": {
        "analyzer": "rebuilt_brazilian",
        "type": "text"
      }
    }
  },
  "settings": {
    "analysis": {
      "filter": {
        "brazilian_stemmer": {
          "type": "stemmer",
          "language": "brazilian"
        },
        "brazilian_keywords": {
          "keywords": [
            "exemplo"
          ],
          "type": "keyword_marker"
        },
        "brazilian_stop": {
          "type": "stop",
          "stopwords": "_brazilian_"
        }
      },
      "analyzer": {
        "rebuilt_brazilian": {
          "filter": [
            "lowercase",
            "brazilian_stop",
            "brazilian_keywords",
            "brazilian_stemmer"
          ],
          "tokenizer": "standard"
        }
      }
    }
  }
}


POST /dio-experts/_doc/1
{
  "content" : "Aprenda a desenvolver uma API de gerenciamento de super heróis utilizando Spring WebFlux ,utilizada por empresas como Netflix e Pivotal, junto com a library reativa Reactor que atualmente é mantida pela VmWare. Além disso, usaremos o banco DynamoDb localmente para armazenar nossos dados e demonstrarei como realizar testes unitários da sua API com Junit e como gerar documentações simples por meio do Postman e também do Swagger."
}

GET /dio-experts/_analyze
{
  "analyzer" : "rebuilt_brazilian",
  "text" : "eu prefiro cerveja ao ter que beber agua"
}

POST /dio-experts/_search
{
  "query": {
    "match": {
      "content": {
        "query": "aperndendo spring",
        "fuzziness": 1, 
        "operator": "and"
      }
    }
  }
}


POST /dio-experts/_search
{
  "query": {
    "match": {
      "content": {
        "query": "aperndendo spring",
        "fuzziness": 1, 
        "operator": "and"
      }
    }
  }
}

POST /teste-aggs/_doc
{
  "date" : "2020-04-10",
  "numero" : 17,
  "status" : "active"
}

POST /teste-aggs/_search
{
  "size": 2,
  "from": 0,
  "aggs": {
    "perStatus": {
      "terms": {
        "field": "status.keyword",
        "size": 10
      }
    },
    "perDay": {
      "date_histogram": {
        "field": "date",
        "fixed_interval": "1d"
      },
      "aggs": {
        "perStatus": {
          "terms": {
            "field": "status.keyword",
            "size": 10
          }
        },
        "avgnumero" : {
          "avg": {
            "field": "numero"
          }
        },
        "maxnumero" : {
          "max": {
            "field": "numero"
          }
        },
        "minnumero" : {
          "min": {
            "field": "numero"
          }
        }
      }
    }
  }
}