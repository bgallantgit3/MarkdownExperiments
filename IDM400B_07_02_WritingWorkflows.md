---
layout: lesson
---

## Deploying and Creating a Workflow

![](IDM400B_07_02_WritingWorkflows/image1.png)

#### Lesson objectives

![](IDM400B_07_02_WritingWorkflows/image2.png)

### Describe the structure of workflow files

![](IDM400B_07_02_WritingWorkflows/image3.png)

#### The .bar file

![](IDM400B_07_02_WritingWorkflows/image4.png)

The extension `.bar` stands for business archive. The suffix is likely from the words business archive, but the file is basically a ZIP archive.

The `.bar` archives contain all files that are needed for running a BPMN process in a BPMN 2.0 workflow engine.

The `.bar` archives have exactly the same format as a `.zip` archive and can therefore be extracted with any ZIP compatible tool.

In Activiti, the Java classes need to be available on the Java `classpath`. Therefore, they usually need to be extracted from the `.bar` file and stored in a library directory. In case of IDM, they also need to be made OSGi-compatible.

#### The .bpmn20.xml file

![](IDM400B_07_02_WritingWorkflows/image5.png)

The `bpmn20` files are in XML format and have two elements on the top level (only the first one is mandatory):

- Process, containing the BPMN process definition
- BPMN diagram, containing an XML formatted representation of the workflow flow diagram

#### BPMN diagram

![](IDM400B_07_02_WritingWorkflows/image6.png)

The slide shows a typical BPMN diagram. The elements of the diagram shown are:

* Start event (which contains a start up form, which is not visible in the diagram)
* Script task (to prepare variables, labelled Prepare Task in the diagram)
* User task (for manual interaction, labelled Approve Contractor in the diagram)
* Gateway (to split the workflow, the X in the diagram)
* Script task (to send a deny notice, if not approved)
* Script task (to create the new user, if approved)
* Service task (to read the new user object for testing)
* Script task (to send an accept notification)

#### bpmn20.xml example I

![](IDM400B_07_02_WritingWorkflows/image7.png)

The slide shows an abbreviated example of a `bpmn20.xml` file.

It includes rudimentary event elements:
* `startEvent`
* `scriptTask`
* `serviceTask`
* `userTask`

#### bpmn20.xml example II

![](IDM400B_07_02_WritingWorkflows/image8.png)

The slide shows the continuation of the previous slide example.

The elements shown in this part are:
* `endEvent`
* `sequenceFlow`
* `conditionExpression`
* `bpmndi`, i.e. BPMN diagram

#### bpmn20.xml: events I

![](IDM400B_07_02_WritingWorkflows/image9.png)

Start events, as the name indicates, are usually at the beginning of a new process. There are different types of start events available in BPMN:

- None start event: Used when the process is supposed to be started through an API call. There is no catching event available.
- Timer start event: The process instance will be created at a given time. The starting time is set at deployment time.
- Message start event: The process is instantiated on arrival of a named message and the event can be depending on the messages name, i.e. different messages start different processes.
- Error start event: A sub-process is initiated on a certain error. Error start events are for sub-processes only!

See also <http://www.activiti.org/userguide/#bpmnStartEvents>

#### bpmn20.xml: events III

![](IDM400B_07_02_WritingWorkflows/image10.png)

Intermediate catching events always need an `EventDefinition` such as a `timerEventDefinition` or a `signalEventDefinition`. Depending on the event definition, it works as follows:

- `timerEvent`: Works like a stop watch
- `signalEvent`: Triggered on a signal
- `messageEvent`: Catches a named message

#### bpmn20.xml: sequenceFlow

![](IDM400B_07_02_WritingWorkflows/image11.png)

A sequence flow simply connects two events, or describes how to continue after an event or task is finished.

A sequence flow can have a condition. Conditions only make sense if there is more than one sequence flow from an event or task.

If an event has more than one outgoing sequence flow, then the engine will take each path where the sequence flow is either unconditional or the condition is `true`.

If more specific continuation is needed, then a gateway of a certain type is needed for the connection.

#### bpmn20.xml: gateways

![](IDM400B_07_02_WritingWorkflows/image12.png)

Gateways control the sequence flow based on conditions or events. The following are the types of gateways:

- Normal Gateway: A normal gateway acts the same way as an event or task; all unconditional flows or flows with a condition that is true will be taken.
- Exclusive Gateway: The first flow which is either unconditional or where the condition is true will be taken, no other. Flows are tested in the order they appear.
- Parallel Gateway: It does not evaluate the conditions! All flows will be taken. Same on the input side; it will wait until all arriving flows have come in!
- Inclusive Gateway: All unconditional flows, or flows with a condition that is true, will be taken on the outgoing side. On the incoming side it will only wait for unconditional flows or flows with a true condition.
- Event-based Gateway: Only flows where an event happens will be taken. An event can be a timer or a user action. Whatever event occurs first will make the process continue.

#### bpmn20.xml: sub-processes

![](IDM400B_07_02_WritingWorkflows/image13.png)

Sub-processes are part of the same XML as the main process. They cannot be called from any other process, only from the process in which they are embedded.

Sub-processes are used to group tasks together for better visualization. They can be collapsed or expanded on the graphical view.

Sub-processes also create their own scope for instance for boundary events. They can only be started with their start event and controlled at an end event.

Sequence flows cant cross sub-process boundaries. Sub-processes have the same variable context as their main process.

#### bpmn20.xml: call activity

![](IDM400B_07_02_WritingWorkflows/image14.png)

Processes can call any other process at any time, except the sub-processes, which are embedded in other processes. The mechanism to call another process is called a Call Activity.

In this way, complex processes can be modularized and different main processes can share other processes for reuse. In contrast to sub-process, processes called by a Call Activity do not share the same variable scope. Variables have to be handed over when calling another process.

#### bpmn20.xml: boundary event

![](IDM400B_07_02_WritingWorkflows/image15.png)

Boundary events are attached to another event or sub-process and catch the appearance of a trigger. For instance, a timer boundary event would wait for a certain amount of time. If the process execution still sits at the same event when the timer goes off, then the boundary event would take control.

This usually makes the process take a different path.

#### bpmn20.xml: tasks

![](IDM400B_07_02_WritingWorkflows/image16.png)

User task: Tasks with a manual interaction by a user. Process execution waits until one of the subjects has taken an action and makes the process continue. A subject can be an individual user, a list of users, or a list of groups in which the users must be member to be one of the subjects who can claim the task.

Script task: Is a task containing a script. The script can be written in any JSR-223-supported language. By default only Groovy is shipped with Activiti. IDM also currently only supports Groovy. Still, it must be stated that a script is a Groovy script.

Service task: Will call a Java class that must be available on the `classpath` of the server. If Activit is used as an embedded service in IDM, then the class must also be OSGi-compatible.

Email task: This is a very convenient way to send emails from an Activiti workflow. An SMTP server must be configured in the `workflow.json` when used with an embedded IDM server.

Execution Listener task: Allowed to execute Java code when an event occurs during the process execution. Events can be:

- The start of a process instance
- The end of a process instance
- A certain transition is taken
- The end of a certain activity
- The start of a certain activity

#### User task example I

![](IDM400B_07_02_WritingWorkflows/image17.png)

#### User task with initiator as owner

![](IDM400B_07_02_WritingWorkflows/image18.png)

Since the simple workflow does not do anything, you need to add logic to it.

In the exercise, you do that in the BPMN file.

Here you identify an Activiti initiator, which in this case is the Start User.
It then specifies an assignee, referring to the Start User.
It is also possible to directly hard code an assignee or initiator by entering a valid username for the `id`.

#### User task: adding form fields

![](IDM400B_07_02_WritingWorkflows/image19.png)

Activiti has a nice interface for creating interactive forms and that is covered later in this lesson. Here you specify the form with its elements and properties within `extensionElement` tags.

#### Script task example

![](IDM400B_07_02_WritingWorkflows/image20.png)

The slide shows a `scriptTask` that will create a user in the IDM repository.

The user data is first constructed as a JSON object and stored in a variable called `user`:

```javascript
def user = [ 'userName':'daduck',
 	'givenName':'Donald',
 	'familyName':'Duck1',
 	'email':['daduck@example.com']
 ]
```

Then, the `openidm.create` method is called with the target object `managed/user/<id of the user>` and the user object:

```javascript
openidm.create('managed/user/' + _id, user)
```

#### Using the IDM API

![](IDM400B_07_02_WritingWorkflows/image21.png)

The `openidm` context is available in any Activiti workflow when started on IDM (local workflow engine) or configured for use with IDM (remote workflow engine). IDM objects like managed objects in the repository or system objects on external resources can be managed using the `openidm` context.

#### bpmn20.xml: thread execution

![](IDM400B_07_02_WritingWorkflows/image22.png)

A process instance, after started, will continuously execute until a certain event occurs that puts it into a wait state. Such an event can be a user task, or a timeout task. At this moment, the execution state will be persisted in the database.

