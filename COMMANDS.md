* elasticsearch basics
* index documents

* size, from, sort
* query
  https://www.elastic.co/guide/en/elasticsearch/reference/current/term-level-queries.html
  * range
  * exists
  * term
  * terms   
    * boost
  https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html
  * bool
    * filter
    * must
    * must_not
    * should
    * minimum_should_match
  https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html
  * match (variations)
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html
* aggs
  * metrics: avg, max, min, sum, cardinality, top hits
  * buckets: date histogram, date range, histogram, range, filter, terms 
* analyzers 
  * mappings https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html
  * brazilian https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-lang-analyzer.html#brazilian-analyzer
