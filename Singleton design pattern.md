## Singleton design pattern Example

1) One of the example is from DecisionTableReader. Where I think it parse jar file only once and collect all rate rule name in collection object at first place while jvm bounce. Since DecisionTableReader implement the method from the interface that usually executed once during jvm bounce. That's way the collection can be used and shared by each execution of different claim. Which means no need to spend time parsing rule jar each time claim process against DecisionTableReader to get that rule name collection.

2) Typically singletons are used for global configuration. The simplest example would be LogManager - there's a static LogManager.getLogManager() method, and a single global instance is used.

In fact this isn't a "true" singleton as you can derive your own class from LogManager and create extra instances that way - but it's typically used as a singleton.
