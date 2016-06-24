# Quartz.Net Listeners

## What are Quartz.Net listeners?
Quartz.Net listeners are objects that can be notified when certain events happen inside a Quartz.Net scheduler. There are 3 types of listeners:
- Scheduler listeners
- Job listeners
- Trigger listeners

Let's look at each of these listeners in more detail.

## Scheduler Listeners
Scheduler listeners are used when you want to be notified of scheduler level events. What are scheduler level events? There are quite a few, so I’ve grouped them by the object to which they apply.
##### Job Events
- A job was added
- A job was deleted
- A job was scheduled
- A job was unscheduled
- A job was paused
- A job was resumed
- Several jobs were paused
- Several jobs were resumed

##### Trigger Events
- A trigger was finalized
- A trigger was paused
- A trigger was resumed
- Several triggers were paused
- Several triggers were resumed

##### Scheduler Events
- The scheduler is starting
- The scheduler started
- The scheduler is shutting down
- The scheduler has shut down
- The scheduler has moved to standby mode
- There was a scheduler error
- The scheduler’s scheduling data has been cleared

Scheduler listeners must implement the `ISchedulerListener` interface.

#### Built-In Scheduler Listeners
###### SchedulerListenerSupport
An empty class where all the methods of the interface are implemented but left empty.

###### BroadcastSchedulerListener
The documentation for the BroadcastSchedulerListener says that this class
> Holds a List of references to SchedulerListener instances and broadcasts all events to them (in order).
> This may be more convenient than registering all of the listeners directly with the Scheduler, and provides the flexibility of easily changing which listeners get notified.

In essence this listener manages a list of listeners and then forwards all of the scheduler events to all the listeners. This is probably useful if you have a large number of scheduler listeners.

#### When would I use scheduler listeners?
In general, scheduler level events can be quite useful if you need to keep track or audit changes made to the scheduler and the schedule. Being notified of errors is also useful to monitor possible issues with the scheduler itself.

#### Implementing the ISchedulerListener Interface
To implement a scheduler listener, you need to implement the `ISchedulerListener` interface.
```csharp
public interface ISchedulerListener
{
    void JobScheduled(ITrigger trigger);
    void JobUnscheduled(TriggerKey triggerKey);
    void TriggerFinalized(ITrigger trigger);
    void TriggerPaused(TriggerKey triggerKey);
    void TriggersPaused(string triggerGroup);
    void TriggerResumed(TriggerKey triggerKey);
    void TriggersResumed(string triggerGroup);
    void JobAdded(IJobDetail jobDetail);
    void JobDeleted(JobKey jobKey);
    void JobPaused(JobKey jobKey);
    void JobsPaused(string jobGroup);
    void JobResumed(JobKey jobKey);
    void JobsResumed(string jobGroup);
    void SchedulerError(string msg, SchedulerException cause);
    void SchedulerInStandbyMode();
    void SchedulerStarted();
    void SchedulerStarting();
    void SchedulerShutdown();
    void SchedulerShuttingdown();
    void SchedulingDataCleared();
}
```

In a nutshell, those are the scheduler level events that you can get notifications for.
Notice that you can add and schedule jobs even before the scheduler starts. Also notice that when shutting the scheduler down, the scheduler gets set to standby mode.

