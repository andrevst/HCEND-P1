Glossary
Application Profile Actions - Application Profile Actions (or Profile Actions for short) are a set of operations that you can run on your application. For example, when launching a blueprint, the ‘Create’ action is run.

Application Profiles - Application Profiles expose simple choices to your end users. These choices are often about where an application should run (AHV or AWS), but they can also be used for ‘t-shirt’ sizing (small or large), or a combination of the two (small AHV or large AHV or small AWS).

Blueprint - Blueprints are essentially recipes for applications. These recipes encompass application architecture and Infrastructure choices, provisioning and deployment steps, application binaries, command steps, monitoring endpoints, remediation steps, licensing and monetization, and policies. Every time a Blueprint is executed it results in an application.

CI/CD - CI/CD stands for Continuous Integration/Continuous Delivery or Continuous Integration/Continuous Deployment. It is a term that is commonly used when discussing modern software development and is the solution to the problems that integrating new code can cause for development and operations teams. CI/CD solves those problems by requiring ongoing automation and continuous monitoring through the lifecycle of an application, from integration and testing to delivery and deployment.

Custom Profile Actions Custom Profile Actions are created by the blueprint developer, and should be added whenever you need to expose a set of operations to the end user.

Custom Service Actions - Custom Service Actions are created by the blueprint developer, and should be added for any repeatable operations within the blueprint, like a function definition in a programming language.

Marketplace Manager - The Marketplace Manager is a section of the Calm UI that allows you to manage both custom blueprints and marketplace application blueprints. From the Marketplace Manager, you can publish, unpublish, and delete blueprints.

Multi-VM Blueprints A multi-VM blueprint is a framework that you can use to create an instance, provision, and launch applications that require multiple VMs.

Package Install/Uninstall - Package Install/Uninstall are operations which are run during the ‘Create’ or ‘Delete’ profile actions. In other words, they are operations that run when a user first launches a blueprint, or finally deletes the entire application.

Profile Actions - See: Application Profile Actions, above.

Service Actions - Service Actions are a set of operations to be run on an individual service. Services span application profiles, so their actions (and the operations underlying those actions) do as well.

Services - Services are logical entities exposed by an IP, which span all application profiles, and are managed by Calm.

Single VM Blueprint - A single VM blueprint is a framework that you can use to create an instance, provision, and launch applications requiring only a single VM.

Substrates - Substrates are a combination of the underlying cloud and the virtual machine instance. When the desired cloud is selected in the Calm UI, all of the fields required for creating a virtual machine instance on that particular cloud are displayed — the combination of all these fields make up a substrate.

System Defined Profile Actions - System Defined Profile Actions are automatically created by Calm in every blueprint and underlying application.

System Defined Service Actions - System Defined Service Actions are automatically created by Calm in every blueprint and underlying application. While these actions cannot be individually invoked, they are called when the corresponding profile action is run.

VM Pre-create/Post-delete - VM Pre-create/Post-delete are operations which are run before the substrate is created, or after it is deleted.