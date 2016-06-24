# Triggers in Quartz.Net

Triggers determine WHEN a job executes. A job can have zero or more triggers. A job with no triggers does not execute. A trigger can only be assigned to one job. A trigger is uniquely identified by its `name` and `group` properties. Names must be unique within a trigger group and name/group combinations must be unique within a Quartz.Net scheduler. You can use the `TriggerKey` object to uniquely identify a trigger, which is basically a container that holds the trigger name and trigger group in one object. 

## Built-in Triggers
**CalendarIntervalTrigger**: This trigger is used to fire jobs based on a recurring calendar time interval.

**CronTrigger**

**DailyTimeIntervalTrigger**

**SimpleTrigger**

## Calendars in Quartz.Net
Calendars are used to exclude periods of time during which a trigger would normally fire. Let’s say for example that we have a job that should run every workday, but not during holidays. To schedule this job we could use a CronTrigger to tell it to run Monday through Friday. To exclude holidays, we would then add a calendar to the trigger, specifying the holidays to exclude. This combination of a trigger and a calendar would produce the schedule we want.

### Built-in Calendars
**AnnualCalendar**: With the annual calendar you can exclude a given set of days every year. This calendar is useful for excluding holidays that always fall on the same date every year.

**CronCalendar** uses a cron expression to define the times to be excluded.

**DailyCalendar** lets you exclude or include a time range each day. To configure this calendar, you specify the time range that you want to exclude. It’s also possible to invert the time range, by setting the `InvertTimeRange` property to true.

**HolidayCalendar** contains a list of all the days that should be excluded from triggering a job. If you use this calendar, you must provide a full list of all of the days that must be excluded. Only full days can be excluded by using this calendar.

**MonthlyCalendar** can be used to exclude a given set of days each month. This calendar lets you create exclusion schedules such as don’t run on the 1st and the 15th of every month.

**WeeklyCalendar** can be used to exclude a given set of days each week. This calendar lets you create exclusion schedules such as don’t run on the weekends, or don’t run on Mondays, for example.You set which days are excluded by adding `System.DayOfWeek` values to the calendar.