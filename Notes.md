Welcome to the Java-Notes wiki!

## Generics: 
In a nutshell, generics enable types (classes and interfaces) to be parameters when defining classes, interfaces and methods. Much like the more familiar formal parameters used in method declarations, type parameters provide a way for you to re-use the same code with different inputs. The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types.

Code that uses generics has many benefits over non-generic code:

Stronger type checks at compile time.
A Java compiler applies strong type checking to generic code and issues errors if the code violates type safety. Fixing compile-time errors is easier than fixing runtime errors, which can be difficult to find.

Elimination of casts.
The following code snippet without generics requires casting:
    
    List list = new ArrayList();
    list.add("hello");
    String s = (String) list.get(0);
       
When re-written to use generics, the code does not require casting:

    
    List<String> list = new ArrayList<String>();
    list.add("hello");
    String s = list.get(0);   // no cast
    
Enabling programmers to implement generic algorithms.
By using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.

Reference: 
[Link](https://docs.oracle.com/javase/tutorial/java/generics/why.html)



## Hibernate Criteria Query vs Named Query: 
Most of the time I have seen in my Enterprise projects that executing Criteria query produces timeout issue where as Named Query comparatively execute faster and fix those performance issues.


## Environment not able to point to RES even after WebSphere Console configuration: 
We had to run modeling job in uat2 pointing rule app in RES. We configure admin console by setting RES data source in admin console. Once setup done we try which did not work. Thought App JVM need restart so it could work. JVM still did not help. Finally, we figured out we need to restart Admin Console itself so that it will pickup RES data source setting in admin console. Hope this helps in future with and similar problem.
Date: 3/8/2018 @TyataAcademy.

## Hash Set for Custom Objects
Hashset for String is absolutely working for storing unique Strings. When it comes to string custom objects with with common property. We need to implement hashcode and equals for that custom class. Please find example in below link.
http://www.java2novice.com/java-collections-and-util/hashset/duplicate/

## Best way to convert list to comma separated string in java [duplicate]
From Apache Commons library: import org.apache.commons.lang3.StringUtils

Use: StringUtils.join(slist, ',');

Link : https://stackoverflow.com/questions/15815587/best-way-to-convert-list-to-comma-separated-string-in-java

## How to generate exception in code(Rollback Exception):
Sometime for testing purpose the proof of concept we need to intentionally create an exception in code to recreate the rollback exception scenario in real life application. For example if an application is trying to save huge(100 of thousands) of data in database table and for some reason(Timeout/maintenance/external concurrent process) the saving process may get affected and there is high chance that it may get rollback. In such situation the process will not get completed and application wont get chance handle that properly. To handle such situation or to react such rollback exception we can apply intentional rollback exception with condition under exception clause and make appropriate db update so that user wont get confused and make sure exception properly handled. 

Below is an example code snippet of that.
    
    try{
	for(PrtSevAr psaobj: psalist){
		psa.setHsc(shcg);
		psaDao.set(psa);
	}
	if(ValidationTypes.SUCCESS == vr.getValidationType()){
		throw new RollbackException();
       }
    }catch(final Exception e){
	    uploadStatus = "Error";
	    hsCDao.saveUploadStatus(savedHsCId,uploadStatus);
	    LOG.error("Unable to save. "+e);
	    vr.setValidationType(ValidationTypes.Error);
	    vr.setCode("Falied");
	    return vr;
    }          

In above under if condition it throw new RollbackException intentionally for testing purpose.


## Singleton design pattern Example

1) One of the example is from DecisionTableReader. Where I think it parse jar file only once and collect all rate rule name in collection object at first place while jvm bounce. Since DecisionTableReader implement the method from the interface that usually executed once during jvm bounce. That's way the collection can be used and shared by each execution of different claim. Which means no need to spend time parsing rule jar each time claim process against DecisionTableReader to get that rule name collection.

2) Typically singletons are used for global configuration. The simplest example would be LogManager - there's a static LogManager.getLogManager() method, and a single global instance is used.

In fact this isn't a "true" singleton as you can derive your own class from LogManager and create extra instances that way - but it's typically used as a singleton.
