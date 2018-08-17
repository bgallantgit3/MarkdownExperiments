# Chapter 1: Basic Provisioning

---

## Performing Basic User Provisioning

> The main goal of this lesson is to introduce IDM and describe how you can quickly perform basic user provisioning between an external resource and IDM using the samples that ship with IDM.

Note:

The lesson introduces basic concepts around IDM user provisioning and how you can easily download, install, and run IDM to demonstrate basic user provisioning. In this lesson, you learn IDM concepts such as installing and running IDM, logging in to the administrative and end user interfaces, and provisioning accounts from an external resource into the IDM repository using connectors, mappings, and managed user objects.

---

#### Lesson objectives

Upon completion of this lesson, you should be able to:

- Install and start IDM for the first time
- Start IDM with the XML sample configuration and run the sample
- Start IDM with the LDAP sample configuration and run the sample
- A new section goes here

---

### A New slide

> my quote

---

### Install and start IDM for the first time

This first section describes how to install and start IDM for the first time.

---

#### TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE

![testimagesize](testimagesize900x600.png)

---

#### 2 TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE TITLE

![testimagesize](testimagesize670x440.png)

---

#### Try a LucidChart PNG 1

![LucidChart](LucidChartsTest1.png)

---

#### Try a LucidChart PNG 2

![LucidChart](LucidChartsTest2.png)

---

#### Download, unzip, and start IDM

![LucidChart](LucidChartsTest1.png)

Note:

Installing IDM simply involves downloading the software zip file and unzipping the file to an empty folder.

For example, you can download and unzip either the released version of IDM or the evaluation build of IDM depending on your needs. For example, whether you are an existing customer with an appropriate license and account, or an evaluator who is interested in evaluating one or more ForgeRock products.

The student workstation in the instructor assigned lab environment already has the IDM evaluation zip file downloaded. In the first lab, you unzip the downloaded file to the `/opt/projects/` folder and test the samples in this `openidm` instance. In subsequent labs, you install IDM into other folders to keep your work separated. For example, in the next lab you install IDM in the `/opt/projects/enterprise` folder. This allows you to reference earlier work you have done.

The contents of the zip file are extracted to the `openidm` directory. To start IDM, simply change to the `openidm` directory and run the startup script.

