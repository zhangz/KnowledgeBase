## What Is Quartz.Net
- Scheduler (think task scheduler)
- Queue for asynchronous jobs
- C# port of Quartz (java)
- Apache license

## Why Use Quartz.Net
- Scale out
- Redundancy
- Smart handling of failures
- Job chaining (poor man’s workflow)
- Custom scheduling

## The Basics
- Scheduler
	- Runs jobs
	- Manages the scheduling
- Jobs
	- Do the work
	- Some built-in
	- Mostly roll you own
	- Implement IJob
- Triggers
	- Tell the scheduler when jobs should run
- Scheduling
	- Associate a job to a trigger
	- Multiple triggers can be set
	- When the trigger fires, the job runs
			scheduler.ScheduleJob(jobDetail, trigger);

## Advanced Features
- Listeners
- Special Jobs
	- Stateful Jobs
	- Interruptible Jobs
- Remote management
	- Scheduler can be managed remotely
	- Exposed via Remoting
	- *Most* scheduler functions available
- Clustering
	- Load balancing
	- Job Failover
	- Caveat: clocks synchronized within a second
- Plug-ins
- Unit testing

## How Can I Run It
There are at least two ways to use Quartz.Net: 
- Embedded (hosted) in your application 
- As a standalone windows service

*When Quartz.Net is running, you may get a firewall warning window, which is because by default the scheduler exposes a remoting interface that allows you to interact with it.*

## Set up Quartz Server

#### Embedding Quartz.Net in Your Application
If you are going to host the Quartz.Net scheduler in your application, you’ll need to create a scheduler. This can be easily done through the scheduler factory provided in the distribution. The scheduler factory configures your scheduler for you.

Once you’ve got a reference to the scheduler, and in order to be able to do something useful with it, you must start it. After your scheduler is started it will start firing triggers and executing the jobs that have been loaded. We’ll talk more about how to load jobs into the scheduler later, but one way to do it is to add them every time you start the scheduler.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Quartz;
using Quartz.Impl;
using Quartz.Job;

namespace Lesson1
{
    class Program
    {
        static void Main(string[] args)
        {
            //Create the scheduler factory
            ISchedulerFactory schedulerFactory = new StdSchedulerFactory();

            //Ask the scheduler factory for a scheduler
            IScheduler scheduler = schedulerFactory.GetScheduler();

            //Start the scheduler so that it can start executing jobs
            scheduler.Start();

            // Create a job of Type WriteToConsoleJob
            IJobDetail job = JobBuilder.Create(typeof(WriteToConsoleJob)).WithIdentity("MyJob", "MyJobGroup").Build();

            //Schedule this job to execute every second, a maximum of 10 times
            ITrigger trigger = TriggerBuilder.Create().WithSchedule(SimpleScheduleBuilder.RepeatSecondlyForTotalCount(10)).StartNow().WithIdentity("MyJobTrigger", "MyJobTriggerGroup").Build();
            scheduler.ScheduleJob(job, trigger);

            //Wait for a key press. If we don't wait the program exits and the scheduler gets destroyed
            Console.ReadKey();

            //A nice way to stop the scheduler, waiting for jobs that are running to finish
            scheduler.Shutdown(true);
        }
    }
}
```
#### Using Quartz.Net as a Standalone Windows Service

To use Quartz.Net as a windows service you’ll need to install and configure it. To do this, type `Quartz.Server.exe install` in the command prompt. Now type `services.msc` in the command prompt to open the services administration window. There you should see the quartz service installed as “Quartz Server” with automatic startup and running under the Local System account.  

#### Installing Multiple Instances of Quartz.Net on One Machine
First you need to create a separate directory for the new Quartz.Net instance that we are about to create.
**Step 1 – Change the Configuration File**
In order to the change the Quartz.Net service name, we’re going to be modifying the `Quartz.Server.exe.config` file. Uncomment the `<quartz>` section and add a few settings in there. Feel free to name your service appropriately.

```xml
<quartz > 
  <add key="quartz.checkConfiguration" value="false"/> 
  <add key="quartz.server.serviceName" value="JQuartz"/> 
  <add key="quartz.server.serviceDisplayName" value="JQuartz"/> 
  <add key="quartz.server.serviceDescription" value="JQuartz description"/> 
</quartz> 
```
**Step 2 – Install the Service**
Open a command prompt as administrator in the same folder where your new service files are. Now type `Quartz.Server.exe install` in the command prompt.

## Configuring Quartz.Net
There are 3 files we need to look at: 
- Quartz.Server.Service.exe.config
- quartz.config
- quartz_jobs.xml

#### Job File Format
quartz_jobs.xml, which contains the jobs that will be loaded by the initialization plugin.:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- This file contains job definitions in schema version 2.0 format -->
<job-scheduling-data xmlns="http://quartznet.sourceforge.net/JobSchedulingData" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" version="2.0">
  <processing-directives>
    <overwrite-existing-data>true</overwrite-existing-data>
  </processing-directives>
  <schedule>
    <job>
        <name>sampleJob</name>
        <group>sampleGroup</group>
        <description>Sample job for Quartz Server</description>
        <job-type>Quartz.Server.SampleJob, Quartz.Server</job-type>
        <durable>true</durable>
        <recover>false</recover>
    </job>
    <trigger>
      <simple>
        <name>sampleSimpleTrigger</name>
        <group>sampleSimpleGroup</group>
        <description>Simple trigger to simply fire sample job</description>
        <job-name>sampleJob</job-name>
        <job-group>sampleGroup</job-group>
        <misfire-instruction>SmartPolicy</misfire-instruction>
        <repeat-count>-1</repeat-count>
        <repeat-interval>10000</repeat-interval>
      </simple>
    </trigger>
  </schedule>
</job-scheduling-data>
```

