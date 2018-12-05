---
layout: post
title:  "Danger of not awaiting async"
date:   2018-12-05 00:00:00 +1000
categories: tech blogs
---
Recently found a bug in a production system where exceptions happened when the code tried to commit data into database. 

The exception was 
```
InvalidOperationException
The transaction has already been committed or rolled back.
```

The cause of the exception was because a very deep invocation of an asynchronous handler was not awaited. If not awaited, the method will keep running outside of the scope of the sql transaction and when the method tries to commit into database, the exception will be thrown. 
<!--more-->
The interface is defined like below. It is hard to identify that this is an async method from the caller’s perspective and very hard to identify it as a potential problem when debugging. 
```
Public interface IDoSomething 
{
	Task DoSomeThing();
}
```

I come to a conclusion that when defining an async method signature it is the best to put ::Async:: into the method name. This way every caller of the method will know the method needs to be awaited. 
```
Public interface IDoSomething
{
	Task DoSomeThingAsync();
}
```


#blog