The process will continue on the next trigger, such as when a user has interacted and made a user event continue, or when a timer is ended. Persisting the process state and continuation in a new thread can be forced by using the `activit:async=true` extension.

### Describe how to model workflows

![](IDM400B_07_02_WritingWorkflows/image23.png)

#### Tools

![](IDM400B_07_02_WritingWorkflows/image24.png)

A BPMN workflow developer can simply use any text editor that is able to write XML-compliant files. Or use some BPMN or even better Activiti specific tools.

For Activiti there is:
* The Activiti Designer
* Activiti Process modeler

Activiti Designer:
* Is a plugin for Eclipse

Activiti Process modeler:
* Is part of the Activiti Explorer. The Activiti explorer is a complete development and test environment for writing Activiti processes. It can be downloaded and deployed on a web container, like Tomcat.

The Activiti Explorer contains:
* The Activiti workflow engine
* A test environment including the needed DB for Activiti
* The Activiti Modeler to write, edit, and deploy Activiti processes

This course uses the Activiti Modeler in the lab, but the resulting workflows cannot be tested in the Activiti engine of the same installation as it is missing the IDM API, like `openidm.create`.
Therefore, the process definitions need to be exported and deployed on an Activiti engine that runs within IDM. The IDM engine will also need some configurations according to the classes use case, like the roles for the Owners and Family and Friends.

#### Activiti Designer

![](IDM400B_07_02_WritingWorkflows/image25.png)

The Activiti designer is a plugin for the Eclipse IDE. It can be downloaded at <http://activiti.org/designer/update/plugins>.

With the designer, it is also possible to do some graphical workflow design. The designer can make use of the eclipse XML editor and it is closely associated with all the Java development tools of Eclipse. That makes it the preferred tool for developers.

#### Getting started with Activiti Designer

![](IDM400B_07_02_WritingWorkflows/image26.png)

Activiti is a lightweight workflow and BPMN platform targeted at business people, developers, and system admins.
Its core is a super-fast and rock-solid BPMN 2 process engine for Java. It's open source and distributed under the Apache license.
Activiti excels at designing workflows and saving projects in BPMN-compliant files. It's easy to design basic or interactive workflows and add features and logic, either with the Activiti Designer or by editing the BPMN files. In this lab, you use the Activiti Designer and make changes directly in the BMMN files.

Note that IDM embeds an Activiti Process Engine that is started in the IDM OSGi container. IDM does not include the Activiti Designer or any other BPMN-specific tool for editing workflow. The Activiti Designer is only one method you can use to edit workflow.

#### Step-by-Step: your first Activiti workflow

![](IDM400B_07_02_WritingWorkflows/image27.png)

A very popular development environment for developing workflows with Activiti is to use the Eclipse IDE and the Activiti Designer plugin. This is the environment used in this class.

To start Eclipse on the lab environment, select Applications, then Programming, then Eclipse, and follow the instructions on the slide to start Activiti Designer.

#### Simple Activiti diagram

![](IDM400B_07_02_WritingWorkflows/image28.png)

In this exercise, you create the most simple possible Activiti workflow and deploy it to IDM. The goal is simply to get you working with Activiti Designer and to go through the process of creating the BAR file and deploying it to IDM. This workflow appears as a valid process in IDM and users can start it, but it doesn't do anything.

In subsequent exercises, you add functionality and logic to this starting workflow.

#### Activiti Explorer I

![](IDM400B_07_02_WritingWorkflows/image29.png)

The Activiti Explorer is deployed on a web container like Tomcat, and its interface can easily be accessed with a browser by going to the application's URL. In this example, the browser is on the same host as the web container and you can access the Activiti Explorer UI using the following URL: <http://localhost:8080/activiti-explorer>.

Activiti Explorer prompts users to log in. A default installation of Activiti Explorer has a user called `kermit` (password: `kermit`) which serves as the administrator.

#### Activiti-Explorer II

![](IDM400B_07_02_WritingWorkflows/image30.png)

Here is a list of typical tasks and their ordering when developing and testing workflows:

In the modeler:
* Create a new model
* Edit the model
* Export the model to a file
* Add XHTML forms

On the command line:
* Zip it to a `.bar` file
* Copy the `.bar` file to IDMs `workflow` folder

In IDM:
* Test the workflow

### Describe how to use forms in workflows

![](IDM400B_07_02_WritingWorkflows/image31.png)

#### userTask

![](IDM400B_07_02_WritingWorkflows/image32.png)

