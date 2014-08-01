grails-plugin-to-uml
=============================

Generate UML diagrams from your Grails app source code.
   
## Features

  1. [ *DONE* ] Global Class diagrams for Domain  (see screenshot section below)
  1. [ *DONE* ] Diagram generation using online [PlantUML server](http://www.plantuml.com/plantuml) (ie no need to rely on plantUML.jar - no local image generation) 
  1. [ *DONE* ] Diagrams exposed as specific UmlController (http interface)
  1. [ *DONE* ] Global Class diagrams (layered) for Controllers & Services & other beans (only public methods, no javadoc comments)
  1. [ *DONE* ] Diagram generation in PNG via plantUML.jar from [PlantUML project](http://plantuml.sourceforge.net/)
  1. [ ONGOING ] Configuration of the data exposed (through an html page)
  1. [TODO] Diagrams exposed as a grails script: "grails to-uml" (cli interface)
  1. [TODO] Inclusion in standard gdoc process
  1. [TODO] Diagram generation using online [yUML](http://www.yuml.me/diagram/scruffy/class/draw) (different syntax)
  1. ~~[TODO] Document specifically webapp interfaces (public methods of Controllers, with javadoc, input/output spec?)~~ (currently out of the scope of this UML plugin)
  
## Finished tasks

* (v0.2.0) Refactor (pass1) script / controller / service (UmlController | UmlService | PlantUmlService |YumlService)
* (v0.2.0) domain introspection result to diagram spec
* (v0.2.0) domain >> all fields not external (ie excluding hasmany)
* (v0.2.1) domain >> list dependencies (hasmany)
* (v0.2.3) Config options (fieldFilterRegexps classFilterRegexps showCanonicalJavaClassNames diagramType)
* (v0.2.4) grails introspect 
  * controllers >> list dependencies
  * services >> list dependencies
* (v0.2.4) (as option = config parameter) filter fields via regexp (eg: exclude id and version)
* (v0.2.4) (as option = config parameter) no language package names: java.lang, java.util
* (v0.2.5) (as option = config parameter) no internal classes from grails framework
* (v0.2.6) plantUml diagram spec to PNG byteStream
  
## Ongoing tasks  

* Config options (showCanonicalClassNames duplicateFieldAndArrow showGrailsInternals)
* Refactor UmlService (too much duplication) ==> Domain vs Controller/Service should not be separate methods

## Future tasks

* ~~Correct UmlService (Graph representation) ==> in the classData, the associations field is a duplicate of the properties field~~
* ~~grails introspect controllers, services >> public methods~~ (currently out of the scope of this UML plugin)
* ~~plantUml Class diagram spec (non trivial example)~~
* ~~plantUml Dependency diagram spec (non trivial example)~~
* GSP View exposing the Config Command object
* (as option = config parameter) no duplication: if isAssociation, don't list in properties
* Refactor (pass2) UmlController | UmlService | PlantUmlService |YumlService
* Yuml as secondary option
* Documentation of the plugin (specifically: ConfigurationCommand, controller mode, script mode, renderers)
* plantUml diagram spec to PNG file (script mode)
* gdoc inclusion of the script
* script : controller/services/domains >> to PNG files in target output folder, see http://grails.org/doc/2.2.x/ref/Command%20Line/bootstrap.html
```
    includeTargets << grailsScript("_GrailsBootstrap")
    loadApp()
    for (grailsClass in grailsApp.allClasses) { println grailsClass }

    configureApp()
    Connection c = appCtx.getBean('dataSource').getConnection()

    PS1: may want to investigate http://grails.org/plugin/grails-runtime-docs
    PS2: same for this http://gr8labs.org/getting-groovy/ 
```  

## Inspiration from 

* http://grails.org/plugin/class-domain-uml
* http://www.grails.org/plugin/create-domain-uml
* https://github.com/trygvea/grails-class-diagram/
* https://github.com/david-w-millar/grails-plantuml-plugin
* https://code.google.com/p/grails-domain-uml/source/browse/#svn%2FCreateDomainUml
## Installation

[TODO]
Add `runtime ":to-uml:0.2.6"` to **BuildConfig.groovy** in plugins section (requires grails 2.0 > *).

## Development

* Create a web-app: `grails create-app web` 
* Add in BuildConfig of this new projet (adjust path as needed): 
```
grails.project.fork = [
  test: false , 
  run: false , 
  war: false , 
  console: false , 
]
grails.reload.enabled = true
grails.plugin.location.'to-uml'="../grails-plugin-class-domain-uml"`
```

Apart from the last line, we're basically turning off grails 2.3 forking process, which hampers auto-reload. 

You may also need to add _grails.reload.enabled = true_ to ../grails-plugin-class-domain-uml/.../BuildConfig.groovy  

From then on, you can modify code in the plugin, and your "web" application reflects the changes immediately.

## Usage

1. Run your grails app 
2. Point your web browser to `http://localhost:8080/yourApp/uml`
3. (more specifically) 
```
curl -v -H "Content-Type: application/json" -d '{"fieldFilterRegexps"=["^id$","^version$"],"classFilterRegexps"=[".*City"],"diagramType"="DOMAIN","showCanonicalJavaClassNames":false}' http://localhost:8080/yourApp/uml 
```

## Screenshots

![Domain example](src/gdoc/0.2.5-domain.png)

![Layers example](src/gdoc/0.2.5-layers.png)
