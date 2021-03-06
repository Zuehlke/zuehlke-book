---
authorName: Florian Besser
authorGithubUsername: florian-besser
issue: 76
title: Containerisation and why to use it
---
# {{page.title}}

You are a good software engineer, but releases are a time of pain. Your project matches one or more of the following:
* Deployments are done manually or via scripts that execute on some remote host.
* Scaling the app is hard as new machines or VMs take weeks to arrive and set up.
* Your application dependencies differ in version or feature set between environments.
* Developers have difficulty reproducing production issues on their local machines.
* Application dependencies are installed manually, developers spend days setting up their own machine.

This article will explain how containerisation can help solve each of these issues.

## Cumbersome deployments and dirty hosts

Running scripts on a host seems OK in the beginning, but over time your host will become "dirty". Some versions of your script were only run in QA, and production has leftover artefacts from previous deployments. These leftovers confuse maintainers and reduce confidence in your team's ability to deploy software to production systems.

With containerisation, you immediately get rid of leftovers your software may cause. By **throwing away the old container** after deploying a new version, you have already cleaned up after your application.

Sometimes you want to persist data across container crashes or restarts. In these cases you can use volumes to store the data and not pollute your host.
Volumes allow you to use containerisation with databases quite nicely: The data is safe from crashes but up- and downgrading a DB is just a question of stopping the old container and booting a new one.

## More hardware and the procurement process

There is a conflict between needing more resources and a company's willingness to spend money. Usually, there is a long-winded procurement process for ordering hardware and getting it installed in a rack. At the current time, when hardware is cheap and humans are expensive, paying some PaaS a few hundred bucks a month is cheaper in most cases.

Any established platform will offer you support for Docker, and if you have containerised your application you'll be able to solve the scaling problem a lot more easily. Usually, this involves uploading the image to the platform, creating a service and then defining scaling rules for said service. While the platform will not run on your local machine, this does not prevent you from testing locally, as you can install Docker and are guaranteed the same runtime as on the platform.

Protip: Most platforms offer some functionality for doing rolling updates, you can deregister your containers from the load-balancer before killing them, thus achieving deployments that are invisible to the customer. Now every day is a production deployment day!

## Different environments, different dependencies and the highway to hell

Differences between environments should be as small as possible, but they tend to increase over time. Someone upgrades only the QA database for testing; someone applies emergency security fixes only to production because other environments are not critical. You find an increasing number of builds pass QA but fail in staging or production!

Containerisation can remove this problem as **whatever is containerised is versioned** and there are **orchestration tools** that ensure all dependencies are the correct version. Orchestration means that the app and its dependencies are deployed together as one coherent bundle. This way you can remove uncertainties for everything you can containerise. 

Note that many things a novice deems uncontainerisable are actually containerisable: From any database, including Oracle, to messaging systems to webservers, you'll find premade images or you can roll your own. Anything that runs on an unmodified Linux kernel can be containerised!

## Production bugs and developer madness

We task developers with reproducing production bugs locally and fixing them. This approach can suffer greatly if the developer has to emulate production instead of running an exact copy. In the worst case, this leads to unreproducible bugs and some "blind" bug-fixing in the hopes that changing some related thing will solve the issue.

Luckily, with containerisation, we are able to run an exact copy of the version that is in production. If bugs are not reproducible, they depend on application dependencies (see above) or on the production configuration (but that should be easy to debug!).

## Setting up ::1 and the onset of despair

As people join there'll be a recurring pain setting up new machines. Done manually it has two downsides: Any newcomer must experience the _setup_ of all the dependencies used and they end up with different versions than their teammates. This introduces "works on my machine" between developers, costs a lot of time and demotivates newcomers.

But if you already containerise your dependencies and have an orchestration solution, then you run the orchestration locally. You'll get the correct dependencies, the correct version, and the correct configuration.

Note that some people argue that setting up everything yourself is a good exercise but I think the dangers outweigh the knowledge gain as the knowledge can more easily be gained by giving newcomers an overview and some first tasks with purpose.

*By {{page.authorName}}*