#### Code Example
```csharp
public class SchedulerListenerExample : ISchedulerListener
{

	public void JobAdded(IJobDetail jobDetail)
	{
		Console.WriteLine("The scheduler called {0} for job {1}", MethodBase.GetCurrentMethod().Name, jobDetail.Key);
	}

	public void JobDeleted(JobKey jobKey)
	{
		Console.WriteLine("The scheduler called {0} for job {1}", MethodBase.GetCurrentMethod().Name, jobKey);
	}

	public void JobPaused(JobKey jobKey)
	{
		Console.WriteLine("The scheduler called {0} for job {1}", MethodBase.GetCurrentMethod().Name, jobKey);
	}

	public void JobResumed(JobKey jobKey)
	{
		Console.WriteLine("The scheduler called {0} for job {1}", MethodBase.GetCurrentMethod().Name, jobKey);
	}

	public void JobScheduled(ITrigger trigger)
	{
		Console.WriteLine("The scheduler called {0} for job {1}, caused by trigger {2}", MethodBase.GetCurrentMethod().Name, trigger.JobKey, trigger.Key);
	}

	public void JobUnscheduled(TriggerKey triggerKey)
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void JobsPaused(string jobGroup)
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void JobsResumed(string jobGroup)
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void SchedulerError(string msg, SchedulerException cause)
	{
		Console.WriteLine("The scheduler called {0}. The cause was {1}", MethodBase.GetCurrentMethod().Name, cause);
	}

	public void SchedulerInStandbyMode()
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void SchedulerShutdown()
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void SchedulerShuttingdown()
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void SchedulerStarted()
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void SchedulerStarting()
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void SchedulingDataCleared()
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void TriggerFinalized(ITrigger trigger)
	{
		Console.WriteLine("The scheduler called {0} for trigger {1}, belonging to job {2}", MethodBase.GetCurrentMethod().Name, trigger.Key, trigger.JobKey);
	}

	public void TriggerPaused(TriggerKey triggerKey)
	{
		Console.WriteLine("The scheduler called {0} for trigger {1}", MethodBase.GetCurrentMethod().Name, triggerKey);
	}

	public void TriggerResumed(TriggerKey triggerKey)
	{
		Console.WriteLine("The scheduler called {0} for trigger {1}", MethodBase.GetCurrentMethod().Name, triggerKey);
	}

	public void TriggersPaused(string triggerGroup)
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}

	public void TriggersResumed(string triggerGroup)
	{
		Console.WriteLine("The scheduler called {0}", MethodBase.GetCurrentMethod().Name);
	}
}
```

## Job Listeners
Job listeners are used when you want to be notified of job level events. Here’s a list of the events that are supported at the job level:
- A job’s execution was vetoed
- A job is about to be executed
- A job was executed
Job listeners must implement the `IJobListener` interface.

#### Built-In Job Listeners
###### JobListenerSupport
It's an empty implementation, so perhaps useful if you're only interested in some of the methods and want to save some keystrokes. It also includes a logger instance in it.

###### BroadcastJobListener
This class is also similar to the `BroadcastSchedulerListener` we went over. This broadcast listener forwards all of the job events to all the registered job listeners. If you have lots of job listeners, this could be useful.

###### JobChainingJobListener
The documentation for JobChainingJobListener states that this class
> Keeps a collection of mappings of which Job to trigger after the completion of a given job. If this listener is notified of a job completing that has a mapping, then it will then attempt to trigger the follow-up job. This achieves "job chaining", or a "poor man's workflow".
> 
> Generally an instance of this listener would be registered as a global job listener, rather than being registered directly to a given job.
> 
> If for some reason there is a failure creating the trigger for the follow-up job (which would generally only be caused by a rare serious failure in the system, or the non-existence of the follow-up job), an error messsage is logged, but no other action is taken. If you need more rigorous handling of the error, consider scheduling the triggering of the flow-up job within your job itself.

Kind of cool no? A simple way to chain jobs is provided in the box for you. It's not a fully developed workflow engine but for sequential jobs it probably does the trick. Let's look at the two methods of interest in this job listener. Here they are:
```csharp
public void AddJobChainLink(JobKey firstJob, JobKey secondJob)
{
    if (firstJob == null || secondJob == null)
    {
        throw new ArgumentException("Key cannot be null!");
    }
    if (firstJob.Name == null || secondJob.Name == null)
    {
        throw new ArgumentException("Key cannot have a null name!");
    }

    chainLinks.Add(firstJob, secondJob);
}

public override void JobWasExecuted(IJobExecutionContext context, JobExecutionException jobException)
{
    JobKey sj;
    chainLinks.TryGetValue(context.JobDetail.Key, out sj);

    if (sj == null)
    {
        return;
    }

    Log.Info(string.Format(CultureInfo.InvariantCulture, "Job '{0}' will now chain to Job '{1}'", context.JobDetail.Key, sj));
    try
    {
        context.Scheduler.TriggerJob(sj);
    }
    catch (SchedulerException se)
    {
        Log.Error(string.Format(CultureInfo.InvariantCulture, "Error encountered during chaining to Job '{0}'", sj), se);
    }
}
```