A `userTask` can include manual interactions during the lifecycle of a process. Workflows are an easy way in IDM to include users feedback while running a process.

When the execution of a process instance processes a `userTask` element, it will create a task object with on or more users assigned for the task, and both the task and the project instance will be stored in the DB and wait for the assignee to call the task. While the task is persisted in the DB it will not use any resources in the IDM process.

A `userTask` usually has some interface elements for the assigned user to interact with the task. By default, these elements will be show in the UI of IDM with standard style instructions.

More influence on the style can be achieved by bundling an XHTML form with the task, either by having the elements defined in the `userTask` definition (not recommended due to a difficult mix of SML and HTML elements in the same file; such as all HTML extra characters need to be escaped).

A better way is to reference a file that needs to be bundled with the process definition and contains all the style instructions of the UI.

#### userTask example

![](IDM400B_07_02_WritingWorkflows/image33.png)

The slide shows an excerpt of a process definition with a `userTask`. Like any element, it can have an id (decideApprovalTask here) and a name (Approve Contractor here).
In addition,  each `userTask` needs to have an assigned user, list of users, or list of groups (a list of roles in the case of the IDM implementation):
* `assignee`
* `candidateUsers`
* `candidateGroups`

The `extensionElements` part contains a list of `activiti:formPropertiy` elements. One for each form field.

Extra style sheet information can be added by referencing an XHTML form file. `activiti:formKey="contractorForm.xhtml;` see next slide

#### userTask with XHTML forms

![](IDM400B_07_02_WritingWorkflows/image34.png)

The interface that is shown to the user who manually interacts with the process through a `userTask` can be styled by an extra file, called the `formKey`. It needs to be referenced in the `userTask` element by `activiti:formKey=contractorForm.xhtml`.

#### XHTML form example

![](IDM400B_07_02_WritingWorkflows/image35.png)

The XHTML form files can define their own style. Each form field will be displayed by `div` referencing the class, id, and validation references.

The label element references the name of the form variable and a localizable name for the label.

In addition, a validation message can be defined.

### Examine, deploy, and start a workflow to let subscribers create family and friend accounts

![](IDM400B_07_02_WritingWorkflows/image36.png)

This section is an introduction into the corresponding exercise for this lesson.

#### Examine the CreateSubSubscriber workflow

![](IDM400B_07_02_WritingWorkflows/image37.png)

The lab VM has a pre-built workflow process that you deploy to the `workflow` folder and test in IDM. However, before you test the workflow, you will launch the Activiti Explorer web application that is running in Tomcat to examine the details of the workflow.

The slide shows a screen shot of the web application with the workflow loaded and converted for editing. You can use the web application to explore the details of the workflow process without having to touch the corresponding workflow files.

#### Deploy the CreateSubSubscriber workflow

![](IDM400B_07_02_WritingWorkflows/image38.png)

Because the workflow is already provided for you, all you need to do is copy the BAR file to the project's `workflow` folder and it will automatically be deployed by IDM. As always, you should monitor the Felix console or IDM server log for any errors deploying the workflow.

#### Start the CreateSubSubscriber workflow

![](IDM400B_07_02_WritingWorkflows/image39.png)

After you deploy the workflow, then you test the workflow by:

1. Launching the workflow from the end user perspective.
2. Monitoring the activity as the IDM administrator from the Admin UI.
3. Completing the workflow from the end user perspective.
4. Monitoring the completion of the workflow from the Admin UI.

### Create and deploy a simple workflow using Activiti Explorer

![](IDM400B_07_02_WritingWorkflows/image40.png)

This section is an introduction into the corresponding exercise for this lesson.

#### Create a simple workflow process

![](IDM400B_07_02_WritingWorkflows/image41.png)

In the final exercise of the course, you use the Activiti Explorer to create a simple workflow process. You save the process and then export it from Activiti Explorer and deploy it to the IDM `workflow` folder.

#### Deploy a simple workflow process

![](IDM400B_07_02_WritingWorkflows/image42.png)

The export from Activiti Explorer is an BPMN 2.0 XML file that you copy over to the IDM instance's `workflow` folder. IDM then automatically deploys the workflow. You can then test the workflow from the Self-Service UI.

#### Lab exercises

![](IDM400B_07_02_WritingWorkflows/image43.png)

The slide lists the exercises to perform in this lesson. Please see the corresponding lab in the Student Workbook.

#### Deploying and Creating a Workflow Process

![](IDM400B_07_02_WritingWorkflows/image44.png)
