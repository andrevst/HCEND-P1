# Creating and Publishing Multi-VM Blueprints

## Introduction

We can create two different types of blueprints based on the complexity of the application that you need to automate. This lesson focus on Multi-VM Blueprint. We will:

- Start the lesson by covering some of the common **blueprint lifecycle management** tasks such as cloning, downloading, and uploading a blueprint.*To create a blueprint, you must first create a project and configure the environment.*
- Bbriefly cover **Calm services and substrates** and walk you through the process of configuring a Nutanix environment as an example.
- Introduce various advanced **Calm constructs such as macros, variables, dependencies, library, etc.** These concepts will serve as a foundation to understand the process of creating a multi-VM Calm blueprint.
- Finally, cover each task involved in creating a **multi-VM Calm blueprint**.
- An overview of the **LAMP stack**, the foundation to build course's web application.
- **Load balancer and web servers** design with a scaling web service tier to address our “two tier” web application’s high availability and performance.

The goal is to perform blueprint lifecycle management tasks, configure a Nutanix environment and create a multi-VM Calm blueprint.

## Blueprint Lifecycle Management

### Clone a Blueprint

Cloning is an easy way to allow experiments and improvements to be conducted while keeps the original blueprint unaltered and ready to use by others. Also allows to publish and promote the changed blueprint for consumption and deletion when it is ready.

It is possible to clone an application blueprint from a pre-seeded application blueprint. Cloning a blueprint is a simple process.

1. From the Marketplace, click the application blueprint you want to clone.
2. Click Clone.
3. Enter a name and select the project that you want to assign it to.
4. Click Clone again.

### Download a Blueprint

Download a Blueprint to retain an offline copy and add it to a repo for later use. There is also the option to download the blueprint with the credentials and secrets used in the blueprint.

To download a blueprint:

1. Click the blueprint icon.
2. Click the blueprint that you want to download.
3. Click Download.
4. Select the check box, if you want to include credentials and secrets and type a password.
5. Click Continue.

*You can also perform the same operation by selecting the blueprint and clicking the Download option under the Actions dropdown.*

### Upload a Blueprint

Upload too restore a downloaded blueprint, transport between Calm instances or to use a public community available one. Much like the ability to download a blueprint, the ability to upload one is also one of the eleven blueprint management tasks that we briefly discussed in Lesson 3.

You can also upload configured blueprints to the Blueprints tab. To do that:

1. Click the Blueprint icon.
2. Click Upload Blueprint.
3. Browse and select the saved blueprint.
4. Give the blueprint a name and select the project you want to assign it to.
5. Click Upload.

*If you have previously downloaded the blueprint with credentials and secrets, then during upload you must provide the same password as previously specified.*

## Calm Services and Substrates

Services, substrates, application profile, and actions are primary elements required to create a multi-VM blueprint.

### Services

Services are logical entities exposed by an IP that span all application profiles and are managed by Calm. End users and services communicate with each other over a network using their exposed IPs and ports.

### Configuring Substrates

Substrates are combinations of the underlying cloud and the VM instance. When you select the desired cloud in the Calm UI, all the fields required to create a VM instance on that particular cloud are displayed. The combination of these fields is a substrate.

The substrate or VM details are configured in the environment.

The environment depends on the cloud provider selected during the project creation process. The environment consists of the cloud provider and the operating system details. For example, if you choose Nutanix as the provider. You have two options, Nutanix+Linux and Nutanix+Windows. Calm supports ***Nutanix AHV, Nutanix Xi Cloud, AWS, VMware, GCP, and Azure*** as providers for your project. 

#### Configure a Nutanix AHV environment

Environment allows the addition of multiple credentials and configuration VM details for the selected provider. It can be configured in project creation.

- Environment is mandatory to publish the applications into the marketplace.
  - If no VM configuration is defined while creating a blueprint, the configuration must be defined as part of the environment.
  - During the application blueprint launch from the marketplace, the values are picked from the environment.

