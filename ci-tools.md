# CI Tools for Kubernetes
This article is a study to find the most suitable CI tool for Kubernetes. In this article, CI tools that are likely to be most suitable for our infrastructure are examined under 4 headings in terms of features, pros, cons and cost.

## CircleCI
### Features
1. CircleCI is a cloud-based system — no dedicated server required, and you do not need to administrate it. However, it also offers an on-prem solution that allows you to run it in your private cloud or data center.
2. It has a free plan even for a business account.
3. Rest API — you have an access to projects, build and artifacts. The result of the build is going to be an artifact or the group of artifacts. Artifacts could be a compiled application or executable files (e.g. android APK) or metadata (e.g. information about the tests`success).
4. CircleCI caches requirements installation. It checks 3rd party dependencies instead of constant installations of the environments needed.
5. You can trigger SSH mode to access container and make your own investigation (in case of any problems appear).
6. That’s a complete out of a box solution that needs minimal configuration\adjustments.

### Pros
1. Fast builds.
2. CircleCI has a free plan for enterprise projects.
3. It’s easy and fast to start.
4. Lightweight, easily readable YAML config.
5. You do not need any dedicated server to run CircleCI.
6. The have very customizable platform, with intelligent pricing as you scale.
7. Great customer service and it is easy to use. There are also constant product updates based on reviews, which is not common with most other products.
8. It offers a lot of useful features, most of which can be used with the free plan. Customer service is also excellent.
9. GitHub integration.

### Cons
1. CircleCI supports only 2 versions of Ubuntu for free (12.04 и 14.04) and MacOS as a paid part.
2. Despite the fact CircleCI do work with and run on all languages tt supports only the following programming languages “out of the box”: Go (Golang), Haskell, Java, PHP, Python, Ruby/Rails, Scala.
3. Some problems may appear in case you would like to make customizations: you may need some 3rd party software to make those adjustments.
4. Also, while being a cloud-based system is a plus from one side, it can also stop supporting any software, and you won’t be able to prevent that.
5. The config is probably the worst of the cloud CI software.
6. Does not cache docker images.
7. Changes the environment without warning.

### Costs
[Go to the Web Site](https://circleci.com/pricing/?utm_medium=scc&utm_source=gartnerDigital_capterra&utm_campaign=scc-gartnerDigital_capterra-dg-ao-multi-en-default--traffic&utm_source=GetApp)


## Travis CI
### Fetaures
1. Encrypts secure environment variables or files.
2. Automatic integration with GitHub.
3. Accesses the warehouse to create pull requests.
4. Publishes to multiple cloud services.
5. CLI client and API for scripting.
6. Comes with free cloud based hosting with no maintenance or management.
7. Recreates the virtual machine after each build.
8. Services databases, message queues, etc.
9. Supports 21 languages, including Android, C, C#, C, Java, JavaScript (with Node.js), Perl, PHP, Python, R, Ruby, etc.
  
### Pros
1. There is no dedicated server needed.
2. Lightweight and easy to set up.
3. Builds matrix feature.
4. Free for open source projects.
5. Allows block tests to run in parallel.
6. Unlimited open source projects with full functionality.
7. Multiple build environments and target platforms (eg Node).

### Cons
1. Limited options for customization.
2. Enterprise plans come with a cost.
3. Higher starting price point for paid plans.
4. No completely free account for projects with private repos.

### Costs
[Go to the Web Site](https://www.travis-ci.com/pricing/)


## Github Actions
### Pros
1. GitHub actions are just consecutive docker runs. Very easy to reason about and debug. Reproducing the build environment for container-based Travis is possible, but more difficult. On GitHub actions it's just a docker build docker run away.
2. The individual actions in a workflow are isolated by default. You can use a completely different computing environment for, say, compilation and testing. Travis CI (and I think other "traditional" CI) would run all "stages" (~ actions) in the same computing environment. Again, GitHub actions are much easier to reason about and debug.
3. The main.workflow spec (a subset of the HCL and really just a directed acyclic graph) is open source. The whole thing is a pretty thin wrapper around Docker anyway, so platform lock-in is arguably minimal.
4. There are already open-source reimplementations of GitHub actions, such as act for local testing.
5. You have ready access to the GitHub API with (somewhat limited) authentication out of the box.
6. There might be a vibrant community (marketplace?) where people can share actions. For example, I'm reusing deploy actions build by different people in different ecosystems.
7. A directed acyclic graph (DAG) and the visual editor for main.workflows is perhaps a good way to model CI/CD in particular and workflows in general. Takes some getting used to, but generalises well.
8. GitHub actions can do a whole lot more than just CI! You've got basically the whole API at your fingertips as inputs and outputs.

### Cons
1. No native caching. You get image and layer caching (it's complicated), but nothing else. For build artifacts, you have to roll your own cache (via AWS, Azure, etc. ...), which can be a lot of work. (You can see a hacky setup here.
2. Surprisingly, no support for pull requests from forks. It's again a bit complicated, and understandable from a security standpoint, but it's currently not possible to run actions a) against the secrets of the receiving repo of a fork PR (base), and/or b) against the would-be merge result of a fork PR (that's what travis does). For a workflow that involves forks, that makes GitHub actions largely unuseable as CI/CD tool.
3. Single platform, it's just whatever you can run inside docker, so some Linux distro. That seems unlikely to change, but might be an acceptable limitation. You can always add an action to call other cross-platfrom CI/CD services.
4. The documentation is still pretty sparse. Not much in the way of best practices or scaffolding.
5. The quality and breadth of published GitHub actions (at least on the marketplace) is still pretty low / limited. We'll see whether that takes off.
6. No great way to unit-test actions.

### Costs
[Go to the Web Site](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)


## GitLab CI
### Features
1. Version control and repository management based on Git.. 
3. Individual Issues Deadlines.
4. Development analytics.
5. Create/Track milestones, track issues and create a wiki for team communications.
6. Service desk or ticketing system.
7. Issues & Merge Requests Templates.

### Pros
1. Allows us to collaborate, review code, manage projects and monitor issues.
2. Outstanding pipeline capabilities.
3. Community based environment.
4. Built with enterprise grade security capabilities.
5. Properly manages the Entire Lifecycle of Applications.
6. Easy to add jobs and manage conflict issues.
7. Robust CI with awesome Docker support.
8. Simple configuration.

### Cons
1. Advance security and portfolio management is available in Ultimate plan only.
2. No analytics for pipeline tracking.
3. Its interface may be somewhat slower than the competition.
4. There are some common problems with repositories.

### Costs
[Go to the Web Site](https://about.gitlab.com/pricing/)

## TeamCity
### Features
1. TeamCity provides full support for its integration with some key tools like your build tool, version control, issue tracker, and package repository. TeamCity’s support for these and many more integrations is widely known in the industry and is a major element of its structure.
2. TeamCity provides numerous features to facilitate excellent continuous integration protocols, such as: 
   - Remote runs and pre tested commits
   - Problems with test management
   - Automatic investigation assignment
   - On the fly build progress reporting metadata in test results and so on.
3. TeamCity utilizes cloud computing by hosting build agents on farms like Amazon EC2, Microsoft Azure, Google Cloud, VMware vSphere, and Kubernetes. It dynamically scales its build agents on these platforms to automatically start and stop depending on your workload.
4. TeamCity helps developers improve their code quality by utilizing IntelliJ IDEA and ReSharper to analyse and inspect Java and .NET code. TeamCity also supports other out of the box plugins to improve your code in major areas.

### Pros
1. Developers have praised TeamCity for having a simple, effective and aesthetic UI. Its simple user interface has been a significant selling point for it, as the interface design follows modern design principles, which optimizes user experience and work effectiveness. 
2. TeamCity’s simplistic design makes it easy for users to locate functions on the software and carry out tasks quickly without having to source external assistance. The invention also features intuitive gestures like drag and drop, making workflow more fluid and enjoyable.
3. TeamCity has the best .NET integration framework of any other CI/CD tool. Many .NET technologies are easily integrated into TeamCity, including code coverage analysis, .NET testing frameworks, and static code analysis.
4. TeamCity easily integrates different environments like Jira, Visual Studio, Bugzilla, etc., as well as many version control systems like GitLab, GitHub, and VCS.
5. Unlike Jenkins, TeamCity does not rely on plugins to carry out basic tasks. Instead, the majority of the functions on TeamCity are built in and do not require additional plugins to work with. Among the features are building chains and dependencies, source control, comprehensive historical data and statistics, monitoring the source project of multiple projects, and so on.

### Cons
1. A major turn off for most developers regarding TeamCity is that it is not free. Although TeamCity offers a free professional license for open source projects, to use it at an enterprise level requires a purchase fee of $1,999, with a single build agent costing $299 amidst other pricing options in its offering.
2. TeamCity lacks enough resources or documentation on its API integration. Apart from the information provided by Jetbrains in its guide, there is not much information one can find on specific topics that one could need. The focus on TeamCity’s RESTful API page even states that it is not complete and only provides basic information.

### Costs
[Go to the Web Site](https://www.jetbrains.com/teamcity/buy/#on-premises)