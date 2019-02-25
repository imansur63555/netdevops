<p align="center"> 
<img src="imgs/0frontpage.jpg">
</p>

<!-- vscode-markdown-toc -->
* [Network Programmability](#NetworkProgrammability)
	* [The challenge of Dynamic applications vs Static network](#ThechallengeofDynamicapplicationsvsStaticnetwork)
	* [What is Programmability](#WhatisProgrammability)
	* [Why Coding](#WhyCoding)
	* [What has changed?](#Whathaschanged)
	* [Summary](#Summary)
* [NetDevOps](#NetDevOps)
	* [The challenge of network configuration today](#Thechallengeofnetworkconfigurationtoday)
	* [Network configuration as code](#Networkconfigurationascode)
	* [Components](#Components)
	* [Benefits](#Benefits)
* [Demo - Network Configuration with NetDevOps](#Demo-NetworkConfigurationwithNetDevOps)
	* [Book a sandbox](#Bookasandbox)
	* [GitLab setup](#GitLabsetup)
	* [CICD setup](#CICDsetup)
	* [VIRL verifications](#VIRLverifications)
	* [Local environment setup (optional)](#Localenvironmentsetupoptional)
	* [Running the demo](#Runningthedemo)
	* [Summary](#Summary-1)

<!-- vscode-markdown-toc-config
	numbering=false
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

---

## <a name='NetworkProgrammability'></a>Network Programmability

Do you often ask yourself why we keep configuring our network devices in the same way we have been doing it for the last 30 years? Isn't it strange that we still have to log into each individual box and use command-line instructions to perform any changes? Do you wonder if there might be a more optimal way of configuring your infrastructure, apart from CLI? Does this way of working make you feel like any _simple_ change in your network is _complex_ to implement?

__You are not alone.__

<p align="center"> 
<img src="https://media.giphy.com/media/paN2mV7vuCXx6/giphy.gif">
</p>

There are definitely alternative and innovative ways of programming your network infrastructure. Yes, when you configure your network devices to adopt a certain behaviour, or implement a new available feature, you are _programming_ them. So one of the first things we should be looking for is more optimal ways of programming our infrastructure.

### <a name='ThechallengeofDynamicapplicationsvsStaticnetwork'></a>The challenge of Dynamic applications vs Static network

Furthermore, as the network _exists_ to provide connectivity for __applications__, we should take a look at how these are evolving. Agile microservices-based cloud-native development, DevOps automation with CICD pipelines, and automated unit testing, enable really __dynamic__ application development for quick time-to-market requirements. Let's not forget that software is one of the most important assets to differentiate modern enterprises from their competition. Being able to quickly implement new features, deploy new locations, or fix issues, is absolutely __key__ to their success.

<p align="center"> 
<img src="imgs/1staticnw.jpg">
</p>

For the last years, servers have been virtualized with Virtual Machines that can be automatically deployed in minutes. These days the trend is going to container-based microservices, that are deployed _insanely fast_. These are short-lived entities that may be deployed dynamically across hybrid cloud environments, interacting among them to provide the desired service with virtually _unlimited_ scalability, and adapting to any possible issues in the underlying infrastructure via declarative statements.

In comparison, network infrastructure is much more _static_. In order to accommodate requirements from application developers it needs to be faster, more flexible and cost-optimized. Today network configuration is often a completely manual process that makes any desired change across the network complex and slow. The more elements these changes include (eg. firewalls, load-balancers...) the more difficult it gets to make them quick, reliable and adaptable. This situation often leads to bare minimum configurations in the network, that allows for a faster deployment (eg. no security ACLs, no QoS config, or trunking every VLAN in an interface) but usually leading to much bigger concerns.

<p align="center"> 
<img src="imgs/2skeleton.jpg">
</p>

Infrastructure is full of products designed to be used by... _humans_. It may not always seem that way, but human operators are the target users for CLI and web interfaces. This means that when you need to get something done via these interfaces, you (or some other human) has to do the work.

You won't have to think back too far to remember the last time you needed to complete some bulk-task on a computer. The task probably involved a lot of clicking, typing, copying-and-pasting, or other mind-numbing repetitions. These human interfaces (and the paradigm of having humans do the work) are to blame for the bulk-work that we sometimes have to do to complete a task.

Our brain has a great capacity, but clearly human input/output interfaces with a computer (typing and reading) are not very _fast_. Our thoughts neck down to this tiny straw, which output-wise is like poking things with your meat sticks, or using words (speaking or tapping things with fingers). For example, machine typing usually happens at a 20th of the speed you are thinking. And I am talking ten-finger typing, let's not even go into two-thumb typing... 
So while Elon Musk finishes his [BMI](https://waitbutwhy.com/2017/04/neuralink.html) (Brain Machine Interface), aka Wizard Hat, we will have to explore alternative options that optimize how we configure our networks.

<p align="center"> 
<img src="https://28oa9i1t08037ue3m1l0i861-wpengine.netdna-ssl.com/wp-content/uploads/2018/04/Communication-Speed-GRAPH-1.png">
</p>

### <a name='WhatisProgrammability'></a>What is Programmability

Computers are _great_ at bulk-work, but if you want your computer to talk to your infrastructure and do something, you will need a machine-to-machine interface or __API__ (Application Programming Interface): an interface designed for software pieces to interact with each other.

_Network Programmability_ uses a set of software tools to deploy, manage and troubleshoot network devices and controllers _via APIs_, gathering data and driving configurations to enhance and secure application delivery. This software can on-box or off-box, and work on-demand or event-driven.

We can ask an API to:
* Take some action
* Provide us with some piece of information
* Store some piece of information

We use these machine-to-machine APIs to make _simple_ requests to our infrastructure, which in aggregate, enable us to complete _powerful_ tasks.

For example, you might use APIs to make simple requests like...
* Get the status for interface X
* Get the last-change time for interface X
* Shutdown interface X
* Set the description of interface X to "Interface disabled per Policy"

... and that way complete a powerful task like: _"Disable all ports that have been inactive for 30 days."_

Sure, you could do this manually, but wouldn't it be better to codify the process (write it once) and then let your computer run this task whenever you need it done? 

If you need information from your infrastructure, ask for it. Using a machine-to-machine API means your request will complete, your data retrieved, or you will receive notification to the contrary. All done in a way that enables you to automate the interaction. APIs make it easy to send requests to your infrastructure, but what makes it easy to codify the processes?

### <a name='WhyCoding'></a>Why Coding

_Coding_ is the process of writing down instructions, in a language a computer can understand, to complete a specific task.

<p align="center"> 
<img src="https://media.giphy.com/media/OVtqvymKkkcTu/giphy.gif">
</p>

Let's consider a simple codified process that we are asking a computer to follow:
* For each switch in my network...
    * For each interface in the switch...
        * If the interface is down, and hasn't changed states in more than thirty days, then:
            * Shutdown the interface
            * Update the interface description to mention why it's been shut down

```python
for switch in my_network:
    for interface in switch:
        if interface.is_down() and interface.last_change() > thirty_days:
            interface.shutdown()
            interface.set_description("Interface disabled per Policy")
```

This is essentially the process that you, as a human, would go through to complete the same task. By taking the time to codify it (write it down in a machine interpretable language), you can now ask the computer to do the task whenever you need it done. You, the human, are providing the intelligence (what needs to be done and how it should be done), while letting the computer do the boring and repetitious work (which is what it does best). Win-Win.

<p align="center"> 
<img src="imgs/6not.jpg">
</p>

While the code sample above is a snippet of a larger script, and is calling other functions (like `interface.last_change()` and `interface.shutdown()`), implementing the utility functions is straightforward and the code shown is actual valid Python code that would complete the task. The core logic is that simple.

### <a name='Whathaschanged'></a>What has changed?

APIs and programming languages aren't new, so, why the recent hype?

Well... they have _matured!_

#### <a name='ModernProgrammingLanguagesTools'></a>Modern Programming Languages & Tools

Modern programming languages like JavaScript, Python, Go, Swift, and others are less cumbersome and more flexible than their predecessors. It used to be that you had to write 10,000 lines of C++ code to do anything useful, but with these modern languages (and packages and libraries available from their developer communities) you can do powerful things in less than 300 lines of code. Which is probably shorter, or on par with, most Cisco IOS configurations that you have worked with.

These languages, when combined with other modern developer tools (eg. Git repositories, Package management systems, Virtual environments, Integrated Development Environments) equip you with powerful development tools that enable you to automate your tasks and processes and begin creating your own set of powerful tools and workflows.

While these tools are great, and are now bringing rich value to the systems engineering discipline, we are also benefiting from another maturing area of the software development industry.

#### <a name='OnlineCommunities'></a>Online Communities

In the past, when you set out to create some script or program, you often had to start _from scratch_, working with low-level standard libraries included with your programming language and toolset of choice. This created a high barrier to entry (and massive global repetition) as software developers had to write the same _heavy lifting_ modules to get common tasks done. Take for example making a HTTPS web request, where they had to write code to:

* Open a TCP connection on port 443
* Handle TLS negotiation and exchange certificates
* Validate the certificates
* Manage the TCP connection (and any connection pooling)
* Format HTTP requests
* Interpret HTTP responses

That is a lot of work when all the developer wanted to do was to get or send some data to / from some remote server. This is the reason why engineers left this work to software developers.

Now, thanks to the Open Source community, social code-sharing and collaboration sites like GitHub, and public package repositories, the developer communities around these new modern programming languages are building and sharing Open Source software libraries that help to encourage reuse and reduce duplicate work. Leveraging these community-created libraries can save you tremendous amounts of time and effort, and they enable you to focus your time and effort on what you want your code to do: your codified process.

<p align="center"> 
<img src="https://media.giphy.com/media/5IqxJsqlCtkqc/giphy.gif">
</p>

You can make a HTTPS request without much personal investment, because of the work done by these online communities.

```shell
$ pip install requests
Collecting requests
  Using cached
<-- output omitted for brevity -->
$ python
>>> import requests
>>> requests.get("https://api.github.com")
<Response [200]>

```

What you are seeing here:
* We installed a community library from a public package repository ( `pip install requests` )
* We entered a Python interactive shell ( `python` )
* We imported the library into our Python code ( `import requests` )
* We made an HTTPS request to https://api.github.com and it was successful ( `<Response [200]>` )

Starting with installing the `requests` package on our machine, in four typed lines in a terminal we were able to download and install the package and use it to make an HTTPS request (without having to think about the steps involved with making the HTTPS request).

Now that languages and tools have evolved to be useful for infrastructure engineers, APIs have become easier to work with.

#### <a name='APIMaturity'></a>API Maturity

Gone are the days where it took an expert programmer to work with a product's API. Previous API standards like SOAP proved themselves to be not so _simple_, and easier to use API models like RESTful APIs have taken their place.

Now, thanks to RESTful APIs and standardized data formats like JSON, you can make requests of your infrastructure with the same ease these modern programming languages provide.

### <a name='Summary'></a>Summary

APIs and programming languages have evolved and matured to the point of being useful and applicable to the domains of infrastructure engineers.

The _net-effect_ being that you can get powerful things done with relatively small amounts of code. And by so doing, you can automate the repetitious and/or labor intensive parts of your job freeing you up to focus your time and effort on tasks deserving of your intellect.

Network programmability provides consistent and dynamic infrastructure configuration by automating deployments and simplifying network management, bringing the following main benefits:
* Automation
    * Time and cost optimization
    * Reduce errors
* Integration
* Innovation

## <a name='NetDevOps'></a>NetDevOps

DevOps principles are not exclusive to software development, and some of them can definitely be applied to infrastructure configuration. NetDevOps brings the culture, technical methods, strategies and best practices of DevOps to network management.

Sometimes it is referred to by different names, like _DevNetOps_, _NetOps_, or _SuperNetOps_. But in general it is related to the more generic Network Reliability Engineer (also coming from the DevOps counterpart [Site Reliability Engineering](https://en.wikipedia.org/wiki/Site_Reliability_Engineering)).

### <a name='Thechallengeofnetworkconfigurationtoday'></a>The challenge of network configuration today

Networks exist to provide connectivity for end-systems and applications, so obviously they have a critical role in any type of service. _Everything_ needs connectivity, so the network is certainly a fundamental asset in any modern enterprise these days. Its functionality has become so critical that most business nowadays would not be able to survive without connectivity.

However there is a very common _perception_ that the network is actually _fragile_.

<p align="center"> 
<img src="imgs/3yoda.jpg">
</p>

Key network engineers that have been working long enough on a certain network become _gurus_. They are the ones that know the _why_ and _how_ of multiple specific configurations: why _that_ had to be done last year on those core routers, how many neighbors should be seen by a certain edge router, or what that propagated BGP community means. Every box has a _unique configuration_ to accommodate whatever was required at a specific point in time: troubleshooting or debugging a certain issue, that small fix in the routing protocol weight to determine the right interface to use, or those interfaces that are down and nobody knows if they should actually be up or not. Sequential and manual provisioning leads into a situation where each network device becomes a _snowflake_, due to how its configuration has changed organically according to whatever was required along since it was installed.

Without these key engineers there is a _fear_ that network changes will go _wrong_. So operations teams tend to minimize the number and frequency of changes in their networks. Nobody wants to affect that _precious_ business traffic and be pointed at by the CTO as the person responsible for that big failure. So changes rarely happen. And when they happen they are _BIG_, because there is a backlog of things to do. The bigger the change, the more possibilities that something will fail. Besides this, teams are not well practiced because changes do not happen often. Fixing an issue while operating a network _live_, or performing a rollback quickly, requires practice. So now any problem that happens during the maintenance window will lead to the perception that the network configuration change was a _failure_.

Furthermore, applying network-wide policies becomes a task proportionally tedious to how big the network is. For example, consider a possible Infosec recommendation to change SNMP strings every 3 months. Doing it manually in a big network might require a number of engineers performing those changes simultaneously across the network, maybe during a maintenance window by night to make sure systems can be synchronized next morning. This manual process involves quite some manual interaction, which is definitely prone to errors.

<p align="center"> 
<img src="https://media.giphy.com/media/10PcMWwtZSYk2k/giphy.gif">
</p>

This type of considerations is very similar to the ones they had in classic software development. With their monolith architectures and bi-annual software updates, they suffered from similar challenges. And then they started doing things different, with things like Agile, DevOps, CICD pipelines and automated unit testing.

Applying this same type of principles to network configuration is what we called NetDevOps, and it will provide similar benefits to the ones software developers obtained while implementing this practices in their own environment. But it will require big cultural changes, like:
* _Embracing failure_ and learning from it for the future
* Understand that _change is good_
* _Collaborate actively_ between network developers and operations teams
* _Empower teams_ to take ownership and responsibility
* Provide _feedback systems_ that are actually useful to iterate and improve processes
* End-to-end _automation_ for the whole lifecycle of changes

<p align="center"> 
<img src="imgs/4culture.png">
</p>

What if network engineers started working with network configurations the same way software developers work with their code?

What if we could create automated pipelines for those network configurations, that worked like CICD does for software development?

What if the network could be continuously monitored for health and improvement?

Now __that__ would be a game changer. Not only in the way we manage our networks, but also in how we scale up, how we automate repetitive tasks, how different teams collaborate, and how we improve the reliability of our networks.

Let's explore it.

### <a name='Networkconfigurationascode'></a>Network configuration as code

With the advent of Cloud computing we have now the capabilities to provision and manage _ephemeral_ data centre resources (compute and connectivity) via machine-readable definition files. These files can be treated as common code, utilizing the same version control systems and best practices we use for software development, with goals like providing automation, improving efficiency and reducing errors. This is called _Infrastructure as Code_, or IaC.

We could follow the same approach with network device configurations, and this is what we call _Network as Code_. It is based on the idea of storing all network configurations in a [Version Control System](https://en.wikipedia.org/wiki/Version_control) (VCS) that manages and tracks changes in the network. This system storing all configurations for the whole network would be considered the [Single Source of Truth](https://en.wikipedia.org/wiki/Single_source_of_truth) for all-things network configuration.

In this new mode of operation, network configuration changes are proposed in code _branches_, like software code developers do. These branches are _safe_ places where network developers will be able to work _safely_ on their proposed configurations, without affecting the _master_ branch, where master configurations reside. Once these configurations are ready, developers will request their branch to be _merged_ with the master configurations, and will go through an approval process to verify there are no issues when incorporating these changes.

Continuing with the emulation of DevOps automation capabilities, this will lead into using CICD (Continuous Integration and Delivery) Build Servers to automatically deploy and test the proposed configurations in testing, staging and production environments. Configurations that sucessfully pass the complete tests set, will be deployed into the production environment. In case of failure during that final deployment, the system itself will automatically rollback the proposed changes, leaving the production network in the previous state just before the change.

<p align="center"> 
<img src="imgs/5cicd.png">
</p>

And considering that modern network devices support modern interfaces and APIs, let's leverage those to deploy our configurations across the network in an optimal way, instead of using the classic, slow and error-prone command-line interface.

### <a name='Components'></a>Components

Apart from a VCS we will need some additional elements to support the desired functionality:
* [Cisco Network Services Orchestrator](https://developer.cisco.com/site/nso/): formerly Tail-f, it provides end-to-end automation to design and deliver services much faster
* [pyATS](https://developer.cisco.com/pyats/): automation tool to perform stateful validation of network devices operational status with reusable test cases
* [VIRL](http://virl.cisco.com/): network modelling and simulation environment
* [Ansible](https://www.ansible.com/): simple automation

### <a name='Benefits'></a>Benefits

NetDevOps will deliver consistent version-controlled infrastructure configurations, deployed with parallel and automated provisioning.

Our wishlist for the desired system will provide the following benefits _across the whole network_:

* Track the status of network configurations at any point in time
* Track who proposed and approved each specific configuration change
* Provide visibility on what are the differences of configurations at any point in time vs a previous situation
* Enable rollback to any previous moment
* Provide syntax-checking capabilities for network changes in your own local workstation
* Automate the deployment of any proposed change across different environments (eg. testing, staging, production)
* Model simulated virtual environments to test proposed changes before going to production
* Define and run the required tests set and passing criteria, both in testing and production, before accepting a change as successful
* Automatically rollback any proposed configuration that does not pass the tests set

## <a name='Demo-NetworkConfigurationwithNetDevOps'></a>Demo - Network Configuration with NetDevOps

What better way of understanding the real benefits of NetDevOps than building your own setup and seeing how it works? The goal will be to create a complete environment that demonstrates all features in the previous wishlist.

### <a name='Bookasandbox'></a>Book a sandbox

The first thing you will need is a [sandbox](https://developer.cisco.com/site/sandbox/): an environment where you have all the required platforms and elements that you will need for your demo. In our case we need a _big_ server to run VIRL simulations for all network devices we will discuss later, and another server to run our VCS, NSO netsim, etc.

You may find the required sandbox for our demo using [this link](https://devnetsandbox.cisco.com/RM/Diagram/Index/6b023525-4e7f-4755-81ae-05ac500d464a?diagramType=Topology), and book it for up to one week exclusively for you.

<p align="center"> 
<img src="imgs/7reserve.png">
</p>

_Note: when doing the reservation please choose 'None' for simulation, as we will be launching the required topologies as part of the setup process._

Spinning up the whole system will take roughly 15 mins, so please look at this strangely satisfying pendulum while we get everything ready for you.

<p align="center"> 
<img src="imgs/8pendulum.gif">
</p>

Once the setup is ready you will receive an email with all required information to VPN into your sandbox. If you do not have a VPN client you may download AnyConnect [here](https://developer.cisco.com/site/sandbox/anyconnect/). Connect to your VPN and you are now ready!

### <a name='GitLabsetup'></a>GitLab setup

Open a terminal window (ie. [putty](https://www.putty.org/) on Windows or `terminal` on OSX) and `ssh` to your _devbox_ with the following credentials: `developer`/`C1sco12345`

```
$ ssh developer@10.10.20.20
```

Once in, clone the repository that includes all required files to build the setup into your _devbox_.

```
[developer@devbox prod]$git clone --recurse-submodules https://github.com/DevNetSandbox/sbx_multi_ios.git
```

With that, your sandbox _devbox_ includes now all required info to start building the environment.

```
[developer@devbox prod]$cd sbx_multi_ios/gitlab
[developer@devbox prod]$./setup.sh
```

`setup.sh` will start and configure your Version Control Server, a GitLab instance inside a Docker container running in your _devbox_. 

The process will take like 5 minutes, so check this out in the meanwhile.

<p align="center"> 
<img src="imgs/9pendulum2.gif">
</p>

Once your terminal shows the process is finished, you may check with `docker ps` that your GitLab containers are running, and how they offering their service in port 80.

```
[developer@devbox ~]$docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS                PORTS                                                                                       NAMES
5cd18a397811        gitlab/gitlab-ce       "/assets/wrapper"        2 days ago          Up 2 days (healthy)   0.0.0.0:80->80/tcp, 0.0.0.0:4567->4567/tcp, 0.0.0.0:32769->22/tcp, 0.0.0.0:32768->443/tcp   gitlab_gitlab_1
182c5937b931        gitlab/gitlab-runner   "/usr/bin/dumb-init …"   2 days ago          Up 2 days
```

Please point your browser to [http://10.10.20.20](http://10.10.20.20/), the IP address of your _devbox_ (default port 80), and check that you can access the HTTP interface for your new GitLab service.

### <a name='CICDsetup'></a>CICD setup

Now that GitLab is ready, go back to your terminal and let's run the script to setup the complete CICD environment.

```
[developer@devbox prod]$cd ../cicd-3tier
[developer@devbox prod]$./setup.sh
```

In this case `setup.sh` will perform the following actions:

1. Launch the required VIRL simulations for two different environments: test and production
2. Start NSO
3. Import test and production network configurations from VIRL to NSO
4. Synchronize devices configuration from NSO into VIRL simulations
5. Create a new repo in GitLab and initialize it locally in your _devbox_
6. Create locally in _devbox_ the prod and test git branches and push them to GitLab
7. List the status of VIRL nodes in _production_ and _test_

This complete process will take like 10 minutes, so time for your fix.

<p align="center"> 
<img src="imgs/10pendulum3.gif">
</p>

__Congrats, everything is installed and ready!__

### <a name='VIRLverifications'></a>VIRL verifications

Now you have two complete simulated environments running in your VIRL server: one for testing, and one replicating what would be a production physical network. Real world scenarios might be diverse: some customers may have a physical network in production, but only a simulated one for testing. Others might also have a real network for testing. Maybe even an additional one for staging before going to production. No matter how, the same principles apply to what we will be demonstrating. In our case the sandbox includes a couple of virtual environments, like the one depicted below, and implemented with VIRL for convenience.

<p align="center"> 
<img src="imgs/11topology.png">
</p>

As you can see each environment includes a standard 3-tier architecture, with 2x IOS-XE routers in the Core, 2x NX-OS switches in Distribution, and another 2x NX-OS switches in the Access layer.

You may find VIRL definitions for these two environments at the following locations in your _devbox_:
* `/home/developer/sbx_multi_ios/cicd-3tier/virl/test/topology.virl`
* `/home/developer/sbx_multi_ios/cicd-3tier/virl/prod/topology.virl`

Please make sure all your simulated routers are readily available (_REACHABLE_ status) in both prod and test. If they are not, your demonstration will fail in different stages.

```
[developer@devbox test]$pwd
/home/developer/sbx_multi_ios/cicd-3tier/virl/test
[developer@devbox test]$virl nodes
Here is a list of all the running nodes
╒══════════════╤═════════════╤═════════╤═════════════╤════════════╤══════════════════════╤════════════════════╕
│ Node         │ Type        │ State   │ Reachable   │ Protocol   │ Management Address   │ External Address   │
╞══════════════╪═════════════╪═════════╪═════════════╪════════════╪══════════════════════╪════════════════════╡
│ test-dist1   │ NX-OSv 9000 │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.213        │ N/A                │
├──────────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ test-access1 │ NX-OSv 9000 │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.215        │ N/A                │
├──────────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ test-dist2   │ NX-OSv 9000 │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.214        │ N/A                │
├──────────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ test-core2   │ CSR1000v    │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.212        │ N/A                │
├──────────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ test-core1   │ CSR1000v    │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.211        │ N/A                │
╘══════════════╧═════════════╧═════════╧═════════════╧════════════╧══════════════════════╧════════════════════╛
[developer@devbox test]$cd ../prod
[developer@devbox prod]$virl nodes
Here is a list of all the running nodes
╒═════════╤═════════════╤═════════╤═════════════╤════════════╤══════════════════════╤════════════════════╕
│ Node    │ Type        │ State   │ Reachable   │ Protocol   │ Management Address   │ External Address   │
╞═════════╪═════════════╪═════════╪═════════════╪════════════╪══════════════════════╪════════════════════╡
│ core2   │ CSR1000v    │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.222        │ N/A                │
├─────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ core1   │ CSR1000v    │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.221        │ N/A                │
├─────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ access1 │ NX-OSv 9000 │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.225        │ N/A                │
├─────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ dist2   │ NX-OSv 9000 │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.224        │ N/A                │
├─────────┼─────────────┼─────────┼─────────────┼────────────┼──────────────────────┼────────────────────┤
│ dist1   │ NX-OSv 9000 │ ACTIVE  │ REACHABLE   │ telnet     │ 172.16.30.223        │ N/A                │
╘═════════╧═════════════╧═════════╧═════════════╧════════════╧══════════════════════╧════════════════════╛
```

*If* any of the nodes stay in _UNREACHABLE_ status please try the following:

1. Go into the environment directory (prod or test) and restart the node.

    ```
    [developer@devbox prod]$cd virl/test
    [developer@devbox prod]$virl stop test-dist2
    [developer@devbox prod]$virl start test-dist2
    ```

2. Connect into that specific node (with `virl ssh` or `virl console`) and reboot it (password is `cisco`).

    ```
    [developer@devbox prod]$virl ssh core1
    Attemping ssh connectionto core1 at 172.16.30.221
    Warning: Permanently added '172.16.30.221' (RSA) to the list of known hosts.
    cisco@172.16.30.221's password:


    core1#reload
    ```

3. If it still refuses to cooperate, stop the whole environment...

    ```
    [developer@devbox cicd-3tier]$pwd
    /home/developer/sbx_multi_ios/cicd-3tier
    [developer@devbox cicd-3tier]$./cleanup.sh
    ```

    ... and then restart it.

    ```
    [developer@devbox cicd-3tier]$pwd
    /home/developer/sbx_multi_ios/cicd-3tier
    [developer@devbox cicd-3tier]$./setup.sh
    ```
Now that both of your VIRL environments are ready, let's setup your local environment.

### <a name='Localenvironmentsetupoptional'></a>Local environment setup (optional)

To experience and demonstrate the full NetDevOps configuration pipeline, you may want to setup a local development environment where you can test proposed configuration changes before committing and pushing them to GitLab for the full test builds to occur. This is a completely optional step you might want to skip if you are not interested in testing locally.

To complete this step you will need to have a few local pre-requisites setup on your local workstation.

**1. Common software**: install Java JDK, python and sed (`brew install gnu-sed` in OSX)

**2. [Network Service Orchestrator](https://developer.cisco.com/site/nso/)**: in order to test the configuration pipeline locally, you'll need to have a local install of NSO on your workstation. Furthermore, you will need to have the same versions of NSO and NEDs (network element drivers) installed as the _DevBox_ within the Sandbox. Using different versions _may_ work, but for best experience matching the versions exactly is recommended.

* Network Service Orchestrator 4.5.3
* Cisco IOS NED 5.8
* Cisco IOS XE NED 6.2.10
* Cisco NX-OS NED 4.5.10

Once downloaded, you would install NSO in OSX like this:

```
$ sh nso-4.5.3.darwin.x86_64.signed.bin
$ sh nso-4.5.3.darwin.x86_64.installer.bin ~/ncs-4.5.3 --local-install
```

You may download the required NEDs from your sandbox _devbox_ via SCP to your own workstation.

```
$ scp developer@10.10.20.20:/usr/src/nso/ncs-4.5.3-cisco-ios-5.8.signed.bin .
$ scp developer@10.10.20.20:/usr/src/nso/ncs-4.5-cisco-nx-4.5.10.signed.bin .
$ scp developer@10.10.20.20:/usr/src/nso/ncs-4.5-cisco-iosxr-6.2.10.signed.bin .
```

Install those NEDs, by running the following two commands for each downloaded binary...

```
$ sh <bin_file>
$ tar -xzvf <gz_file>
````

... and then move each uncompressed folder into `~/dev/ncs-4.5.3/packages/neds`, replacing the existing ones.

Check all required NEDs are installed.

```
$ ls $NCS_DIR/packages/neds/
```

Once you have installed these versions, you'll need to `source` the `ncsrc` file for this version before beginning the local development process.

```
$ source ~/ncs-4.5.3/ncsrc
```

_Don't forget to include this command in your startup shell (eg .zshrc)_

Now you can test your local NSO installation.

First, setup the required structure and environment in your preferred directory.

```
$ ncs-setup --dest ~/ncs-run
```

Then start the NCS daemon.

```
$ cd ~/ncs-run
$ ncs
```

Check if NCS started correctly.

```
$ ncs --status
```

Start the CLI to connect to NCS...

```
$ ncs_cli -u admin
```

... or connect via SSH (default password is `admin`).

```
$ ssh -l admin -p 2024 localhost
```

Point your browser to [http://localhost:8080/](http://localhost:8080/) (credentials arer `admin`/`admin`).

If everything works correctly you may now stop the NCS daemon.

```
$ ncs --stop
```

_Congrats, your NSO local installation is complete!_

**3. Python + Ansible** 

The network-as-code mechanism in this demonstration leverages both Ansible and NSO, with Ansible orchestrating the execution and configuration used by NSO to deploy to the network. In order to test locally, you'll need to have a Python environment (_virtual environment_ is recommended) that meets these requirements.

* [Python](https://www.python.org/downloads/) 3.6.5 or higher
* Ansible 2.6.3 or higher

Once you install them, and with your virtual environment active, install the requirements.

```
$ python3 -m venv env
$ source env/bin/activate
$ pip install -r requirements.txt
```

__All pre-requisites are now complete!__

Let's now dig into setting up the local environment in your workstation.

1. Clone a copy of the repository from GitLab to your local workstation. Use this command to ensure the demo credentials are embedded in the git configuration.

    ```
    $ git clone http://developer:C1sco12345@10.10.20.20/developer/cicd-3tier
    $ cd cicd-3tier
    ```

2. To simplify the setup and management of the local environment, a `Makefile` is included in the repository. Simply run `make dev` to do the following (to see the exact commands being executed for each of these steps, just take a look at the contents of your `Makefile`):

    a. Use NCS netsim to start a local simulation of the network including the core, distribution, and access devices
    b. Setup a local NCS project directory within the repo, start NCS and import in the netsim simulation
    c. Synchronize netsim and NCS
    d. Deploy the current network-as-code configuration to NCS and the network devices, using Ansible

    ```
    $ make dev
    ```

    Let's examine what is happening here, by going through the content of the `Makefile`.

    ```
    $ cat Makefile
    ```

    You will see the first line defines the different steps that are part of the `dev` directive.

    ```
    dev: netsim nso sync-from dev-deploy
    ```

    These steps are defined later in the same `Makefile`. You may also run them independently if you want to execute only that special step (eg. `make netsim`).

    __a. Start netsim__

    ```
    netsim:
        -ncs-netsim --dir netsim create-device cisco-ios core1
        -ncs-netsim --dir netsim add-device cisco-ios core2
        -ncs-netsim --dir netsim add-device cisco-nx dist1
        -ncs-netsim --dir netsim add-device cisco-nx dist2
        -ncs-netsim --dir netsim add-device cisco-nx access1
        -ncs-netsim start
    ```

    These `ncs-netsim` commands create netsim devices in the `netsim` directory, with the specified NEDs (ie. `cisco-ios` or `cisco-nx`) and a certain name (ie. `coreX`, `distX`, `accessX`). Then the last step starts them locally in your workstation. Netsim devices are a quick and easy way to test configuration changes locally, with no risk.

    You may check your netsim devices started correctly and their ports configuration, with:

    ```
    $ ncs-netsim is-alive
    $ ncs-netsim list
    ```

    You can also connect to your netsim devices CLI, and check with `show run` that nothing is configured yet. For example, to connect to `core1`:

    ```
    $ ncs-netsim cli-c core1
    ```

    __b. Start NSO__

    ```
    nso:
        -ncs-setup --dest . --package cisco-ios --package cisco-nx
        -ncs
    ```

    This `nso` directive prepares the current directory (`--dest .`) for a local NCS project, with the NEDs it will use (ie. `cisco-ios`and `cisco-nx`), and then it starts NCS.

    _It is important to note that NCS will automatically detect and add existing local netsim devices._

    You may login into NSO CLI and check the discovered devices (your netsim devices in this case) with:

    ```
    $ ncs_cli -C -u admin

    admin connected from 127.0.0.1 using console on JGOMEZ2-M-D2KW
    admin@ncs# show devices brief
    NAME     ADDRESS    DESCRIPTION  NED ID
    ------------------------------------------
    access1  127.0.0.1  -            cisco-nx
    core1    127.0.0.1  -            cisco-ios
    core2    127.0.0.1  -            cisco-ios
    dist1    127.0.0.1  -            cisco-nx
    dist2    127.0.0.1  -            cisco-nx
    admin@ncs#
    ```

    You may also see the devices configuration stored in NSO (not configured yet). For example, for `core1`:

    ```
    admin@ncs# show running-config devices device core1
    ```

    __c. Synchronize netsim and NCS__

    ```
    sync-from:
        -curl -X POST -u admin:admin http://localhost:8080/api/running/devices/_operations/sync-from
    ```

    This step will synchronize initial configurations _from_ netsim devices _into_ NCS. Check the configuration of your devices in NCS again, and you will see they include interfaces definitions now (eg. Loopback, Eth, FE).

    __d. Apply configurations__

    ```
    dev-deploy:
        -ansible-playbook --syntax-check -i inventory/dev.yaml site.yaml
        -ansible-playbook -i inventory/dev.yaml site.yaml
    ```

    This last directive uses ansible to first check the syntax ([linting](https://en.wikipedia.org/wiki/Lint_(software))), and then executes the `site.yaml` playbook on the list of devices defined in the `dev.yaml` inventory file.

    The inventory file (`dev.yaml`) lists the devices that will be configured by the playbook, with their hostnames, credentials (if necessary) and management IP addresses:

    * NSO
    * One access switch
    * Two core routers
    * Two distribution switches

    <p align="center"> 
    <img src="imgs/12nsoarch.png">
    </p>

    If you review the playbook itself (`site.yaml`) you will find it executes the following steps:

    1. Synchronize _old_ configurations from NSO to devices
    2. Push _new_ configurations to NSO
    3. Synchronize _new_ configurations from NSO to devices

    But specifically for step 2 you might be wondering _where are those new configurations?_

    Take a look at this extract from `site.yaml`, describing that step 2:

    ```
    - name: Push new configurations to NSO
    hosts: all
    connection: local
    gather_facts: no

    tasks:
        - name: Device configuration
        nso_config:
            url: "{{ nso.url }}"
            username: "{{ nso.username }}"
            password: "{{ nso.password }}"
            data:
            tailf-ncs:devices:
                device:
                - name: "{{ nso_device_name }}"
                tailf-ncs:config:
                    "{{ config }}"
    ```

    That _tasks_ description uses the [`nso_config` module](https://docs.ansible.com/ansible/latest/modules/nso_config_module.html), and provides the required NCS URL, username and password, as parameters defined in the inventory file mentioned before.

    The `data` section is the one that describes what configuration to apply, and there you may find you need to provide the _device_name_ and _config_. Device names come again from the inventory file. BUT configurations are stored in the `host_vars` directory, where ansible looks for variables as required. That directory stores individual yaml files, one per device, with the required configuration to apply to NCS devices.

    These configuration files in the `host_vars` directory will be important for us throughout the demo, as they store the configuration we want to apply, and therefore we will use them to apply changes in our network.

    After `dev_deploy` is completed you will see configurations correctly applied (and synchronized) to your netsim devices and NCS ones. You may check it with the same commands described in previous steps. For example, for `core1`:

    ```
    $ ncs-netsim cli-c core1

    admin connected from 127.0.0.1 using console on JGOMEZ2-M-D2KW
    core1# show running-config
    ```

    And...

    ```
    $ ncs_cli -C -u admin

    admin connected from 127.0.0.1 using console on JGOMEZ2-M-D2KW
    admin@ncs# show running-config devices device core1
    ```

    _(Note: after you complete the rest of this demo, when you don't need the local environment anymore, you can easily delete everything by running `make clean`. It will shutdown netsim devices, NSO, and delete any related remnants.)_

### <a name='Runningthedemo'></a>Running the demo

Our demonstration will include the following architecture and elements, to show how a completely automated CICD pipeline could be applied to a network configuration environment across a complete network, including test and production environments.

<p align="center"> 
<img src="imgs/20pipeline.png">
</p>

The flow will be as follows: our network operator will interact with GitLab to perform any configuration changes. Ansible and NSO will deploy those changes into a virtual _test_ environment (with VIRL), and run automated tests (with pyATS) to verify the expected results after the change. If everything goes well, then our VCS will run the same process in the _production_ environment to implement those changes in the real network.

Your GitLab Version Control Server (VCS) is ready. Please find the new infrastructure-as-code repository by pointing your browser to [http://10.10.20.20/developer/cicd-3tier](http://10.10.20.20/developer/cicd-3tier), and login with `developer`/`C1sco12345`. Leave that window open, as we will use it to run the demo.

<p align="center"> 
<img src="imgs/21gitlab_project.png">
</p>

The repository (or _repo_) stores all required files and configurations to work with during the demo. Some key elements are the following ones:

* `.gitlab-ci.yml` is the pipeline definition, including all different steps to follow in the automation process
* `virl` is a folder used by VIRL to define the emulated architectures (_test_ and _prod_)
* `tests` is a folder used by pyATS for automated testing
* `group_vars`, `host_vars` and `inventory` are folders used by Ansible to automate configurations deployment

__If__ you did not follow the optional local setup process, please clone a copy of the repository from GitLab to your local workstation (if you already did it in the previous section, please skip this step). Use this command to ensure the demo credentials are embedded in the git configuration.

```
$ git clone http://developer:C1sco12345@10.10.20.20/developer/cicd-3tier
$ cd cicd-3tier
```

You will need to edit some of the files in this local repo, so please choose your favorite editor / IDE ([integrated development environment](https://en.wikipedia.org/wiki/Integrated_development_environment)). One possible option is [Visual Studio Code](https://code.visualstudio.com/), but you could also just defer to using something simpler like `vi` or any other text editor.

First of all, please take a look at the `.gitlab-ci.yml` pipeline file definition.

```
$ cat .gitlab-ci.yml
```

You will see our pipeline includes the following steps:

1. Use Ansible to validate configurations that need to be applied to NSO and network devices are syntactically correct ([linting](https://en.wikipedia.org/wiki/Lint_(software))), for the three environments: _dev_ (local), _test_ and _production_.
2. Deploy those configurations to the _test_ environment.
3. Run automated testing in the _test_ environment to make sure the resulting network state is the expected one.
4. Deploy those configurations to the _production_ environment. In this case you will see it specifies `when: manual`, meaning we would like to explicitly initiate the deployment process to _production_. `allow-failure: false` means that in case of failure when deploying in _production_ the system should automatically roll-back to the previous state.
5. Run automated testing in the _production_ environment to make sure the resulting network state is the expected one. 

_Important note: for our demonstration we will use two simulated environments: test and production. It is more convenient for us to use a simulated environment for production, but in a real-world scenario the production environment would be built by real equipment from the production network._

<p align="center"> 
<img src="https://media.giphy.com/media/G0GfSkTMYKNWw/giphy.gif">
</p>

Let's take a look at our network configurations.

```
$ cd host_vars
$ ls
access1.yaml core1.yaml   core2.yaml   dist1.yaml   dist2.yaml
```

As you can see there is one yaml file per device in our network. Those files will be the ones you need to modify to perform changes in your network.

In a real-world scenario each network developer would have cloned this repository in their local machine, and work in their own local copy, via a specific branch. For our demo we will be one of those network developers, and propose changes from our local git repo.

For example, let's say we would like to change the OSPF router-id of our core1 router, from `.1` to `.101`. We would have to edit `core1.yaml`, look for the relevant configuration line...

```
ospf:
    - id: 1
      network:
      - area: 0
        ip: 172.16.0.0
        mask: 0.0.0.3
      - area: 0
        ip: 172.16.0.4
        mask: 0.0.0.3
      - area: 0
        ip: 172.16.0.16
        mask: 0.0.0.3
      - area: 0
        ip: 192.168.1.1
        mask: 0.0.0.0
      router-id: 192.168.1.1
```

... and change that last line to the desired value.

```
      router-id: 192.168.1.101
```

Save the file.

Right now you have _only_ modified a local text file in your workstation. And _git_ knows about it.

```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   core1.yaml

no changes added to commit (use "git add" and/or "git commit -a")
```

As long as we are happy with this change, we need to add the modified file to our next _git_ commit.

```
$ git add core1.yaml
$ git commit -am "Update OSPF router-id from .1 to .101"
[master 0b24c9b] Update OSPF router-id from .1 to .101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Now is the time to send our configuration change to the remote repo in the VCS GitLab server.

```
$ git push
warning: redirecting to http://10.10.20.20/developer/cicd-3tier.git/
Counting objects: 4, done.
Delta compression using up to 12 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 389 bytes | 389.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0)
To http://10.10.20.20/developer/cicd-3tier
   00f2b65..1b70da9  test -> test
```

Go back to the browser window that pointed to your GitLab repo at [http://10.10.20.20/developer/cicd-3tier](http://10.10.20.20/developer/cicd-3tier), and you will see the update there.

<p align="center"> 
<img src="imgs/22gitlab_pipeline_start.png">
</p>

Pushing your proposed change in the new `core1.yaml` file will automatically start the pipeline defined in the `.gitlab-ci.yml` file. It is applied on the _test_ network, and you may check its execution in real-time by clicking on the _CI / CD_ section in the left bar. In this example we would be running pipeline #3.

<p align="center"> 
<img src="imgs/23gitlab_pipeline_run.png">
</p>

As you can see the pipeline includes 3 different _stages_: _validate_, _deploy_to_test_ and _verify_deploy_to_test_.

These are coming from the pipeline definition in your `.gitlab-ci.yml` file.

Clicking on each one of these stages will show you the specific steps followed in there:

1. _validate_

<p align="center"> 
<img src="imgs/24gitlab_pipeline_validate.png">
</p>

This stage starts a container running Ansible and do the syntax checking of proposed configurations, including changes, in all 3 environments: _dev_ (local), _test_ and _prod_.

2. _deploy_to_test_

<p align="center"> 
<img src="imgs/25gitlab_pipeline_deploy.png">
</p>

Second stage starts a container running Ansible to sync existing configs from NSO to devices, then apply configuration changes to NSO, and finally sync again configs from NSO to devices.

3. _verify_deploy_to_test_

<p align="center"> 
<img src="imgs/26gitlab_pipeline_test.png">
</p>

The final stage starts a container running a pyATS image and run automated tests based on the content of `tests/validation_tasks.robot` and `tests/test_testbed.yml`. The defined set of tests includes not only reachability, but also number of expected OSPF neighbors and interfaces in each network device after the applied changes. 

The whole process will take like 5 minutes, until you can see the 3 stages completed successfully in the _test_ environment.

<p align="center"> 
<img src="imgs/27gitlab_pipeline_complete.png">
</p>

At that point your proposed network configuration change has been completely validated in a VIRL-simulated _test_ environment, and you are now good to propose it to be applied in the real _production_ network.

You can do that by _requesting_ to merge the content of the git _test_ branch into the git _production_ branch.

<p align="center"> 
<img src="imgs/28merge1.gif">
</p>

<p align="center"> 
<img src="imgs/29merge2.gif">
</p>

If you go back to the pipeline section you will see there is a new pipeline there, #4. It includes the same steps as the previous one, but the main difference is that this pipeline is being applied to the _production_ network.

<p align="center"> 
<img src="imgs/30gitlab_pipeline_prod_blocked.png">
</p>

As you can see the _production_ pipeline has the same 3 stages as the one we applied previously in the _test_ environment. However when running it, the pipeline appears as _blocked_ before running the _deploy_to_prod_ stage. The reason is we configured `when: manual` in the `.gitlab-ci.yml` pipeline definition file, so that we _had_ to manually confirm we want to actually initiate the deployment in the production network. This is a configuration decision, and maybe useful if you would like to perform the actual change during a maintenance window.

In order to move forward with the pipeline we need to confirm it manually by pressing the _play_ button.

<p align="center"> 
<img src="imgs/31gitlab_pipeline_prod_confirm.png">
</p>

It will automatically start deploying our configuration changes into the _production_ network. If everything goes well, it will successfully complete this stage and move to the next one, to test the results after changes are implemented. 

After 5 minutes, by the end of this process you should see the complete pipeline has been successfully executed, and your proposed changes have been tested and finally applied to the _production_ network.

<p align="center"> 
<img src="imgs/32gitlab_pipeline_prod_complete.png">
</p>

__CONGRATULATIONS! You have completed the NetDevOps demo!__ 

### <a name='Summary-1'></a>Summary

In this NetDevOps demo you have seen a modern approach into version-controlled automated network configuration and testing. The scenario describes how multiple network operators would be able to propose configuration changes, in the same way developers do it for code: by mean of git branches. A standard version control server provides multiple benefits, like automated pipelines, version control and tracking, rollback cababilities, etc. During the demo you have also experienced the benefits of being able to locally verify syntax for proposed changes before submitting them. Also how a simulated environment helps verifying proposed changes are correct, before applying them into the production network. Finally, the set of automated tests helps making sure proposed changes have not had unexpected results on critical business-relevant functionality. This way you have experienced end-to-end automation and testing in a scalable and error-free approach.