IDM runs within an [OSGi Container](http://www.osgi.org/Technology/WhyOSGi). The current release of IDM is supported on the [Apache Felix](http://felix.apache.org/site/index.html) OSGi Container and is included with the installation of IDM.

Output messages are displayed on the Felix console, which is part of the Apache release. The console is useful for displaying errors that might occur during the configuration and operation of IDM. Note that messages are also logged to the IDM server log file.

Finally, the container runs within a Java Virtual Machine (JVM), which can run on most Operating Systems that support Java.

Note that the platform must be a supported Operating System and already have Java installed for running a Java Virtual Machine (JVM).

---

#### Download, unzip, and start IDM (TEST 2)

![LucidChart](LucidChartsTest1.png)

Note:

Installing IDM simply involves downloading the software zip file and unzipping the file to an empty folder.

For example, you can download and unzip either the released version of IDM or the evaluation build of IDM depending on your needs. For example, whether you are an existing customer with an appropriate license and account, or an evaluator who is interested in evaluating one or more ForgeRock products.

The student workstation in the instructor assigned lab environment already has the IDM evaluation zip file downloaded. In the first lab, you unzip the downloaded file to the `/opt/projects/` folder and test the samples in this `openidm` instance. In subsequent labs, you install IDM into other folders to keep your work separated. For example, in the next lab you install IDM in the `/opt/projects/enterprise` folder. This allows you to reference earlier work you have done.

The contents of the zip file are extracted to the `openidm` directory. To start IDM, simply change to the `openidm` directory and run the startup script.

IDM runs within an [OSGi Container](http://www.osgi.org/Technology/WhyOSGi). The current release of IDM is supported on the [Apache Felix](http://felix.apache.org/site/index.html) OSGi Container and is included with the installation of IDM.

Output messages are displayed on the Felix console, which is part of the Apache release. The console is useful for displaying errors that might occur during the configuration and operation of IDM. Note that messages are also logged to the IDM server log file.

Finally, the container runs within a Java Virtual Machine (JVM), which can run on most Operating Systems that support Java.

Note that the platform must be a supported Operating System and already have Java installed for running a Java Virtual Machine (JVM).

---

#### Download, unzip, and start IDM (TEST 3)

![LucidChart](LucidChartsTest1.png)

Note:

Installing IDM simply involves downloading the software zip file and unzipping the file to an empty folder.

For example, you can download and unzip either the released version of IDM or the evaluation build of IDM depending on your needs. For example, whether you are an existing customer with an appropriate license and account, or an evaluator who is interested in evaluating one or more ForgeRock products.

The student workstation in the instructor assigned lab environment already has the IDM evaluation zip file downloaded. In the first lab, you unzip the downloaded file to the `/opt/projects/` folder and test the samples in this `openidm` instance. In subsequent labs, you install IDM into other folders to keep your work separated. For example, in the next lab you install IDM in the `/opt/projects/enterprise` folder. This allows you to reference earlier work you have done.

The contents of the zip file are extracted to the `openidm` directory. To start IDM, simply change to the `openidm` directory and run the startup script.

IDM runs within an [OSGi Container](http://www.osgi.org/Technology/WhyOSGi). The current release of IDM is supported on the [Apache Felix](http://felix.apache.org/site/index.html) OSGi Container and is included with the installation of IDM.

Output messages are displayed on the Felix console, which is part of the Apache release. The console is useful for displaying errors that might occur during the configuration and operation of IDM. Note that messages are also logged to the IDM server log file.

Finally, the container runs within a Java Virtual Machine (JVM), which can run on most Operating Systems that support Java.

Note that the platform must be a supported Operating System and already have Java installed for running a Java Virtual Machine (JVM).

---

#### Review the IDM root directory structure

Some JavaScript to: show:

```javascript
./openidm
audit/          <--audit logs
bin/
bundle/
cli.bat
cli.sh
conf/           <--JSON configuration files
connectors/
db/
felix-cache/
legal-notices/
lib/
logs/           <--server logs
samples/        <--sample configurations
script/         <--custom script files
```

Note:

Notes to go along with this.

---

#### Continued

This should be the next slide to display more:

```sh
security/
shutdown.sh
startup.bat
startup.sh      <--startup script from the command line
ui/
workflow/       <--custom workflows
```

Note:

The slide shows the folders below the `openidm` folder where IDM is installed. The subfolders highlighted represent the folders that administrators interact with on a regular basis.

---

#### View the Felix console at startup

Here is some shell output:

```sh
$ ./startup.sh
Executing ./startup.sh...
Using OPENIDM_HOME:   /opt/projects/openidm
Using PROJECT_HOME:   /opt/projects/openidm
Using OPENIDM_OPTS:   -Xmx1024m -Xms1024m
Using LOGGING_CONFIG: -Djava.util.logging.config.file=/opt/projects/openidm/conf/logging.properties
Using boot properties at /opt/projects/openidm/conf/boot/boot.properties
-> OpenIDM ready
OpenIDM version "5.0.0" (revision: e6689a6) jenkins-OpenIDM – Release (5.x)-14 release/5.0.0

->
```

Note:

The slide shows an example of starting IDM with one of the sample configurations:

The `-p` option specifies the *project* location that contains the necessary configuration and scripts to start and run IDM. This includes the `conf/` and `script/` folders. Other folders within the project location support the configuration files. For example, the `data/` folder contains the files necessary for the connector configuration files found in the `conf/` folder.

The output shows the project starting start messages. You should get an `-> OpenIDM ready` response if IDM boots correctly. The terminal window remains at the Felix console prompt `->` until the IDM instance is `shutdown` from the Felix console.

---

#### Log in to IDM

![](IDM400B_01_01_BasicProvisioning/image7.png)

Note:

After installation, the default user who is authorized to administer IDM is the `openidm-admin` user. The default password for the user is the same as the username.

---

#### IDM includes two user interfaces:

- Self-Service UI for regular users
- Admin UI for administrators

Note:

The slide illustrates that the login page for both the Self-Service UI and Admin UI look identical; however, the URL's to access the interfaces are different. The Self-Service UI is for all authorized users accessing IDM. The Admin UI is specific for IDM administrators.

The example shows the URL using the host and domain name of the server. You can also use `localhost` when working on the same server; however, it is best to try and use the proper name that you will be using when testing.

---

#### Log in to the Admin UI as the administrator user and view the Reconciliation Dashboard

![LucidChart](LucidChartsTest1.png)

Note:

The slide shows a screenshot of what the IDM administrator would see when they first log in to the Admin UI.

The `openidm-admin` user has administrative privileges which let the administrator manage IDM (via the DASHBOARDS and CONFIGURE menu items) and manage the IDM users (via the MANAGE menu item).

Note that there are initially no end users managed by IDM. The administrator can choose to add users manually through the Admin UI (by selecting MANAGE and then USER). However, users are normally populated into the IDM repository by synchronizing with external resources that contain identity data or by allowing users to register through the Self-Service UI.

The Reconciliation Dashboard is automatically displayed when you first log in to the Admin UI as the administrator. The Dashboard page contains four main sections that allow you to quickly manage and view the state of the IDM installation. The administrator can also manage the Dashboard via the DASHBOARDS menu option and the + Add Widgets button.

The first section contains a Quick Start set of options that allow the administrator to perform the most common options when configuring or administering IDM.

---

#### View the Last Reconciliation and System Health sections in the Admin UI Dashboard

![LucidChart](LucidChartsTest1.png)

Note:

If you scroll down from the Reconciliation Dashboard, the next two default sections of the Dashboard include information about the last reconciliation to occur, which in this case is empty because there are no connectors and mappings defined to synchronize identity data, and the system health.

Reconciliation is basically the *process* that initiates identity synchronization between an external resource and IDM. You first define a connector and how it communicates with an external resource and then define a synchronization mapping or rules between the connector and IDM to tell IDM how and when to synchronize data.

The System Health section contains a set of widgets to give you a visual indication of how much CPU and memory are used by IDM.

---

#### Performing Basic User Provisioning

The end of the lesson.
