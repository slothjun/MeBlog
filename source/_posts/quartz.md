---
title: quartz-初始
---
## quartz 简介
## maven坐标
## quartz 快速入门

```java
        StdSchedulerFactory stdSchedulerFactory = new StdSchedulerFactory();//创建StdSchedulerFactory工厂
        Scheduler scheduler = stdSchedulerFactory.getScheduler();

        scheduler.start();

        JobDetail job = newJob(HelloJob.class)
                .withIdentity("myJob","group1")
                .build();

        Trigger trigger = newTrigger()
                .withIdentity("myTrigger","group1")
                .startNow()
                //调度触发执行内容和时间的操作
                .withSchedule(simpleSchedule()
                    .withIntervalInSeconds(4)
                    .repeatForever())
                .build();
        scheduler.scheduleJob(job,trigger);
        while (true){}//测试用，防止主线程结束
```
## Quartz 常用接口

### Scheduler (与调度程序交)

#### 生命周期
    
    开始：SchedulerFactory 创建后开始
    结束：Scheduler调用shutdown结束

### Job (调度的内容实现定义)

### JobDetail (作业实例)

### Trigger （触发器）

#### 公共属性

    所有类型的trigger都有TriggerKey这个属性，表示trigger的身份；除此之外，trigger还有很多其它的公共属性。这些属性，在构建trigger的时候可以通过TriggerBuilder设置。

trigger的公共属性有：

jobKey属性：当trigger触发时被执行的job的身份；

startTime属性：设置trigger第一次触发的时间；该属性的值是java.util.Date类型，表示某个指定的时间点；有些类型的trigger，会在设置的startTime时立即触发，有些类型的trigger，表示其触发是在startTime之后开始生效。比如，现在是1月份，你设置了一个trigger–“在每个月的第5天执行”，然后你将startTime属性设置为4月1号，则该trigger第一次触发会是在几个月以后了(即4月5号)。

endTime属性：表示trigger失效的时间点。比如，”每月第5天执行”的trigger，如果其endTime是7月1号，则其最后一次执行时间是6月5号。

#### 优先级（priority）

如果你的trigger很多(或者Quartz线程池的工作线程太少)，Quartz可能没有足够的资源同时触发所有的trigger；这种情况下，你可能希望控制哪些trigger优先使用Quartz的工作线程，要达到该目的，可以在trigger上设置priority属性。比如，你有N个trigger需要同时触发，但只有Z个工作线程，优先级最高的Z个trigger会被首先触发。如果没有为trigger设置优先级，trigger使用默认优先级，值为5；priority属性的值可以是任意整数，正数、负数都可以。

注意：只有同时触发的trigger之间才会比较优先级。10:59触发的trigger总是在11:00触发的trigger之前执行。

注意：如果trigger是可恢复的，在恢复后再调度时，优先级与原trigger是一样的。

#### 错过触发描述（misfire instructions）

trigger还有一个重要的属性misfire；如果scheduler关闭了，或者Quartz线程池中没有可用的线程来执行job，此时持久性的trigger就会错过(miss)其触发时间，即错过触发(misfire)。不同类型的trigger，有不同的misfire机制。它们默认都使用“智能机制(smart policy)”，即根据trigger的类型和配置动态调整行为。当scheduler启动的时候，查询所有错过触发(misfire)的持久性trigger。然后根据它们各自的misfire机制更新trigger的信息。当你在项目中使用Quartz时，你应该对各种类型的trigger的misfire机制都比较熟悉，这些misfire机制在JavaDoc中有说明

#### Simple Trigger


### JobBuilder (用于构建JobDetail)

### TriggerBuilder (用于构建Trigger)

### Scheduler Job Trigger 三者的调用关系

Scheduler 调度计划，如果想要执行调度，需要将Job和Trigger放入
scheduler实例化后，可以启动(start)、暂停(stand-by)、停止(shutdown)注意：scheduler被停止后，除非重新实例化，否则不能重新启动；只有当scheduler启动后，即使处于暂停状态也不行，trigger才会被触发

Job 用来定义工作内容

Trigger用来定义调度的触发时间，以及绑定工作组

### JobDataMap的使用

1.Job类
```java
public class DumbJob implements Job {

    public DumbJob() {
    }

    public void execute(JobExecutionContext context)
      throws JobExecutionException
    {
      JobKey key = context.getJobDetail().getKey();

      JobDataMap dataMap = context.getJobDetail().getJobDataMap();

      String jobSays = dataMap.getString("jobSays");
      float myFloatValue = dataMap.getFloat("myFloatValue");

      System.err.println("Instance " + key + " of DumbJob says: " + jobSays + ", and val is: " + myFloatValue);
    }
  }
```
2.调度定义时
```java
  // define the job and tie it to our DumbJob class
  JobDetail job = newJob(DumbJob.class)
      .withIdentity("myJob", "group1") // name "myJob", group "group1"
      .usingJobData("jobSays", "Hello World!")
      .usingJobData("myFloatValue", 3.141f)
      .build();
```
#### 自动填充jobDataMap参数

只需要修改Job类，在类中添加map相对应的字段提供set方法即可。如下

```java
public class DumbJob implements Job {


    String jobSays;
    float myFloatValue;
    ArrayList state;

    public DumbJob() {
    }

    public void execute(JobExecutionContext context)
      throws JobExecutionException
    {
      JobKey key = context.getJobDetail().getKey();

      JobDataMap dataMap = context.getMergedJobDataMap();  // Note the difference from the previous example

      state.add(new Date());

      System.err.println("Instance " + key + " of DumbJob says: " + jobSays + ", and val is: " + myFloatValue);
    }

    public void setJobSays(String jobSays) {
      this.jobSays = jobSays;
    }

    public void setMyFloatValue(float myFloatValue) {
      myFloatValue = myFloatValue;
    }

    public void setState(ArrayList state) {
      state = state;
    }

  }
```
## @PersistJobDataAfterExecution

由于Job类的创建是由Scheduler上的JobFactory 运行时动态创建的，所以个人猜测框架创作人使用了动态代理的技术。所以Job的成员变量与外界的交互就成为了难题。如何实现呢？答案就是在实现Job的类上加入@PersistJobDataAfterExecution注解，这样JobDataMap中的数据就可以与外界交互了。

## @DisallowConcurrentExecution

    个人目前还没有使用过，w3cschool的官方文档上是这样介绍的：
```text
@DisallowConcurrentExecution：将该注解加到job类上，告诉Quartz不要并发地执行同一个job定义（这里指特定的job类）的多个实例
```