## Environment not able to point to RES even after WebSphere Console configuration:

We had to run modeling job in uat2 pointing rule app in RES. We configure admin console by setting RES data source in admin console. Once setup done we try which did not work. Thought App JVM need restart so it could work. JVM still did not help. Finally, we figured out we need to restart Admin Console itself so that it will pickup RES data source setting in admin console. Hope this helps in future with and similar problem.
Date: 3/8/2018 @TyataAcademy.
