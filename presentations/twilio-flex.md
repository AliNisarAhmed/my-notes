

### Twilio flex

- Attribute based Routing using TaskRouter
    - Worker / Agents
    - Admins
    - Task
    - Task Queues
    - Workflows
    - Twilio Flex UI

- Other things 
    - Studio
        - Allows doing pre-agent processing to tasks, like Automated call responses, etc.
        - example: when a customer sms's a dealer for the first time, we can ask the customer to enter their details like name, location, issue etc. while they wait for the agent to connect
    - Twilio Functions
    - Proxy Service (public beta)

***

### Features

- Dealer Isolation
- Dealer Separate Workflows (WF) and Task Queus (TQ)
- Separate sms numbers for dealers, each with its own WF/TQ
- Separate fb pages for dealers, each with its own WF/TQ
- Routing after task timeout and reservation timeout to H2H
- H2H transfers task back to the queue/Worker 
- Separate Leads Workflow/TQ for Leads, per dealer
- On Leads arrival, agent can initiate sms and phone call with the customer (fb can also work in theory, but does not work right now)
- Call Recording

***

### SMS 

- SMS by the customer on the dealer's number
    - Task routed to available agent using appropriate Workflow/TQ
    - When the Agent accepts the task, CRM loads customer's profile (if profile not found, it loads the search screen)

- Agent Timeout 
    - Happens when a task cannot find a valid worker in the TQ

- Reservation Time 
    - Happens when the agent takes too long to accept the task

- In both cases, task moves to H2H Task Queue
    - These Agent/Reservation timeout rules are described when setting up workflows (SHOW)
    - H2H agents can see where the Task originated from, and can then handle it, or transfer it back to the agent/queue where it came from.
    - The transfer task feature is using Twilio function deployed.

The same H2H fallback TQ is implemented for FB messages as well.

***

### Leads

A Task is created when a Lead is generated

- When an agent accepts a lead Task, we load the lead on the CRM
- Agent can start sms chat with the customer, or can even call them
- Facebook outgoing messaging can also work (currently does not work)
- Uses another deployed Twilio function underneath

***

### Issues faced

- Scalability (could be averted with scripting the JSOn configuration)
- Reliability (sometimes the messages would get lost and tasks would not be created, have to do )
- PITB development (documentation is good, but not great, does not cover every scenario & leaves no option but to contact their support (which was pretty responsive))
- Some Twilio stuff is in beta, e.g. FB integration, Proxy service
- Lead Generation to Task Creation flow could happen on a Twilio function for ease of development.
- On Flex UI, flex's built in components are hard to tinker/modify
- Cross Channel History (there could be ways to make this work, but there is nothing built-in in twilio)