*Before configuring a Nutanix environment, ensure there is a project with the desired provider.*

To configure the environment for Nutanix:

- In the Create Project page, Under the environments tab, select Nutanix.
- Under the Nutanix tab:
  - Enter credential details. Nutanix supports both password based and SSH private key based secret type.
  - Select the operating system type.
  - Provide VM configuration details such as name, image, firmware, vCPU, device bus, memory, etc.
  - Confirm whether or not to check log on status after the VM is created.
  - Specify the connection type and port.

## Macros

Before we talk about macros, it is important to understand variables.

The properties such as IP addresses, DNS names, and instance IDs that are associated with the services provisioned in blueprints are called variables. 

- They can be static, provided at run time, or generated during blueprint or action runs.
- Variables can have various data types (strings, integers, dates, or times) and various inputs (generated using single value, single input arrays, multiple input arrays, or an API call), and can be validated through regular expressions (regex).

### Macros Overview

Macros enable you to access the value of variables and properties set on entities, and help you make generic scripts and create reusable workflows.

For instance, a web server install script could use a macro to reference the IP address of a database. At deployment, the system replaces the macro with the actual IP address. Macros begin with ```@@{“ and end with “}@@```.

The syntax of a macro is "@@{variable_name}@@", where "variable_name" is the name of the variable.

The syntax to access the value of variables or properties of other entities or dependencies is ```@@{.<variable/attribute name>}@@```.

- ```entity name``` is the name of the other entity or dependency
- ```variable/attribute``` name is the name of the variable or attribute.
  - For example:
    - A blueprint contains a service by the name of app_container
    - To access the IP address of the app_container service in any other service use ```@@{app_container.address}@@``` syntax.

> Calm macros are part of a templating language for Calm scripts. These are evaluated by Calm's execution engine before the script is run.

#### Supported Entities

- Application
- Service
- Package
- Virtual machine

#### Types of Variables

> There are two types of variables

- User-defined
- Built-in.
  - [Nutanix variables](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v2_9_8:nuc-macros-ahv-c.html)
  - [AWS Variables](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v2_9_8:nuc-nucalm-aws-variables-c.html)
  - [GCP Variables](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v2_9_8:nuc-macros-gcp-variables-c.html)
  - [Azure Variables](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v2_9_8:nuc-macros-azure-variables-c.html)
  - [Kubernetes Variables](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v2_9_8:nuc-macros-kubernetes-c.html)
  - [VMware Variables](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v2_9_8:nuc-macros-vmware-variables-c.html)

#### Credentials as Macros

ou can also access credentials as macros. The format to access credential is:

```@@{cred_name.username}@@" and "@@{cred_name.secret}@@```

- ```cred_name```: Name of the credential with which the cred is created.

#### Access Macros of an Array Service

Nutanix Calm allows you to access macros of an array service using a special macro, which starts with "calm_array". You can configure a VM with replicas and access the common macros of all the replicas.

Let’s look at some of the examples:

- Use the following macro to retrieve the name of all the instances of VM separated by commas. ```@@{calm_array_name}@@```
- Use the following syntax to retrieve the IP address of all the instances of VM separated by commas. ```@@{calm_array_address}@@```
- Use the following syntax to retrieve the ID of all the instances of VM separated by commas. ```@@{calm_array_id}@@```
Supported Data Types
- Macro supports string and numbers data types. You can use them in the following format:

- String: ```@@{"some string"}@@``` or ```@@{'some string'}@@``` Newline or other such special characters are not supported. You can use \ to escape quotes.
- Numbers: Supports integer and float. For example, ```@@{ 10 + 20.63 }@@```"

#### Supported Operations

- Basic binary operations or numbers. For example, ```@@{(2 * calm_int(variable1) + 10 ) / 32 }@@```
- String concatenation. For example, ```@@{ foo + bar }@@```
- Slicing for strings. For example, ```@@{foo[3:6]}@@```

