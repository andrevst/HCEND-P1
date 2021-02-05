# Lesson 4 Glossary

**Actions** - A set of sequentially executed tasks in a blueprint. Basic actions included in every blueprint consist of: Create, Delete, Stop, Start, Restart.

**Application profiles** - Application profiles often include choices about where an application should run (AHV or AWS), but they can also be about sizing (small or large), or a combination of the two (small AHV or large AHV or small AWS).

**Dependency** - A logical sequential connection between services, orchestrating their basic actions together.

**Macro** - Either a Calm built-in or blueprint specified variable that can be used in tasks and variables.

**Multi-VM Blueprint** - Multi-VM blueprint is a framework that you can use to create an instance, provision, and launch applications requiring multiple VMs.

**Service** - An instance of a substrate, typically a virtual machine, existing machine, or a Kubernetes pod.

**Substrate** - An infrastructure provider instance, a blueprint can use all the providers enabled in a project.

**Task** - An individual stage of operational execution in an action.

**Variable** - A blueprint property: statically set by the developer role with a default value, used as a macro in task, and specified with a runtime property to delegate setting by an operator role during blueprint launch.
