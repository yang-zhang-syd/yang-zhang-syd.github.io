---
layout: post
title:  "Quartznet Database (Mysql) Example"
date:   2018-07-29 17:00:00 +1000
categories: jekyll update
---
>* You can download the [**source code**](https://github.com/yang-zhang-syd/quartznet-database-demo) on Github.
>* A set of data tables need to be generated in the database. I have included the sql scripts in the [**source**](https://github.com/yang-zhang-syd/quartznet-database-demo/tree/master/quartznet-database-demo/sql).
<!--more-->
To use quartznet with database as the datastore, we need to set the following properties. I have created a [**utility method**](https://github.com/yang-zhang-syd/quartznet-database-demo/blob/master/quartznet-database-demo/Utils.cs) to parse the json configuration file.

{% highlight json %}
"Quartz": {
    "quartz.scheduler.instanceName": "DemoScheduler",
    "quartz.scheduler.instanceId": "instance1",
    "quartz.threadPool.type": "Quartz.Simpl.SimpleThreadPool, Quartz",
    "quartz.threadPool.threadCount": "10",
    "quartz.threadPool.threadPriority": "Normal",
    "quartz.jobStore.misfireThreshold": "60000",
    "quartz.jobStore.type": "Quartz.Impl.AdoJobStore.JobStoreTX, Quartz",
    "quartz.jobStore.useProperties": "true",
    "quartz.jobStore.dataSource": "default",
    "quartz.jobStore.tablePrefix": "QRTZ_",
    "quartz.jobStore.lockHandler.type": "Quartz.Impl.AdoJobStore.UpdateLockRowSemaphore, Quartz",
    "quartz.dataSource.default.provider": "MySql",
    "quartz.serializer.type": "binary",
    "quartz.dataSource.default.connectionString": "server=localhost;database=quartz;uid=root;pwd=password;SslMode=none"
}
{% endhighlight %}

Before creating a job, we need to check if the job was already created before. If the job exists in the datastore, then we need to read it instead of creating a new one. 

{% highlight csharp %}
var helloJob = await scheduler.GetJobDetail(new JobKey("HelloJob"));
if (helloJob == null)
{
    Console.WriteLine("No HelloJob found in datastore. Creating a new Job record!");

    var helloJobCron = config["CronSchedule:hello.job"];
    helloJob = JobBuilder.Create<HelloJob>()
        .WithIdentity("HelloJob")
        .StoreDurably(true)
        .Build();
    var trigger = TriggerBuilder.Create()
        .WithIdentity("HelloJobTrigger")
        .WithCronSchedule(helloJobCron, x => x.WithMisfireHandlingInstructionFireAndProceed())
        .Build();
    await scheduler.ScheduleJob(helloJob, trigger);
}
else
{
    Console.WriteLine("Read HelloJob from datastore!");
}
{% endhighlight %}

* It is important to assign a job identity `WithIdentity("HelloJob")` which will be used later to retrieve it from the datastore.
* When creating a trigger, there are three types of misfire instructions to choose.

    Name | Description 
    --- | ---
    WithMisfireHandlingInstructionFireAndProceed | Immediately executes first misfired execution and discards other (i.e. all misfired executions are merged together). Then back to schedule. No matter how many trigger executions were missed, only single immediate execution is performed.
    WithMisfireHandlingInstructionDoNothing | All misfired executions are discarded, the scheduler simply waits for next scheduled time.
    WithMisfireHandlingInstructionIgnoreMisfires | All misfired executions are immediately executed, then the trigger runs back on schedule.

In the example, I use the Cron Trigger `0/2 * * * * ?` which will trigger the job every two seconds. A full tutorial on Cron Trigger can be found [here](http://www.quartz-scheduler.org/documentation/quartz-2.x/tutorials/crontrigger.html).

