<br><b><a href="http://www.valtech.com" target="_blank" style="font-size:36px; color:#FFFFFF; text-decoration: none;">valtech_</a></b><br>

Allegion US
========
Welcome to the Allegion documentation. In this documentation you can find specific information on how to integrate
HTML, CSS and JavaScript elements into the system. You can also find information and tutorials on how to extend or create new
templates and or components using the current system.


In this document **you will not find**  information about the libraries used in the site.
Please refer to the [Technology](#technology) section below. 

Frontend
-------
There is a direct binding between the data-component-name="name" attribute in the HTML and the al.views.name in the JavaScript file. This technique is
used to ensure only the JavaScript of the components included in the page are executed.
It is possible to create N amount of components that share the same data-component-name attribute name. The Loader.js file will make sure
to initialize all of them and assign the elements that contains the data-component-name attribute as the BackBone view DOM root element (this.$el).
It is possible to have nested DOM element with the same data-component-name attribute name but is not recommended. Please note that to be able
to use the data binding you need to add the "al-component" class on the DOM element.


Below is the code snippet of the areas that get involved during the initialization of a component view. You may use this code to create a new component.
####JavaScript####
```
// Used by jshint
/* global al, bbEvent, Backbone, Hammer, config, jQuery */

(function ($, win, doc) {
    'use strict';

    /**
     * @exports  al.views.navigation
     */
    al.views.navigation = Backbone.View.extend({

        /**
         * Called upon initialization. Please refer to <a target="_blank" href="http://backbonejs.org/">backbone</a>
         * documentation
         */
        initialize: function () {
            // Code start here
        }
    });
})(jQuery, window, document);

```
####HTML####
```
  <div class="al-component" data-component-name="navigation">
      <!-- Code Goes Here -->
  </div>
```

####LESS####
```
 /* imports here */
 @import "global.config.less";
 @import "global.color.less";

 .navigation {
    // All navigation less/css goes here
 }
```

####Loader.js####
```
 /**
  * Scrapes DOM for matching class attributes & saves context
  * while instantiating new isntance of designated component
  */
function loadModules() {
  // Module loader
  $('.al-component').each(function (index, value) {
     var $this = $(this);

     // Get component name
     var componentName = $this.data('component-name');

     // Exit if no component name is defined (i.e., no javascript is required for this component)
     if (typeof componentName === 'undefined') {
        return;
     }

     // Bail if there aren't any components
     if (typeof window.AL.components === 'undefined') {
        return;
     }

     // Instantiate new component
     views.push(new window.AL.components[componentName]({
        'el': $this
     }));
  });
}
``` 

Backend
-------
In general each component has its own model which is responsible for mapping sling properties to the fields of java model.
Java models are responsible for all backend operations like obtaining additional models and computing values based on
the properties obtained from resource.


####Component code####
``` jsp
 <%--includes--%>
 
 <%-- obtaining a model for a resource --%>
 <sling:adaptTo adaptable="${resource}"<br>
                adaptTo="com.allegion.us.components.content.example.ExampleModel" var="model" />
 
 <div class="example">
    <%-- Using property in jsp --%>
    ${model.property}
 </div>
```

####Java model####
``` java
/*imports*/
@Model(adaptables=Resource.class, defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
public class ExampleModel {

    @Inject
    private String property;

    public String getProperty() {
        return property;
    }
}
```

<div id="technology">Technology used for this project</div>
-------

Frontend dependencies
-------
Below all the tools used to generate and create the FE code for Allegion US Experience Guides:
<ul>
<li>
  <a href="http://jquery.com/" target"_blank">jQuery</a><br>
  jQuery is a fast, small, and feature-rich JavaScript library. It makes things like HTML document traversal and manipulation,
  event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers.
  With a combination of versatility and extensibility, jQuery has changed the way that millions of people write JavaScript.
</li>
<br>
<li>
  <a href="http://underscorejs.org/">Undescorejs</a><br>
  Underscore is a JavaScript library that provides a whole mess of useful functional programming helpers without extending any built-in objects.
</li>
<br>
<li>
  <a href="http://backbonejs.org">BackBonejs</a><br>
  Backbone.js gives structure to web applications by providing models with key-value binding and custom events, collections with a
  rich API of enumerable functions, views with declarative event handling, and connects it all to your existing API
  over a RESTful JSON interface.
</li>
<br>
<li>
  <a href="http://owlgraphic.com/owlcarousel/">owl.carousel.js</a> (Part of it)<br>
  **(NOTE: This tool has been modified to fit with the current architecture of the project)**<br>
  Touch enabled jQuery plugin that lets you create beautiful responsive carousel slider.

</li>
<br>
<li>
  <a href="http://jquery.com/">velocityjs</a><br>
  Velocity is an animation engine with the same API as jQuery's $.animate().
</li>
<br>
<li>
  <a href="http://lesscss.org/">less</a><br>
  Less is a CSS pre-processor, meaning that it extends the CSS language, adding features that allow variables, mixins,
  functions and many other techniques that allow you to make CSS that is more maintainable, themable and extendable.
</li>
<br>
<li>
  <a href="http://usejsdoc.org/about-getting-started.html">jsDoc</a> (for this documentation)<br>
  JSDoc 3 is an API documentation generator for JavaScript, similar to JavaDoc or PHPDoc. You add documentation
  comments directly to your source code, right along side the code itself. The JSDoc Tool will scan your source code,
  and generate a complete HTML documentation website for you.
</li>
</ul>

Backend dependencies
-------
Below all the external dependencies used by BE code
<ul>
<li>
  <a href="https://sling.apache.org/documentation/bundles/models.html" target"_blank">Sling Models</a><br>
  Sling models is a framework allowing using model objects automatically mapped from Sling objects instead of using 
  properties maps provided by sling. It facilitates process of separating component presentation layer from the backend logic.<br>
 
</li>
<br>
<li>
  <a href="https://github.com/Cognifide/CQ-Actions" target"_blank">CQ-Actions</a><br>
  CQ-Actions provides a transport layer to communicate from publish to author in easy way.<br>
</li>
</ul>
This project uses following backend dependencies defined in parent pom:

Sling Models
``` xml
<dependency>
    <groupId>org.apache.sling</groupId>
    <artifactId>org.apache.sling.models.api</artifactId>
    <version>1.1.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>org.apache.sling</groupId>
    <artifactId>org.apache.sling.models.impl</artifactId>
    <version>1.1.0</version>
    <scope>provided</scope>
</dependency>
```

CQ-Actions
``` xml
<dependency>
    <groupId>com.cognifide.cq.actions</groupId>
    <artifactId>com.cognifide.cq.actions.api</artifactId>
    <version>3.0.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>com.cognifide.cq.actions</groupId>
    <artifactId>com.cognifide.cq.actions.core</artifactId>
    <version>3.0.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>com.cognifide.cq.actions</groupId>
    <artifactId>com.cognifide.cq.actions.msg.replication</artifactId>
    <version>3.0.0</version>
    <scope>provided</scope>
</dependency>
```
