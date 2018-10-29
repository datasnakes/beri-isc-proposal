beRi Environments for R Installations
================
October 28, 2018

Preface
=======

Reproducibility is a cornerstone of science advancement \[[1](https://www.nih.gov/research-training/rigor-reproducibility)\]. Given R's popularity in biotech and research as well as the aims of rOpenSci, there is a great need for frameworks or tools that improve reproducibility.
The [containerit](https://github.com/o2r-project/containerit) and [rocker](https://github.com/rocker-org/rocker) projects are examples that incorporate containerization via docker. However, Docker is not widely adopted by academic institutions and other R users due to potential system [vulnerabilities](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface) and the requirement for administrative privileges. In these situations, [conda](https://docs.anaconda.com/anaconda/user-guide/tasks/use-r-language/) is a preferred and popular framework for reproducibility. In other cases, users to different R packages including [packrat](https://github.com/rstudio/packrat), [checkpoint](https://github.com/RevolutionAnalytics/checkpoint) for MRAN, [miniCRAN](https://github.com/andrie/miniCRAN), [drat](https://github.com/eddelbuettel/drat), [workflowr](https://github.com/jdblischak/workflowr), and [rbundler](https://cran.r-project.org/web/packages/rbundler/index.html). While these packages have tremendous value to R users, they limit users to using an interactive R session.

Scope
=====

Comparing Programming Languages
-------------------------------

Many popular programming languages in the bioinformatics community have a command-line package manager that accesses a standard or customizable package repository. Python has [pip](https://pypi.org/project/pip/) and [PyPi](https://pypi.org/), JavaScript has the [npm-client](https://github.com/npm/cli) and [npm-registry](https://docs.npmjs.com/misc/registry), and Ruby has the [gem-client](https://github.com/rubygems/rubygems) for [RubyGems.org](https://rubygems.org/). [CRAN](https://cran.r-project.org/) and [Bioconductor](https://www.bioconductor.org/) are package repositories, but R lacks a tool equivalence to pip and npm that installs packages globally or locally via the command-line. Improving the management of R package installation and removal could boost reproducibility for projects and increase its usability in pipelines.

In addition to package management, R users sometimes need to install R from source. High-performance computing (HPC) systems generally need R installed from source because system administrators desire specific global configurations. HPC system users also need to install R in locally order to utilize a specific version of R or a package that depends on a specific R version. Other programming languages such as Python ([pyenv](https://github.com/pyenv/pyenv)) and Ruby ([rbenv](https://github.com/rbenv/rbenv) and [ruby-build](https://github.com/jdblischak/workflowr)) have third-party interfaces that manage the installation and management of various versions of the language which aids in the reproducibility of an environment for projects as well as localization of their dependencies.

With these ideas in mind, we propose [beRi](https://github.com/datasnakes/beRi), a suite of tools for managing R. beRi will depend on three independent tools that work towards providing new options for managing R virtual environments, R package management, R installation management, and other R utilities all from the command line. It will be built on top of this toolset, and provide a more holistic experience by adding project management and configuration.

Comparing R Workflows
---------------------

R is [rising in popularity](https://blog.revolutionanalytics.com/popularity/) in the healthcare industry, academic institutions, and government entities as seen by a recent stack overflow analysis on R \[[2](https://stackoverflow.blog/2017/10/10/impressive-growth-r/)\]. Our target audience for the beRi suite of tools are the people in those fields: bioinformaticians, other researchers in basic or clinical sciences, clinicians, data analysts, system administrators, HPC system users, and R package developers. beRi will also appeal to new users of R who are used to Python and/or command line interfaces. While the current workflow options allow R users to successfully perform analyses and manage projects, a good sign of a healthy programming ecosystem is the availability of multiple solutions to the same problem. The most popular options remain to be conda/bioconda, packrat/jetpack, and docker (rocker/containerit) respectively.

Docker seems to be a holistic solution as containerization produces an exact replicate of your project's environment at the operating system level. The primary barrier for Docker is that there are few people in the academia and the healthcare industry who are utilizing this technology. While packrat is an excellent tool for "containing" packages within a project, its builds for large projects can be computationally intensive and slow. Anaconda's ecosystem for R is most similar to our approach and has also been the main question about beRi from the rstats Reddit community:

> "How does this differ from conda?"  
> "What are the benefits of using beRi over conda?"

The main goal of beRi is to connect users directly to the R's most popular repositories (CRAN, Bioconductor, MRAN, GitHub, etc.) and provide a means for micromanaging the R environment in one tool set with an intuitive command line interface (CLI). While conda accomplishes some of the same goals, the CLI is attached to anaconda's repositories, which limits R users from easily integrating latest R packages or updates. beRi is not meant to replace existing tools but to incorporate a new one into the R ecosystem.

The Team
========

[Team-beRi](https://github.com/orgs/datasnakes/teams/team-beri) is a group of individuals who are passionate about reproducibility in science but have experienced the limitations of existing tools. Bridging that gap was the motivation for prototyping beRi at [hackseq18](https://www.hackseq.com/) and winning the popular votes exhibits that there is a strong interest and need for this tool in at least the bioinformatics community.

We are familiar with Python, R, and Bash from our daily work and plan to use these languages as the building blocks of the project. Some of us have software engineering experience to help with the development process. Since we will be both makers and users of this tool and are surrounded by fellow R users in science, we can continually improve beRi by adapting frequent feedback from ourselves and our community who will be the end users of beRi.

Our team is divided into "tiers" of individuals based on experience level and willingness to contribute in the long term. We are using the terms "Project Manager", "Lead Maintainer", and "Contributor" to manage our responsibilities. The beRi team will develop under the [DataSnakes](https://github.com/datasnakes) organization on GitHub. All of the current contributors participated in developing beRi at the Hackseq 2018 bioinformatics hackathon in Vancouver, BC, Canada.

Project Manager
---------------

beRi was conceived by [**Rob Gilmore**](https://www.linkedin.com/in/robert-gilmore-7b451592). At hackseq18, his leadership skills and project management experience guided the team to success as the winners of the hackathon. Gilmore's skill sets came from his education in [Biomedical Engineering](https://www.abe.msstate.edu/academics/undergraduate/biomedical-engineering/curriculum/) at Mississippi State University, professional development at [Engineering Research and Development Center](https://www.erdc.usace.army.mil/) and his extensive Bioinformatics experience as a Researcher in the fields of Genetics and Psychiatry using non-human primate models. In his current position at the [University of Mississippi Medical Center](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html), he has utilized the [Mississippi Center for Computational Research's](https://mcsr.olemiss.edu/research/) HPC system to manage and develop large-scale bioinformatics pipelines using Linux, Windows, Python, R, and Bash.

Lead Maintainers
----------------


-   **Shaurita D. Hutchins** is a Researcher II in the [Department of Psychiatry and Human Behavior's division of Neurobiology and Behavior Research](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html) at the University of Mississippi Medical Center. She received her Master of Science in Biological Sciences from Mississippi College. She is proficient with Python package development, R package development, web development (including shiny apps and flask apps), genomics pipeline development, data analysis using Python, and data visualization using R. She is the co-creator of [Datasnakes](https://www.datasnakes.org/), an open-source and open-science organization and acts as the maintainer for multiple repositories/projects in it.

-   **Santina Lin** is a software engineer at Microsoft and works on maintaining the [RevoScaleR](https://docs.microsoft.com/en-us/machine-learning-server/r-reference/revoscaler/revoscaler) package as well as adding R and Python runtime to SQL server. She completed her master's degree in Bioinformatics at the University of British Columbia where she used R for data analysis and visualization and taught R and statistics in a graduate level course as a teacher assistant.

-   **Bruno Grande** is a Ph.D. candidate working on cancer genomics in [Ryan Morin’s lab](https://morinlab.github.io/) at Simon Fraser University. An advocate for best practices in data science, he was a regular instructor and organizer in [Software Carpentry](https://software-carpentry.org/) and [SciProg](http://sciprog.ca/) workshops to help people overcome the reproducibility crisis in research. He dreams of publishing a Git repository that can reproduce every analysis from his thesis with a single command. beRi plays an important role in making that achievement.

The Plan
========

beRi is a suite of Python packages which is composed of a virtual environment manager for R ([renv](https://github.com/datasnakes/renv)), an R installation and R version manager ([rinse](https://github.com/datasnakes/rinse)), and an R utility tool for installing packages, managing native R configuration files, and setting up local CRAN-like repositories ([rut](https://github.com/datasnakes/rut)). These packages will be developed in separate repositories as standalone or fully independent CLI’s, and [beRi](https://github.com/datasnakes/beRi) will also be developed in a separate repository. However, the beRi CLI will depend on the other three packages, which will provide an enhanced and comprehensive experience as well as project management support.

To begin, these packages will be built with Python for Linux systems starting with the latest Ubuntu release with long-term support. We will optimize our Python packages for the R ecosystem by using the [R manuals](https://cran.r-project.org/doc/manuals/), which can be found on the CRAN website. Following stable builds, we will test under various Linux distributions, followed by Windows 7 and above, and finally, macOS systems. These Python packages are currently being managed by [poetry](https://github.com/sdispater/poetry) for ease of use and intuitive workflow; however, we have also considered switching to [pipenv](https://github.com/pypa/pipenv) in the future if poetry does not satisfy our needs. [click](https://palletsprojects.com/p/click/) will be used to build the command line interface.

Standalone CLI - renv
---------------------

renv, short for R virtual environment, is a Python-style virtual environment package for R. It will use Python's venv [EnvBuilder](https://github.com/python/cpython/blob/3.6/Lib/venv/__init__.py#L17) module along with [cookiecutter](https://github.com/audreyr/cookiecutter) in order to deploy the proper shell scripts (activate) and R configuration files (.Rprofile, .Renviron). A directory will be utilized in the users home directory (\~/.renv) as a repository for any environment created. The environments created with renv will not only be configurable with the .Rprofile/.Renviron files, but also a custom renv.yaml config file located in the root of the environment directory (\~/.renv/<env-name>/renv.yaml). This file will also allow the user to set up default environment variables, CRAN mirrors, and to set default packages to install. The renv CLI will also contain a default yaml file for all newly created environments.

Standalone CLI - rinse
----------------------

rinse, short for R installer, is currently a simple installer for the latest version of R. In the future, we would like to implement features similar to [pyvenv](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md). This includes installing/uninstalling R from source, managing some of R's dependencies, and switching in between version of R. Installing Microsoft R-Open or [other R implementations](https://en.wikipedia.org/wiki/R_(programming_language)#Implementations) will also be considered. After Linux development is stable, a long-term goal is to provide support for the installation of Rtools on Windows.

Standalone CLI - rut
--------------------

rut, short for R utilities, will be an R package manager that will also be able to set up local CRAN-like repositories and manage custom R configuration files such as .Rprofile and .Renviron. In its current iteration, rut can install packages from CRAN using R's remotes package. As the least developed package and likely the most difficult to implement, rut will require more attention in order to develop it with best practices and effectively integrate it with beRi. With regards to rut, we have discussed wrapping the R CMD INSTALL command, integrating a simpler version of [jetpack](https://github.com/ankane/jetpack), or utilizing [rpy2](https://rpy2.readthedocs.io/en/version_2.8.x/) for getting interpreter specific information.

Integrated CLI - beRi
---------------------

The primary and initial goal for the beRi suite of tools was to create standalone packages. We looked to other programming languages for inspiration and R manuals and various documentations to keep the R ecosystem and R community in mind. The integration of the three tools under the beRi CLI will help us develop a custom project workflow with one cohesive interface. Once the standalone tools are functional, we will develop an analogous YAML configuration workflow for each package so that we can configure our environment with one YAML file and develop a beRi file-system. The ~/.beRi folder, for instance, will contain any renv environments, R installations, CRAN-like repositories, and .libPaths() associated with beRi processes by default. While the development of beRi will affect some of the changes we make to the standalone CLI, we will never limit renv, rinse, or rut by linking them to each other or to beRi.

Project Milestones
==================

The planning phase is a continuous effort. GitHub will be the primary means of managing the project. [ZenHub](https://www.zenhub.com/blog/how-to-use-github-agile-project-management/), which integrates with GitHub, provides agile project management that will help contributors understand the project from a hierarchical view. Using ZenHub Milestones, Estimates, Epics, and Pipelines will help us generate timelines based on criteria such as difficulty, stage of development, and the priority level of various tasks. We will use ToDoist for general task management and Slack for higher level discussion among collaborators and users of beRi.

beRi for Linux (Version 1.0)
----------------------------

The beRi suite of tools will first be tested under the Linux OS. The dates below represent estimated points of completion and maybe be modified after further analysis of our workload with ZenHub.

-   Development for Linux specifically Ubuntu LTS - June 2019
-   renv - February 2019
    -   Create virtual environments - Completed
    -   Resolve minor R warnings - January 2019
    -   Code testing and linting - February 2019
    -   Create documentation using sphinx and tutorials - February 2019
    -   Publish package to PyPi - February 2019
-   rut - March 2019
    -   Extend to global package management - February 2019
    -   Code testing and linting - March 2019
    -   Create documentation using sphinx and tutorials - March 2019
    -   Publish package to PyPi - March 2019
    -   Secondary Goal - Manage local CRAN-like repositories
    -   Secondary Goal - Extend rut for managing projects with packrat
-   rinse - April 2019
    -   Manage the Installation of R from source - March 2019
    -   Integrate Microsoft R Open - April 2019
    -   Code testing and linting - May 2019
    -   Create documentation using sphinx and tutorials - May 2019
    -   Publish package to PyPi - May 2019
    -   Secondary Goal - Reverse engineer pyenv
    -   Consider other distributions of R
-   beRi - May 2019
    -   Incorporate YAML configuration for renv, rut, rinse - April 2019
    -   Begin writing manuscript for beRi - April 2019
    -   Incorporate YAML configuration for beRi - May 2019
    -   Create a beRi file system - May 2019
    -   Code testing and linting - June 2019
    -   Creation of documentation and tutorials = June 2019
    -   Publish package to PyPi - June 2019
    -   Complete writing manuscript for beRi - July 2019
    -   Secondary Goal - Add beRi projects
-   Development for other Linux Distributions - July 2019

How Can The ISC Help
--------------------

The total costs of this project will be $22,700, split into the following items:

-   Travel costs for a project mid-completion meeting in a location where the team leaders can convene ($3000).
-   Compensation to team leaders/members and project dissemination including publication costs ($19,000).
-   Project management tools/subscriptions including Slack and Todoist and a `.com` or `.org` website via Namecheap ($700).

Dissemination
-------------

This project will use a permissive open-source license (MIT). The development process will take place in the [datasnakes](https://github.com/datasnakes) organization on GitHub, and beRi and its suite of tools will be submitted to PyPi when it is completed. Discussion and contributions will be encouraged through GitHub issues and the R mailing lists.

We will regularly post updates about this project on beRi's website and on the datasnakes' twitter account. We have created a [gitter](https://gitter.im/CRANbeRi/Lobby) chat room to allow other developers to interact with us and collaborate with us. Additionally, we intend to provide articles for the R Consortium blog at the start, middle, and end of the project to document the project's progress. A publication in scientific journal focused on software is foreseen as we complete beRi.
