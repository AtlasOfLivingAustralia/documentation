
# Using Elastic Search with Grails

This guide is for using the Grails Elastic Search plugin with Elastic Search. This is different to previous applications (such as Ecodata), where ES interactions were manually coded, and indexes were updated via GORM entity listeners.

Why is this approach better?

* ES integration is done via configuration on the entities, so you don't need to programmatically update indexes.
* The plugin adds a dynamic ```search``` method to searchable entities, so you don't need to convert between the ES search result structure and your domain model
* The plugin provides an ES search service for more complicated searches
* It's much cleaner

Where can you see an example of how this is done?

* Profiles-service repository in github
  * BuildConfig.groovy (for the dependency mappings - note the exclusions and the lucene 4.10 dependency)
  * config.groovy (for ES plugin configuration)
  * Profile.groovy & Attribute.groovy (for searchable domains)
  * SearchService.groovy (for searches)
* ala-install repository, profiles.yml playbook for installation

## Versions

* elasticsearch-grails-plugin 0.0.4.6 <-> Elastic Search 1.6.x - GOOD
* elasticsearch-grails-plugin 0.0.4.6 <-> Elastic Search 1.7.x - GOOD
* elasticsearch-grails-plugin 0.0.4.6 <-> Elastic Search 2.0.0 - BROKEN

## Prerequisites

* Grails 2.5 (the Grails Elastic Search Plugin doesn't seem to work with 2.4)
  * To get Grails 2.5 to work in IntelliJ 14.1, make sure fork mode is enabled (better to upgrade IntelliJ so you can still debug your code)
* Grails Elastic Search Plugin http://noamt.github.io/elasticsearch-grails-plugin/guide/introduction.html
* External Elastic Search install (external install, not an embedded instance)

## Guide

* Install Elastic Search (https://www.elastic.co/guide/en/elasticsearch/guide/current/_installing_elasticsearch.html)
  * Read this: https://www.elastic.co/guide/en/elasticsearch/guide/current/index.html
* Change the cluster name for ES
  1. [es_install_dir]/conf/elasticsearch.yml
  1. Uncomment ```cluster.name```
  1. Change to something unique
* Install the Grails Elastic Search plugin: ```plugins { runtime ":elasticsearch:0.0.4.6" }```
* Add the Elastic Search configuration items to ```config.groovy```
  ```
elasticSearch {
  // see http://noamt.github.io/elasticsearch-grails-plugin/guide/configuration.html for defaults
  client.hosts = [[host: "localhost", port: 9300]]
  bulkIndexOnStartup = true
  datastoreImpl = "mongoDatastore"
}
environments {
    development {
        elasticSearch {
            client.mode = "transport"
            cluster.name = "[PROD_CLUSTER_NAME]"
        }
    }
    test {
        elasticSearch {
            client.mode = 'local'
            index.store.type = 'memory' // store local node in memory and not on disk
            cluster.name = "[PROD_CLUSTER_NAME]"
        }
    }
    production {
        elasticSearch {
            client.mode = "transport"
            cluster.name = "[PROD_CLUSTER_NAME]"
        }
    }
}
```
* Annotate domain entities that need to be searchable (this is an example taken from profiles-service - see GitHub for the rest of it):
```
static searchable = {
    only = ["scientificName", "fullName", "matchedName", "rank", "primaryImage", "opus"]
    matchedName component: true
    opus component: true
}
```
* If you want to use Kibana, make sure you include a date field (e.g. lastUpdate), otherwise Kibana won't let you create an index pattern.
* Use this guide to work out your mappings: http://noamt.github.io/elasticsearch-grails-plugin/guide/single.html#mapping
* Add a search method. E.g.
```
ElasticSearchService elasticSearchService
def search(String term) {
    elasticSearchService.search(term)
}
```
* Use this guide to help with searching: http://noamt.github.io/elasticsearch-grails-plugin/guide/searching.html


## Troubleshooting

* ```Error creating bean with name 'elasticSearchClient': FactoryBean threw exception on object creation; nested exception is java.lang.NoSuchFieldError: LUCENE_3_6```
  * You have an older version of lucene on your classpath. Elastic Search Plugin required 4.10.
* http://stackoverflow.com/questions/29418419/grails-elastic-search-plugin-severe-unexpected-transforming-call-sites-grails-e  
* ```Could not find matching constructor for: org.elasticsearch.common.transport.InetSocketTransportAddress(java.lang.String, java.lang.String)```
  * Make sure the port in ```client.hosts = [[host: "localhost", port: 9300]]``` is not defined as a String
* ```Caused by: org.elasticsearch.client.transport.NoNodeAvailableException: None of the configured nodes are available: []```
  1. When the client type is ```transport```, you need to have the ``` elasticSearch.cluster.name``` property set
  1. Make sure you are using port 9300 (TCP), not port 9200 (HTTP) in ```client.hosts = [[host: "localhost", port: 9300]]```
  1. Check the ES logs - if an exception is thrown there, you might end up with the same 'None of the configured nodes are available...' error
* ```java.lang.IllegalStateException: Message not fully read (request) for requestId [0], action [cluster/nodes/info], readerIndex [39] vs expected [57]; resetting```
  * If you are using Elastic Search 2.0 with version 0.0.4.6 of the grails plugin....don't. It won't work. Drop down to 1.6.x or 1.7.x.
