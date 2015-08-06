# Groovy and Grails guidelines

This guideline is meant to be a compilation of best practices, tips and gotchas.

## Best Practices

### General
Let's put [this guide](https://tedvinke.wordpress.com/2015/03/15/basic-groovy-and-grails-code-review-guidelines/) as the baseline :-) 

### Logging

### Web static resources (Resource Plugin) 
* For Grails version 2.3+ (not 3.x), these are the recommended Resources plugin dependencies:
```groovy
    runtime ':resources:1.2.14'
    if (Environment.current == Environment.PRODUCTION) {
        runtime ":zipped-resources:1.0.1"
        runtime ":cached-resources:1.1"
        compile ":cache-headers:1.1.7"
        runtime ":yui-minify-resources:0.1.5"
    }
```

## Version specific gotchas
* Grails 2.4.4 has a [critical bug] regarding the Hibernate flushing mechanism. It is recommended to move to at least 2.4.5 

## Other
* Grails JSON null values parsing issue: All versions of Grails until 2.5.1 and 3.0.4 have a problem in the way the `grails.converters.JSON` parses `null` values. You can find the workaround for older version of Grails [here](TODO) 