- The `<pre-processing-commands>` element is used to run commands prior to adding jobs and triggers to the scheduler.
- The `<processing-directives>` element lets you specify how the xml file is to be processed.
	- `<overwrite-existing-data>` element. Setting its value to `true` will replace any jobs currently scheduled with the new schedule given in the xml file (`true` is the default setting).
	- `<ignore-duplicates>` element
- The `<schedule>` element is where we describe the jobs and triggers that we want to schedule. Under the `<schedule>` element, we can have `<job>` and `<trigger>` elements, which are the building blocks that we use to put together our schedule.
- The `<job>` element is used to describe the `IJob` that we want the scheduler to execute. The job is identified by both the name and the group. Job details are specified by using the following elements:
 	- `<name>`
	- `<group>`
	- `<description>`
	- `<job-type>`
	- `<durable>`: setting this to `true` means that the job will be kept around even if it doesn’t have any triggers pointing to it.
	- `<recover>`: tells the scheduler whether it should try to recover (re-execute) the job if something goes wrong with the scheduler itself while the job was running. This means the job should be coded in such a way that its work is idempotent.
	- `<job-data-map>`: this map is used to pass data to the job.
- The `<trigger>` element is used to describe the trigger that we want to attach to a given job. The trigger is identified by both the name and the group. The xml file loader plugin supports 3 types of triggers:
	- simpleTriggerType
	- cronTriggerType
	- calendarIntervalTriggerType

  These three trigger types are based on the abstractTriggerType, which has the following common elements:
	- `<name>`
	- `<group>`
	- `<description>`
	- `<job-name>`
	- `<job-group>`
	- `<priority>`
	- `<calendar-name>`
	- `<job-data-map>`

  In addition to the elements listed above, each trigger type supports some additional elements.
  
Trigger Type | Additional Elements
  ------------ | -------------
  SimpleTrigger | `<misfire-instruction>`,`<repeat-count>`,`<repeat-interval>`
  CronTrigger | `<misfire-instruction>`,`<cron-expression>`,`<time-zone>`
  CalendarIntervalTrigger | `<misfire-instruction>`,`<repeat-interval>`,`<repeat-interval-unit>`

#### How Does Quartz.Net Configuration Work?
In Quartz.Net, the `StdSchedulerFactory` is responsible for configuring the scheduler. When the Quartz.Net scheduler is started, the factory will try to automatically configure a scheduler by looking for configuration information in different places:

- the hosting application’s configuration file
- a file specified in an environment variable
- the quartz.config file
- the embedded configuration file

First, the factory will try to load the scheduler configuration from the `<quartz>` section of your application’s config file.

If the factory was not able to load the quartz configuration section, then the second place it will check is the environment variables of the process. Specifically, the factory will check to see if the `quartz.config` environment variable has been defined. If such a variable exists and is not empty, the factory will try to load the configuration from whatever file is specified as the value of the variable. For example, let’s say that your configuration file is called myconfig.config. If you set the quartz.config environment variable equal to myconfig.config, then the scheduler will load the configuration information from the file named myconfig.config.

Let’s assume that so far, the factory has not been able to load the scheduler’s configuration. The next step that the factory will take is to try to load a file called `quartz.config` from the same directory where the hosting application’s assembly was loaded. In fact, this is the file that an out-of-the-box Quartz.Net server/service installation uses to configure itself. 

quartz.config:
```xml
# You can configure your scheduler in either <quartz> configuration section
# or in quartz properties file
# Configuration section has precedence

quartz.scheduler.instanceName = MyScheduler

# configure thread pool info
quartz.threadPool.type = Quartz.Simpl.SimpleThreadPool, Quartz
quartz.threadPool.threadCount = 20
quartz.threadPool.threadPriority = Normal

# job initialization plugin handles our xml reading, without it defaults are used
quartz.plugin.xml.type = Quartz.Plugin.Xml.XMLSchedulingDataProcessorPlugin, Quartz
quartz.plugin.xml.fileNames = ~/quartz_jobs.xml

# export this server to remoting context
quartz.scheduler.exporter.type = Quartz.Simpl.RemotingSchedulerExporter, Quartz
quartz.scheduler.exporter.port = 555
quartz.scheduler.exporter.bindName = QuartzScheduler
quartz.scheduler.exporter.channelType = tcp
quartz.scheduler.exporter.channelName = httpQuartz
```

If all of the previous configuration options fail, then the factory falls back on loading the configuration file that is embedded in the quartz assembly.

You’d think that by now, the whole configuration process is finished and the scheduler has been configured successfully. Well, the default scheduler factory takes one last step before giving you the configured scheduler. 

Here’s what happens just after your configuration is loaded: if any of the configuration properties that you set in a configuration are also present in the environment variables, then the factory will overwrite them with the environment value.

You can use the `GetMetaData()` method on the instance of `Scheduler` to get information such as the version of quartz.net, threadpool size, jobStoreType, jobStorePersistanceType, etc.

## Resources
https://github.com/quartznet/quartznet
https://github.com/jvilalta/QuartzNetBook
https://github.com/jvilalta/QuartzNetFeaturePack
http://jayvilalta.com/blog/2015/03/20/introducing-quartz-net-feature-pack/
http://jayvilalta.com/blog/2012/11/24/table-of-contents-for-quartz-net-ebook/
http://www.quartz-scheduler.net/documentation/quartz-2.x/tutorial/
http://www.quartz-scheduler.net/documentation/best-practices.html
https://github.com/mausch/QuartzNetWebConsole
https://github.com/guryanovev/CrystalQuartz