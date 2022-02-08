!!Springboard
Premier Discount is built on Springboard - HCSC's starter application.  Springboard contains the HCSC framework libraries, standard security libraries, and various test and monitoring tools.  Springboard itself is the working shell of an app that can run in a variety of contexts depending on the deployment target.\\

!!!Standard Architecture
The standard architecture involves two separate ears.  We tried and did not succeed in getting a pass to write all of our code in a single ear.  The idea is to develop a standalone presentation application that also acts as a webservice client, and a back-end application that acts as a webservice provider.  The front-end application for PremierDiscount is named premdiscui, while the back-end is simply named premdisc.

!!!Tools
The application is built on Spring 3.0.5, Hibernate 3.5.6, and uses Maven 2.2.1 to manage dependencies.  \\

!!Java
*[http://docs.oracle.com/javase/6/docs/api/]\\
*[http://commons.apache.org/]\\

!!Spring Info
*The application runs in spring contexts.  The contexts are configured in the premdiscui.web and premdisc.services projects, where there are files on the respective classpaths name 'beanRefContext.xml'.  There is one additional beanRefContext file in the premdisc.integration package.  It is used for integration testing data access code.\\
*The standalone context is used locally. It contains the Hibernate tx manager, an off-the-shelf open source datasource/connection pooling solution, mock security configuration and access rules from the local files system.\\
*The default context is used in deployed environments and looks for it's datasource via JNDI and uses other container-only stuff such as the Websphere UOW Transaction Manager, and HCSC's security framework (which is integrated w/ LDAP).\\
*Spring beans are generally wired in using context scanning.  Beans are assigned stereotypes which the context scanner can interpret and categories. Injection is generally done via Autowiring by type. \\
*Spring Batch is used for our batch process configurations.  Spring batch uses a blend of Hibernate and JDBC, and MANAGES IT'S OWN TRANSACTIONS, TRANSACTION ISOLATION LEVELS, and TRANSACTION PROPAGATION.  More on Spring Batch in [System Process Flows].\\
!Docs
*Spring:[http://www.springsource.org/spring-framework#documentation]\\
*Spring Batch: [http://static.springsource.org/spring-batch/]


!!Hibernate Info
The application uses Hibernate as an ORM.  The Hibernate config is in the resources folder of the com.hcsc.premdisc.integration project.  There you'll find configurations for standalone driver manager datasources, connection pool datasources, as well as default configurations for WAS datasources.  The application code that leverages Hibernate can be found in both the com.hcsc.premdisc.common and com.hcsc.premdisc.integration projects.  Classic anemic entities are in the common project - they're mapped to hibernate entities via annotations.  Data access objects and they're interfaces are in the integration project.  Daos are Spring beans, and are available for autowiring via package scanning.  Daos use the 'Repository' stereotype.\\

!Docs
Ref - [http://docs.jboss.org/hibernate/orm/3.5/reference/en-US/pdf/hibernate_reference.pdf]\\
API - [http://docs.jboss.org/hibernate/orm/3.5/api/]\\


!!CXF Info
The webservice layer between the two ears that the app is composed of are written using Apache CXF. Beans generated using CXF (webservice contract beans) can be found in the com.hcsc.premdisc.contract project.  THESE FILES ARE GENERATED. %%(color:red)NEVER EDIT THE GENERATED WSDLS OR CONTRACT CLASSES BY HAND.%% We don't code any webservice contract classes by hand.
!WSDL generation
In the back-end tools project (com.hcsc.premdisc.tools), you'll find the source for contract objects.  You'll also find an installation of the CXF plugin in the tools project's Maven POM.  The Java source available in the tools project serves as the base for the WSDL that we generate using the maven CXF plugin.  If a WSDL needs to be created from scratch or updated, make the appropriate changes to the java source in the back-end tools project, update the POM to reference the correct class and use the maven goal 'process-classes' to re-compile the new classes and produce a new WSDL. You will find the new WSDL in the {{target/generated/wsdl}} folder of this project.  This should be copied to the facade project's {{src/main/resources/wsdl}} folder when it's ready for use.
!Web service endpoint ''(new web services)''
Add your web service endpoint configuration to {{com.hcsc.premdisc.facade/src/main/resources/config/applicationContext-webService.xml}}.
!Contract generation
In the front-end tools project (com.hcsc.premdiscui.tools), you'll find the plugin used to generate the contract objects found in the com.hcsc.premdisc.contract project.  In order to generate new contract objects, copy the WSDL you'd like to generate the new objects from to the {{scripts/ws/contracts}} folder.  Then, update the project's POM to reference the appropriate WSDL file, and run the maven goal 'generate-sources'.  You'll find the generated contract objects in the project's {{target/generates/cxf}} folder.\\
NOTE - all of the classes generated can (should) be copied to the contract project, with the exception of the ObjectFactory class.  This file NEEDS TO BE MERGED with the ObjectFactory class in the contract project each time it's regenerated. You can avoid this step by creating a separate package for your web service. Do this by prepending the package suffix in the namespace host name in {{@WebService(targetNamespace)}} within your facade's Java interface (e.g., {{http://status.contract.premdisc.hcsc.com/}} instead of {{http://contract.premdisc.hcsc.com/}} would put the generated sources into {{com.hcsc.premdisc.contract.status}} instead of {{com.hcsc.premdisc.contract}}).\\

;:If you are using java.util.Date or java.util.Calendar, you will need to update {{com.hcsc.premdiscui.tools/scripts/ws/contract-bindings.xml}} so it refers to your WSDL. You must also be sure to check which datatype conversion {{Adapter}} is assigned to your fields. The available adapters are in {{org.w3._2001.xmlschema}}. There are multiples because contracts are built independently and merged, so one WSDL's adapter may not be compatible with another's. For now, you must manually figure out which adapter to use and possibly modify your generated classes so that they use the correct one (e.g., StatusFacade's were generated with Adapter1, but they were modified to use Adapter3.) If you do not do this, you may lose date or time information in the message.
!Web service client ''(new web services)''
Add your web service client configuration to {{com.hcsc.premdiscui.integration/src/main/resources/META-INF/premdiscui-integration/applicationContext-remoteService-jaxws.xml}} and the local URL to the web service to {{com.hcsc.premdiscui.web/src/main/resources/META-INF/premdiscui-web/standalone/placeholders.properties}}.

!!Premier Discount-Specific Design Decisions
* The application is currently developed in the 'development' branch, and releases are done from HEAD.  Little development is done directly in HEAD.  Generally, emergency bugs fixes only.  Merges are usually one-way, from developmemt to HEAD.  There are two configuration differences between development and HEAD that are of note:
** com.hcsc.premdisc.integration/src/main/resources/standalone/placeholders.properties - path to rule archives
** com.hcsc.premdisc.integration/src/main/resources/standalone/*daoConfig.xml - path to database (HEAD generally points to PDDEVD2, development to PDDEVD1)
* The two ear application architecture makes server side validation without managing two sets of database config relatively challenging, especially when validations can include 'warnings' that can be clicked through.  All of the business validation takes place in the back end application with results being put over the wire on the service projects 'ValidationResult' object.  Formatting, length, various technical validation all takes place in the front end app.  Each time a validation result is sent back and forth, all warnings and errors are included.  The logic that suppresses warning and maintains the state of validation (which is used to enable the click through warnings) is in the premdiscui.web project.
* Validations that trigger events in the application - ex. updating object A invalidates object B - is enabled via Spring Eventing.  The events are not executed on background threads (they are synchronous) and source code for those can be found in the service project in the com.hcsc.premdisc.event.listener package.
* ''__Rather than use the unnecessarily complex and oppressive webservice layer that sits in the middle of the application for no real reason as simple a pass-through__'', we thought we might as well use it as an opportunity to simplify/flatten/add context to contract objects that would otherwise be more cumbersome and less understandable to manipulate.  As a result, there are instances (ex. adjustments) where the business objects being used in the front end of the application vary greatly from the underlying database entities.  The mappers for these classes can be found in the com.hcsc.premdisc.common project.
* The rules engine is embedded in the application, as opposed to the IBM-recommended use of Rule Execution.  The Rule Execution Contexts are maintained in an object pool whose configuration can be found in the service project.  When working in the stable version of the code (HEAD), the rule archives are read from the c:\build\business folder, and when working in the development branch, they are read from c:\build\dev.  The paths to these archives is maintained in the com.hcsc.premdisc.integration project in the 'placeholder.properties' file.
