# Jobs in Quartz.Net

Creating a Quartz.Net job is pretty straightforward. First, you must implement the IJob interface. The IJob interface is pretty simple. Here it is:
```csharp
public interface IJob
{
    void Execute(IJobExecutionContext context);
}
```

When it is time for the job to run or be executed, the scheduler calls the job’s Execute method and passes in the context. The context contains information about the job’s execution environment. The most important part of the context is probably the `JobDataMap`, which is used to pass parameters to the job. You can access the context's `JobDataMap` via the `MergedJobDataMap` property. It is a merge of the `JobDataMap` found on the `JobDetail` and the one found on the `Trigger`, with the value in the latter overriding any same-named values in the former.

A job can have multiple triggers, a trigger can only have one job.

The combination of `name` and `group` uniquely identify a job in Quartz.Net. You can also use the `JobKey` property to uniquely identify a job, which is basically a container that holds the job name and job group in one object. You can create jobs with different names in the same group or jobs with the same name in different groups.

## Built-in Jobs
**DirectoryScanJob**: Basically, you tell the job that it needs to check a certain directory to see if new files have been added or if existing files have been modified. When the job is executed it checks the files and stores the file information in its JobDataMap. The next time the job runs, it compares the information stored in the job’s JobDataMap, with the files in the directory. If any changes are found, the job calls the FilesUpdatedOrAdded method of the IDirectoryScanListener that was configured for that job, passing in an arrary of FileInfo objects.

To configure and use a `DirectoryScanJob`, you will need to set the following keys in the `JobDataMap`:
- `DIRECTORY_NAME`: this is the path to the directory that you want to monitor.
- `DIRECTORY_SCAN_LISTENER_NAME`: this is the name of the `DirectoryScanListener` that should called if changes are detected
- `MINIMUM_UPDATE_AGE`: this is an optional parameter which lets you specify how many milliseconds must have passed since the file’s last modified time in order to consider the file modified. If this parameter is not specified, it defaults to 5000 milliseconds.

**FileScanJob**

**NativeJob**

**NoOpJob**

**SendMailJob**

## Job Factories
The Quartz.Net scheduler implements the factory method pattern to create the jobs that will be executed. Job factories are responsible for producing job instances.

#### IJobFactory Interface
All job factories in Quartz.Net must implement the `IJobFactory` interface. The `NewJob` method will throw a `SchedulerException` if it was unable to create a new instance of the job.
```csharp
IJob NewJob(TriggerFiredBundle bundle, IScheduler scheduler);
```

#### Creating a Job Factory
If you want to create a job factory, you will need to (at a minimum) implement the interface, grab the job detail from the bundle and get the job type. Once you have the type, create a new instance of that type. Then, initialize the object properly and return the initialized object.

#### Built-in Job Factories
***SimpleJobFactory*** is the base class used by the other job factory.

***PropertySettingJobFactory*** creates a new job instance and then tries to set the properties on the job. This job factory inherits from `SimpleJobFactory` and reuses the mechanism to create the job (calling the default constructor). Once the new job is instantiated, this factory will pull all of the data from the JobDataMap and from the scheduler’s Context, merging them into one big data map.
The `PropertySettingJobFactory` includes two properties that you can set to change what happens when a key is present in the job map but an equivalent property is not found. These two properties are here:
```csharp
public virtual bool ThrowIfPropertyNotFound { get; set; }
public virtual bool WarnIfPropertyNotFound { get; set; }
```

#### Configuring Quartz.Net To Use A Custom Job Factory
To demonstrate how this is done we’ll modify the `quartz.config` file and tell Quartz.Net to use a `PropertySettingJobFactory` instead of the default `SimpleJobFactory`.

To do this we will need to add these 3 lines to the `quartz.config` properties file:
```
quartz.scheduler.jobFactory.type = Quartz.Simpl.PropertySettingJobFactory, Quartz
quartz.scheduler.jobFactory.ThrowIfPropertyNotFound = false
quartz.scheduler.jobFactory.WarnIfPropertyNotFound = false
```

Alternatively, if you are configuring Quartz.Net using the application’s configuration file, it would look something like this:
```
<add key=”quartz.scheduler.jobFactory.type” value=”Quartz.Simpl.PropertySettingJobFactory, Quartz”/>
<add key=”quartz.scheduler.jobFactory.ThrowIfPropertyNotFound” value=”false”/>
<add key=”quartz.scheduler.jobFactory.WarnIfPropertyNotFound” value=”false”/>
```

`quartz.scheduler.jobFactory.type` sets the Type of the job. The type is the .Net type of your job factory, and it should be specified with it’s fully qualified name followed by the assembly name (without the .dll extension).

`quartz.scheduler.jobFactory.x` is a prefix that indicates that any name following the prefix is a property on the job factory type you specified. In our example we have specified the two properties supported by the `PropertySettingJobFactory`.

With this configuration, every time the scheduler needs to create a job, it will use the `PropertySettingJobFactory` to create it.

## Scheduling Jobs
There are two ways you can schedule a job to run on Quartz.Net.

#### Scheduling Jobs via the xml file
The first way is to schedule it using the plugin that reads an xml file containing the job and scheduling information. Consider this short description of how the overall process works: 
- When you start the server, a job store is created in memory.
- This job store is empty until we load some jobs in it.
- As part of the server startup, the xml plug-in runs, and it reads the quartz_jobs.xml.
- The plug-in then creates the jobs and triggers that are described in this file.

#### Scheduling Jobs via the Scheduler API
You can also schedule jobs by interacting directly with the scheduler API. The Quartz.Net scheduler supports remote access via the .Net remoting facility. The remote scheduler does have some limitations.

In a nutshell, these are the steps you would follow to schedule a job using the API:
1. Get a reference to the scheduler object. How you do this will depend on whether you are running a standalone or embedded scheduler.
2. Create a `JobDetail` object and set all the necessary properties. At a minimum you must set the `Name`, `Group` and `JobType`. Please note that because the type of JobType is Type, the dll containing the JobType MUST be available to the code that is scheduling the job AS WELL as to the scheduler.
3. Create a `Trigger` object (`Trigger` itself is an abstract class, so you will actually be creating one of these concrete types: `SimpleTrigger`, `CronTrigger`,etc.) and set all the necessary properties. At a minimum you must set the `Name`, and the `Group`. The `JobGroup` and the `JobName` are also necessary as they are what links the `JobDetail` you created in the previous step with the `Trigger` you are creating now. However, you do not need to set these properties right now. Read on to step 4 to understand why.
4. Call the `ScheduleJob` method on your scheduler object passing in your Trigger and your Job. If you call this overload of the `ScheduleJob` method, then the scheduler will link your trigger and your job, so there is no need for you to set the link explicitly.

Code Example
```csharp
public void ScheduleOneTimeJob(Type jobType, JobDataMap dataMap)
{
  string name = DateTime.Now.ToString("yyyyMMddHHmmss"));
  string group = "OneTimeJobs";
  // create job
  IJobDetail jobDetail = JobBuilder
    .Create()
    .OfType(jobType)
    .WithIdentity(new JobKey(name, group))
    .UsingJobData(dataMap)
    .Build();
  // create trigger
  ITrigger trigger = TriggerBuilder
    .Create()
    .ForJob(jobDetail)
    .WithIdentity(new TriggerKey(name, group));
    .WithSchedule(SimpleScheduleBuilder.Create())
    .StartNow()
    .Build();
  // get reference to scheduler (remote or local) and schedule job
  GetScheduler().ScheduleJob(jobDetail, trigger);
}
```