## Application Profiles

One of the tasks involved in creating a multi-VM blueprint is to add an application profile. You must select an application profile while you launch a blueprint. Application profiles often include choices about where an application should run (AHV or AWS), but they can also be about sizing (small or large), or a combination of the two (small AHV or large AHV or small AWS).

***An IT operator or developer should have a good understanding of the underlying differences between these choices, while abstracting that complexity from end users.***

Application Profile Tips and Best Practices
Using default profile

Every blueprint has a default profile - it can be thought of as a base layer of the blueprint.

The default profile is used in the single-VM blueprint, but it is invisible to the user.
If needed, the default profile can be renamed for a better description for operators.
Using additional profiles

Additional application profiles provide the operator role (or higher) deployment choices when using an application deployment.

This increases blueprint reuse (of actions and governance) by eliminating the need to make separate blueprints for each permutation of deployment.
Use application profiles to reduce the number of delegated run-time properties. Less choice reduces complexity and increases productivity.
Naming convention

When naming an application profile:

Make it as simple and user-friendly as possible.
Use nouns that reflect the audience use case or jargon.
Capitalized noun, ideally without spaces.
Make application profiles a set of mutually exclusive choices.
Avoid overly specific or jargon names when possible.
Some examples of right naming conventions are as follows:
Production, Staging, UserAcceptanceTesting, Test, QualityAssurance, Development, ContinousIntegration
Public, Private, Hybrid
AHV, AWS, Azure, GCP, ESX, K8s
DataCenter1, BranchOffice9, Colo3, DisasterRecoveryWest, DisasterRecoveryCentral
Small, Medium, Large, Jumbo
Titanium, Gold, Silver, Bronze
Sample Usage of Application Profiles

Application profiles express the intent of the blueprint developer to allow end-user choice between multiple deployment scenarios. Some typical choices include:

Capacity Size: small versus medium versus large resource consumption for different needs on different days. For example, developers may need to frequently create new, small deployments with less CPU, storage, and memory using the latest version of today’s application code, so they would choose a “small” application profile. At the beginning of each week, testers could run a new performance test on a large resource environment using double the CPU, the same amount of memory, and quadruple the storage, so they would choose a “large” application profile, which may also be used by operations for production deployments.
Infrastructure Provider: for a public, private, and/or hybrid deployment using different infrastructure providers. For example, developers may be restricted by governance to deploy daily workloads only on the private cloud, testers may deploy performance testing only on the public cloud when they need ephemeral resources, and staging and production workloads might always be deployed in a hybrid cloud fashion with a mix of multiple public and private clouds.
Location and Environmental Resources: different scenarios may need different resources, for example a database address with user and password credentials may vary by location or by environment.
For example, you may have developer teams based in Asia, Europe, and the Americas. An application profile named “Asia” might have a Database variable set for db.asia.example.com, while an application profile named “Europe” might have the same database variable set to db.europe.example.com. Another example could incorporate choice for using development versus a staging database, each with different username and password credentials.

A “development” application profile variables may be set for username = development and password = development_password, while an application profile named “staging” could set the same variables to different values for username = staging and password = staging_password. or Limited to full configuration delegation with run-time property overrides.

Any combination of the above: a hypothetical “Asia Development” application profile could incorporate all of these elements and more, such as: small capacity size, private cloud provider, located in Asia, and a development environment database variables, including allowing run-time override for any of the properties if desired.

## Actions

Application Profile Actions, or Profile Actions, in short, are a set of operations that you can run on your application. For example, when launching a blueprint, the ‘Create’ action is run. If your application is not needed for a period of time, you could then run the ‘Stop’ action to gracefully stop your application. When you’re ready to resume work, running the ‘Start’ action will bring the app back up.