The first method, `AddJobChainLink`, is the method we would call to set up the links. That's all we need to do to set up a chain: tell the scheduler which job should fire (secondJob) after the first job completes (firstJob). The `JobWasExecuted` method is called by the scheduler once a job has finished running. If the listener finds a link for the job that was executed, it will trigger the new job immediately.

It could come in handy if you want to link together multiple jobs, so that they execute in sequence 

###### LoggingJobHistoryPlugin
It's a listener that logs all of the job related events to the `common.logging` infrastructure. It's pretty useful if you're not sure why jobs aren't firing when you expect them to and to get a general idea of what the scheduler is doing.

The one thing that might strike you as odd is that it's also a scheduler plugin. This is so that you can add the job listener through the configuration file upon scheduler startup so that you don't miss any events. 

#### When would I use job listeners?
My favorite use for job listeners is to keep track of how long it takes a given job to run and to monitor that run time duration.

#### Implementing the IJobListener Interface
To create a job listener, we must write a class that implements the `IJobListener` interface. This interface is defined as follows:
```csharp
 public interface IJobListener
 {
    string Name { get; }
    void JobExecutionVetoed(IJobExecutionContext context);
    void JobToBeExecuted(IJobExecutionContext context);
    void JobWasExecuted(IJobExecutionContext context, JobExecutionException jobException);
 }
```
The **Name** property will hold the name of the job listener and is read-only. The rest of the interface is what is of most interest to us. These three methods give you the opportunity to interact with a job during its lifecycle. The first method, **JobExecutionVetoed** gets called whenever a trigger listener vetoes the execution of a job. If this method is called, no other methods in this interface will be called. The second method, **JobToBeExecuted**, is called right before the job is about to be executed. The third method, **JobWasExecuted**, is called after the job has been executed. You do not have to provide implementations for all 3 methods unless you want to, but the methods do need to be present in your class to satisfy the interface.

#### Code Example
In our example we will provide very simple implementations of these methods, only to illustrate how to build a listener. All the JobListenerExample does is log an informational message whenever one of the methods of the interface are called. The code for the simple listener is listed below.
```csharp
public class JobDurationListenerExample : IJobListener
{
	public void JobExecutionVetoed(IJobExecutionContext context)
	{
		// Do nothing
	}

	public void JobToBeExecuted(IJobExecutionContext context)
	{
		var stopwatch = new Stopwatch();
		stopwatch.Start();
		Console.WriteLine("Job {0} in group {1} is about to be executed", context.JobDetail.Key.Name, context.JobDetail.Key.Group);
		_Stopwatches.Add(context.FireInstanceId, stopwatch);
	}

	public void JobWasExecuted(IJobExecutionContext context, JobExecutionException jobException)
	{
		var elapsed = _Stopwatches[context.FireInstanceId].ElapsedMilliseconds;
		Console.WriteLine("Job {0} in group {1} was executed in {2}ms", context.JobDetail.Key.Name, context.JobDetail.Key.Group, elapsed);
	}

	public string Name
	{
		get { return "JobDurationListener"; }
	}
	private Dictionary<string, Stopwatch> _Stopwatches = new Dictionary<string, Stopwatch>();
}
```

First, let's look at the JobExecutionVetoed method. If you wanted to be notified if a trigger ever vetoed a job execution, then add your code here. If you need additional information about which job is running or which trigger fired, it’s all available in the context object.

Second, let's look at the JobToBeExecuted method. In this method we’re creating a stopwatch, starting it and adding it to a dictionary so that we can keep track of which job got fired. This job listener listens to all jobs, so we have to keep track of one stopwatch for each job fired. You may want to just listen to events from a given job and you should probably cleanup the stopwatches. Alternatively, you could just store the start time back in the context and then pull it out to compute the duration. Obviously this isn't production quality code, so use it wisely.

