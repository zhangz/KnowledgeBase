# Quartz.Net Plug-in
What are plugins used for in Quartz.Net 2.0? Well, plugins come in handy if there is something you want to do whenever the scheduler is started or stopped. Some examples of handy plugins are: 
- plugins to load jobs into a RAM jobstore upon scheduler startup (this one is available out of the box)
- plugins to register listeners (listeners must be registered with the scheduler upon startup since they are not persisted)

In order to implement a listener in Quartz.Net 2.0 there are several steps we need to take. First we must implement the `ISchedulerPlugin` interface and then we must configure the scheduler to load our plugin. Let’s get started.

#### Built-In Plug-ins
- JobInitializationPlugin
- LoggingJobHistoryPlugin
- LoggingTriggerHistoryPlugin
- ShutdownHookPlugin

#### Implementing the ISchedulerPlugin Interface
In order to create your own plugin, you have to implement the ISchedulerPlugin interface. Here is the ISchedulerPlugin interface definition:
```csharp
 public interface ISchedulerPlugin
 {
    void Initialize(string pluginName, IScheduler sched);
    void Start();
    void Shutdown();
 }
```

Let's take some time to look at these methods in a little more detail.

###### Initialize
The `Initialize` method is called before the scheduler is started and it gives your plugin a chance to initialize itself. Keep in mind that at this point the scheduler itself is not running nor fully initialized. Here is what the code comments have to say: 
> Called during creation of the IScheduler in order to give the ISchedulerPlugin a chance to Initialize. At this point, the Scheduler's IJobStore is not yet (initialized?). If you need direct access (to) your plugin, you can have it explicitly put a reference to itself in the IScheduler’s SchedulerContext (in the) Initialize(string, IScheduler) method.

###### Start
The Start method is probably the method where most of the logic for your plugin will go. I will let the method’s comments describe the method’s purpose: 
> Called when the associated IScheduler is started, in order to let the plugin know it can now make calls into the scheduler if it needs to.

###### Shutdown
The shutdown method is there to allow you to do any cleanup that might be necessary for your plugin. The code comments are as follows:
> Called in order to inform the ISchedulerPlugin that it should free up all of it's resources because the scheduler is shutting down.

#### Creating the Plug-In
Now we will go ahead and actually implement the plugin. In our sample implementation all that our plugin will do is to register a job listener.
```csharp
 public class PluginExample : ISchedulerPlugin
 {
    public void Initialize(string pluginName, IScheduler sched)
    {
        sched.ListenerManager.AddJobListener(new JobDurationListenerExample(), EverythingMatcher<JobKey>.AllJobs());
		sched.ListenerManager.AddTriggerListener(new TriggerListenerExample(), EverythingMatcher<JobKey>.AllTriggers());
		sched.ListenerManager.AddSchedulerListener(new SchedulerListenerExample());
    }

    public void Shutdown()
    {
        //Do Nothing
    }

    public void Start()
    {
        //Do Nothing
    }
 }
```

#### Configuring the Scheduler to Load the Plugin
Now that we have created our plugin, the next step is to tell the scheduler to load it. This is done by adding this line to our properties file : 
```
quartz.plugin.myplugin.type = Examples.PluginExample, Examples 
```
The plugin property needs to be structured like this: 
```
quartz.plugin.{name}.type = {type name}, {assembly name} 
```
Let's discuss the components of the property. First, the **quartz.plugin** part of the property tells the scheduler that this is a plugin that needs to be loaded. The **{name}** part is the name that you want to give to your plug-in, and is the name that is passed in to the **Initialize** method above. The value of the property **{type name}**, **{assembly name}** tells the scheduler the **Type** of the plug-in, so that it can be loaded into memory. **{type name}** is the full name of your plugin, which in our case would be Examples.PluginExample. **{assembly name}** is the name of your assembly file, minus the .dll extension. Here’s another example of a plugin configuration property, lifted from the Quartz.Net default configuration file: 
```
quartz.plugin.xml.type = Quartz.Plugin.Xml.XMLSchedulingDataProcessorPlugin, Quartz
```

#### Final Step
Now that your plugin has been added to the configuration, all you need to do is start the scheduler and check the log for errors. Remember that the dll that contains your plugin needs to be in the same folder as the Quartz.Net binaries so that it can be found by the .Net framework and loaded into memory.
