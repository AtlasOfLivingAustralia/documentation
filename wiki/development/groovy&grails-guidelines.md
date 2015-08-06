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

* In order to host the third party web libraries (eg: jQuery, Angular, ...), it is recommended to store them as they come inside its own folder within a common parent folder (eg: `/web-app/vendor` or `/web-app/3rdparty`), rather than trying to flatten them out and split them in the standard /js, /css,... standard Grails web-app folders.
To illustrate this better, next first image shows a better setup than the second image:

![good resources setup](../../assets/img/web-resources-1-good.png)  ![bad resources setup](../../assets/img/web-resources-2-bad.png)

* To enable that setup the `/vendor` folder has to be added to the resources filter declaration in your `Config.groovy` file:
```groovy
// What URL patterns should be processed by the resources plugin
grails.resources.adhoc.patterns = ['/js/*', '/images/*', '/css/*', '/plugins/*', '/vendor/*']
grails.resources.adhoc.includes = ['/js/**', '/images/**', '/css/**','/plugins/**', '/vendor/**']
```

* It helps to keep the version number of your libraries in the containing folder if they come with it as part of their file name (eg: jquery-2.0.1.js). Or you can just create a folder with the version number you are using. Eg:
```
/vendor/jquery-2.0.1/jquery.js
```
or
```
/vendor/jquery/2.0.1/jquery.js
```

* Default Grails `/web-app/js` and `/web-app/css` folders are better used for resources that have been just developed exclusively for its host application.

* Resources that are declared inside a plugin need to specify the plugin name as part of the URL building process. Eg:
```
modules = {
    ...
    bootstrap {
        dependsOn 'core'
        resource url: [ dir:'js', file: 'bootstrap.min.js', plugin:'ala-bootstrap3']
        resource url: [ dir:'css', file: 'bootstrap.min.css', plugin:'ala-bootstrap3'], attrs:[media:'screen, projection, print']
    }
    ...
}
```


### Other
* Grails JSON null values parsing issue: All versions of Grails until 2.5.1 and 3.0.4 have a problem in the way the `grails.converters.JSON` parses `null` values. You can find the workaround for older versions of Grails [here](TODO)

## Version specific gotchas
* Grails 2.4.4 has a [critical bug] regarding the Hibernate flushing mechanism. It is recommended to move to at least 2.4.5
* Grails 2.5.x do not run in non-forked mode in any of the existing IntellijIDEA releases to date (14.1.4 at the time of writing. It is been fixed for next release. More details [here](https://youtrack.jetbrains.com/issue/IDEA-138537).

## ALA shared plugins