Third, let's look at the JobWasExecuted method. Here we pull out the stopwatch and then get the time it took to execute the job.

One last thing… you’ll have to give your listener a name that will be returned by the Name property.

## Trigger Listeners
Trigger listeners are used when you want to be notified of trigger level events. Triggers will raise the following events:
- A trigger has fired
- A trigger has misfired
- A trigger has completed
- Should we veto this execution?

Trigger listeners must implement the `ITriggerListener` interface.

#### Built-In Trigger Listeners
###### TriggerListenerSupport
An empty class.

###### BroadcastTriggerListener
It's another broadcasting implementation that forwards trigger events to trigger listeners that are registered with it.

#### When would I use Trigger listeners?
If you want to be able to stop a regularly scheduled job from running, you’ll need to use a trigger listener to veto that execution. Tracking misfires is useful as well to make sure your scheduler is healthy and that things are running within reason.

#### Implementing the ITriggerListener Interface
To implement a trigger listener, you need to implement the `ITriggerListener` interface.
```csharp
public interface ITriggerListener
{
    string Name { get; }
    void TriggerFired(ITrigger trigger, IJobExecutionContext context);
    bool VetoJobExecution(ITrigger trigger, IJobExecutionContext context);
    void TriggerMisfired(ITrigger trigger);
    void TriggerComplete(ITrigger trigger, IJobExecutionContext context, SchedulerInstruction triggerInstructionCode);
}
```

You’ll notice that trigger listeners get notified when the trigger is fired, when the trigger misfires and when the trigger is complete. When the VetoJobExecution method is called, the trigger listener can tell the scheduler to ignore the trigger. Essentially it vetoes the firing of the trigger.

#### Code Example
```csharp
class TriggerListenerExample : ITriggerListener
{
	public string Name
	{
		get { return "TriggerListenerExample"; }
	}

	public void TriggerComplete(ITrigger trigger, IJobExecutionContext context, SchedulerInstruction triggerInstructionCode)
	{
		Console.WriteLine("The scheduler called {0} for trigger {1}", MethodBase.GetCurrentMethod().Name, trigger.Key);
	}

	public void TriggerFired(ITrigger trigger, IJobExecutionContext context)
	{
		Console.WriteLine("The scheduler called {0} for trigger {1}", MethodBase.GetCurrentMethod().Name, trigger.Key);
	}

	public void TriggerMisfired(ITrigger trigger)
	{
		Console.WriteLine("The scheduler called {0} for trigger {1}", MethodBase.GetCurrentMethod().Name, trigger.Key);
	}

	public bool VetoJobExecution(ITrigger trigger, IJobExecutionContext context)
	{
		Console.WriteLine("The scheduler called {0} for trigger {1}", MethodBase.GetCurrentMethod().Name, trigger.Key);
		return true;
	}
}
````

First, take a look at the TriggerFired method, since it is the very first method that gets called on the trigger listener. After the TriggerFired method gets called, the VetoJobExecution gets called. You'll notice that our implementation returns true always. If this method returns false, then the JobExecutionVetoed method will get called on the job listener (if there are any) and the job will not get executed.

Next, let's look at the TriggerComplete method. This method gets called when trigger has completed, which means that it gets called after the trigger has fired and after the associated job has finished executing.

The TriggerMisfired method will get called if the trigger ever misfires, so it's a little out of place, because it's not really related to the job processing lifecycle.

Finally, let's look at the Name property. Not much excitement here, as it just returns the name of the listener.

## Loading and Executing the Listener
In order to load and execute your listener, you will have to add it to the scheduler upon start up. You must also specify whether it should executed for all jobs, or for some job in particular. We can do this using a plugin (`ISchedulerPlugin`). Finally, you must make sure that the assembly that contains your listener is available somewhere that the scheduler can load it upon startup.