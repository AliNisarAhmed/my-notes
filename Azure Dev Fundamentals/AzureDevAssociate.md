## 1 - Azure Functions

Business processes modeled in software are often called **workflows**. Azure includes four different technologies that you can use to build and implement workflows that integrate multiple systems:

The first two are **Design-First** Technologies, while the latter are **Code-first**.

- Logic Apps
  - for devs and IT Pros
  - GUI based
  - can also edit workflows in JSON
  - 200 connectors are available
  - A connector is a Logic Apps component that provides an interface to an external service.
  - For example, the Twitter connector
- Microsoft Power Automate

  - for office workers and business analysts
  - Build on top of logic app
  - fully GUI based
  - handles the following four different types of flows
    - Automated: A flow that is started by a trigger from some event. For example, the event could be the arrival of a new tweet or a new file being uploaded.
    - Button: Use a button flow to run a repetitive task with a single click from your mobile device.
    - Scheduled: A flow that executes on a regular basis such as once a week, on a specific date, or after 10 hours.
    - Business process: A flow that models a business process such as the stock ordering process or the complaints procedure.

- WebJobs

  - WebJobs are a part of the Azure App Service that you can use to run a program or script automatically. There are two kinds of WebJob:
  - Continuous. These WebJobs run in a continuous loop. For example, you could use a continuous WebJob to check a shared folder for a new photo.
  - Triggered. These WebJobs run when you manually start them or on a schedule.
  - You may choose WebJobs for the following reasons:
    - You want the code to be a part of an existing App Service application and to be managed as part of that application, for example in the same Azure DevOps environment.
    - You need close control over the object that listens for events that trigger the code. This object in question is the `JobHost` class, and you have more flexibility to modify its behavior in WebJobs.

- Azure Functions
  - The diff with WebJobs is that they do not provide close control of the `JobHost` class
  - `JobHost` class is the object that listens for events that trigger the code

**Notes**:

1. Microsoft Power Automate permit the creative team, who are not developers, to manage a process
2. WebJobs are the only technology that permits developers to control retry policies. So, they are useful in uncertain conditions where we want to ensure success.
3. Azure Logic Apps is the only one of the four technologies that provides a design-first approach intended for developers.
4. Webjobs can be made part of the existing app service, but...
5. Its not necessary to write a WebJob as part of the existing Azure App Service application, because you want to manage the procedure code separately.

### Azure Functions

- Azure Functions are Event triggered. The type of event that starts the function is called a **trigger**.

- **Note:** Every Azure Function must have exactly one trigger associated with it. If you want to use multiple triggers, you must create multiple functions.

#### Bindings

Bindings are a declarative way to connect data and services to your function. Bindings know how to talk to different services, which means you don't have to write code in your function to connect to data sources and manage connections.

#### Timer Trigger

- A timer trigger is a trigger that executes a function at a consistent interval. To create a timer trigger, you need to supply two pieces of information.

  - A Timestamp parameter name, which is simply an identifier to access the trigger in code.
  - A Schedule, which is a CRON expression that sets the interval for the timer.

##### CRON Expression

- A CRON expression is a string that consists of six fields that represent a set of times.

- The order of the six fields in Azure is: `{second}` `{minute}` `{hour}` `{day}` `{month}` `{day of the week}`.

- For example: A Trigger that executes every five minutes `"0 */5 * * * *"`

  - `*` Selects every value in a field. An asterisk `"*"` in the day of the week field means every day.
  - `,` Separates items in a list. A comma "1,3" in the day of the week field means just Mondays (day 1) and Wednesdays (day 3).
  - `-` Specifies a range. A hyphen "10-12" in the hour field means a range that includes the hours 10, 11, and 12.
  - `/` Specifies an increment A slash `"/10"` in the minutes field means an increment of every 10 minutes.

#### Http Trigger

- An HTTP trigger Authorization level is a flag that indicates if an incoming HTTP request needs an API key for authentication reasons.

- There are three Authorization levels:

  - Function
  - Anonymous
  - Admin

The Function and Admin levels are "key" based. To send an HTTP request, you must supply a key for authentication.

There are two types of keys: function and host. The difference between the two keys is their scope. Function keys are specific to a function.

Host keys apply to all functions inside the function app. If your Authorization level is set to Function, you can use either a function or a host key. If your Authorization level is set to Admin, you must supply a host key.

The Anonymous level means that there's no authentication required.