The default application profile also contains several actions, which appear as gray ovals on a service. Actions consist of one or more tasks. Tasks are executed sequentially on each service. The types of tasks are ***Execute, Set Variable, HTTP, and Delay***.

> All services execute their actions and tasks in parallel unless a dependency is created to control orchestration, allowing operational control across the entire application.

### Let's review the simplest life cycle actions for IaaS

#### Create and Delete

Create: This action is invoked upon a blueprint launch and covers all services to allow orchestration. It provisions and configures the service in the provider.

Delete: This action deprovisions the services with the provider.

#### Start, Stop, and Restart

These actions are available to operate the entire deployment. But in the simplest blueprints, these typically remain empty until populated with tasks. In addition, when the create action is complete, it will immediately call the start action on each service.

Correspondingly, the delete action will call and complete the stop action before performing its tasks on each service.

## Library

Calm Library allows you to save user-defined tasks (scripts) and variables that can be used by other application blueprints. By using existing user-defined tasks and variables, you do not have to define the same tasks and variables again. You can share tasks and variables listed as part of the library across different projects. You can also customize an existing task or variable.

### Variable Types Overview

Users can create custom variable types for added flexibility and utility. Beyond just string and integer data types, you can create more data types such as Date/Time, list, and multi-line string. List values can be defined as a static list of values or attach a script (eScript or HTTP task) to retrieve the values dynamically at runtime.

While creating a custom variable type, you are required to select a project. However, you can share the variable type with multiple other projects using the "Share" option on the same page.

### Overview

You can create tasks while configuring a blueprint and publish these tasks to the library. Nutanix Calm allows you to import these published tasks while configuring other blueprints across multiple projects.

Tasks can be browsed and loaded from or saved to the library, to promote reuse across blueprints. Tasks from the library are copied into a blueprint. They do not update when the library changes, which keeps the blueprints safe.

## Creating a Multi-VM Calm Blueprint

Multi-VM blueprint is a framework used to create an instance, provision, and launch applications requiring multiple VMs. It is possible to define the underlying infrastructure of the VMs, application details, and actions that are carried out on a blueprint until the termination of the application.

There are multi-VM blueprints for the Nutanix, AWS, VMware, GCP, and Azure providers.

Once a provider is configured and a project is created, it is possible to create a multi-VM Calm blueprint. The process involves six tasks:

1. Adding a service.
2. Configuring VM, package, and service for your provider.
3. Setting up the service dependencies.
4. Adding and configuring an application profile.
5. Optionally, adding and configuring scale-out and scale in.
6. Creating an action

### Adding a Service
Services are the virtual machine instances, existing machines or bare-metal machines, that you can provision and configure using Nutanix Calm. You can either provision a single service instance or multiple services based on the topology of your application. A service exposes an IP address and ports on which the request is received.

To add a service, from the Blueprint page, click + Create Blueprint and select Multi VM/Pod Blueprint from the dropdown.

In the Blueprint Setup window:

Enter the name and description for the blueprint.
Select a project. The available cloud options depend on the selected project.
Click Proceed.
Click + next to the Services. The service inspector panel is displayed.
Configuring VM, Package, and Service for your Provider
This task involves defining the underlying infrastructure of the VM, application details, and actions that are carried out on a blueprint until the termination of the application on a Nutanix platform.

In the service inspector panel, enter a name for the service. The panel includes three tabs, VM, package and service.

Under the VM tab:

Enter a name for the VM.
Select either the existing machine or Nutanix for platform.
Select the OS type .
Specify the environment details or clone from the existing one.
Add credential details.
Specify the connection details such as port, type and protocol.
Under the Package tab:

Enter the package name.
Click Configure Install to install a package.
Configure tasks.
You can even add the default system actions, if needed.
Similarly, you can configure the tasks for uninstalling a package. When configuring the tasks, you have the choice to either reuse a system task or create one based on your requirement.

Under the Service tab:

Enter the service name
Enter the number of default, minimum and maximum service replicas that you want to create.
Add variables.
Once you provide all these details and click Save, the blueprint is saved and listed under the blueprints tab.

Setting Up Service Dependencies
Dependencies define the order in which the system runs the tasks; Calm visualizes this process in the design phase with arrows indicating the direction of Nutanix Calm execution. You can manually define dependencies—a web server depends on a database—or automatically define them with the Calm engine when macros are referenced.

To set up a service dependency:

Select the service.
Click the dependency icon.
Drag-and-drop to the service on which you need to create the dependency.
Adding and Configuring an Application Profile
You need to apply the application profile while launching a blueprint. To add and configure an application profile:

Click + icon in the Application Profiles.
Enter an application profile name.
Enter the name and value of the variable.
Check the Secret check-box to hide the variable value.
Adding and Configuring Scale Out and Scale In
Before creating an action, you can optionally add and configure the scale out and scale in task. The scale-in and scale-out functionality lets you decrease or increase the number of service replicas. When you configure the scaling task as part of a profile action, you can define the number of instances to add or remove for the service per scale action or choose to define the number at runtime.

To add a scale in and scale out tasks, under the Application Profile, click + against Actions.

Enter the name of the task.
Select Scaling from the Type drop-down menu.
Select either Scale In or Scale Out.
Enter the number in the Scaling Count field.
Click Save.
The scaling out and scaling in number should be less than the minimum and maximum number of replicas defined for the service.

Creating an Action
An action is a set of operations that you can run on your application that are created as a result of running a blueprint. To create an action:

Click the + icon in the Actions pane.
Select the service on which you want to create the task.
Enter the action details.
Enter the task details.
Once you have performed all these tasks, you can now see the blueprint in the Blueprints page.

## Conclusion

This concludes our lesson, Creating and Publishing a Multi-VM Calm Blueprint. Before we move to the next lesson, let's do a quick recap of what was covered.

Some of the common blueprint lifecycle management tasks are cloning, deleting, and uploading a blueprint. You can clone an application blueprint from a pre-seeded application blueprint. You can download the blueprint with the credentials and secrets. You can upload a configured blueprint to the blueprints tab.

The environment is mandatory to publish the application into the marketplace. You can define the environment either when creating a project or when creating a blueprint. If you have configured the environment when creating the project, you can clone that configuration for the VM while configuring a blueprint.

Macros enable you to access the value of variables and properties set on entities. Calm macros are part of a templating language for Calm scripts.

Application profiles often include choices about where an application should run (AHV or AWS), but they can also be about sizing (small or large), or a combination of the two (small AHV or large AHV or small AWS). Every blueprint has a default application profile, it can be thought of a base layer of the blueprint.

Profile Actions are a set of operations that you can run on your application. Actions consist of one or more tasks. Tasks are executed sequentially on each service.

Calm Library allows you to save user-defined tasks (scripts) and variables that can be used by other application blueprints.

Finally, a Multi-VM blueprint is a framework that you can use to create an instance, provision, and launch applications requiring multiple VMs. Creating a multi-VM blueprint involves adding a service, configuring the VM, package and service for the provider, setting up the service dependencies, adding and configuring an application profile, and creating an action.

This completes all the topics necessary to configure a blueprint. So far, we have covered all Calm constructs and walked you through the process to create a single and multi-VM Calm blueprint. In the next lesson, we will explore how Calm automates a 3 tier web application.

## Additional Optional Reading

[Macros Overview in the Nutanix documentation](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v3_0_0:nuc-components-macros-overview-c.html)
[Action Overview](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v3_0_0:nuc-nucalm-components-layer-action-c.html)
[Library Overview](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v3_0_0:nuc-task-library-overview-c.html)
[Library Usage](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v3_0_0:nuc-task-library-usage-c.html)
[Seeding Scripts to the Calm Task Library](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v3_0_0:nuc-seeding-scripts-to-the-calm-task-library-t.html)
