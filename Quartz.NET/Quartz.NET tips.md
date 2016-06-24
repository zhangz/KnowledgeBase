## How to implement IStatefulJob in Quartz.Net 2.0
You just need to mark your IJob with the new `DisallowConcurrentExecutionAttribute`. Any jobs that have this attribute set are only allowed to run one instance of the job at a time. Keep in mind that an instance is defined by a JobKey (job name and group combination), which means that if you have the same job type scheduled with a different job name or a different job group, you may get more than one instance of that type of job running at the same time.

Seems simple enough so far? Well, it is! There is only one other thing to consider. If you want the scheduler to persist the job’s state between executions, you must also add the `PersistJobDataAfterExecutionAttribute` to the class. This attribute tells the scheduler that you would like for it store the job’s JobDataMap once it is finished executing. 

One Last Thing:
While you can use each of these attributes separately, if you are going to use the `PersistJobDataAfterExecutionAttribute` to persist your job’s state, you should always use the `DisallowConcurrentExecutionAttribute`, because if you don’t, you might find that your job state is corrupted due to race conditions.

## Checking Your Cron Expression’s Schedule
Quartz.Net relies on a class called `CronExpression` to generate the schedule defined by the string cron expression you pass to your trigger. That class is accessible to you, so you could write code that looks like this to generate the next ten fire times:
```csharp
public List<DateTimeOffset> Schedule(string cronExpression)
{
    CronExpression expresion = new CronExpression(cronExpression);
    var lastTime = DateTimeOffset.Now;
    var times = new List<DateTimeOffset>();
    for (int i = 0; i < 10; i++)
    {
        var nextTime = expresion.GetNextValidTimeAfter(lastTime);
        if (nextTime.HasValue)
        {
            times.Add(nextTime.Value);
            lastTime = nextTime.Value;
        }
        else
        {
            break;
        }
        }
    return times;
}
```