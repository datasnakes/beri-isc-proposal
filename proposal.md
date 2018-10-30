---
title: beRi Environments for R Installations
date: October 28th, 2018
bibliography: bibliography.bib
citation-style: vancouver.csl
---

**[Robert Gilmore](https://github.com/grabear)^1^,
[Bruno Grande](https://github.com/edzer)^2^,
[Shaurita Hutchins](https://github.com/sdhutchins)^1^,
[Santina Lin](https://github.com/santina)^3^**

^1^ University of Mississippi Medical Center, Jackson, MS, United States  
^2^ Department of Molecular Biology and Biochemistry, Simon Fraser University, Burnaby, BC, Canada  
^3^ Microsoft Corporation, Redmond, WA, United States  

# Preface

Reproducibility is a cornerstone of scientific advancement.[@noauthor_undated-de]
In computational research, we have the unique opportunity of achieving perfect reproducibility given the theoretical capability of computers to reproduce the same output given the same input and software.
In practice, this goal is difficult to achieve because of a number of technical challenges.
Here, we focus on the challenge of creating and managing environments for the R statistical programming language.
We focus on R because of its popularity in data science, especially in the academic and healthcare contexts,[@Robinson2017-xv] and because we rely heavily on the language ourselves.
Coming from Python, we found the toolset in R for managing environments to be lacking in comparison to Python offerings such as [virtualenv](https://virtualenv.pypa.io/en/latest/) and [conda](https://conda.io/docs/).
Projects relying on containerization (*e.g.* Docker) such as [containerit](https://github.com/o2r-project/containerit) and [rocker](https://github.com/rocker-org/rocker) are powerful but ultimately fail to address the needs of those working at institutions where containerization technologies have not yet been adopted.
Slow adoption can be partially attributed to potential [system vulnerabilities](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface) from requiring administrative privileges.
Additional tools that do not rely on administrative privileges include R packages such as [packrat](https://github.com/rstudio/packrat), [checkpoint](https://github.com/RevolutionAnalytics/checkpoint), and [rbundler](https://cran.r-project.org/package=rbundler).
However, these packages fail to provide conda's combination of flexibility, speed, and convenience from the command line.
Our goal is to learn from conda's success and develop a set of modular packages that enable powerful environment management in R.

# Scope

## Comparing Programming Languages

Most popular programming languages in computational research have a command-line package managers to access standard or custom repositories.
For instance, Python has [pip](https://pypi.org/project/pip/) for [PyPi](https://pypi.org/) or [conda](https://conda.io/docs/) for [Anaconda Cloud](https://anaconda.org/), JavaScript has [npm-client](https://github.com/npm/cli) for [npm-registry](https://docs.npmjs.com/misc/registry), and Ruby has [gem-client](https://github.com/rubygems/rubygems) for [RubyGems.org](https://rubygems.org/).
R offers comprehensive package repositories such as [CRAN](https://cran.r-project.org/) and [Bioconductor](https://www.bioconductor.org/) but no tool to manage packages from the command line.
Additionally, local software environments have become increasingly popular for creating isolated or project-specific dependency installations without requiring elevated privileges.
Examples in other languages are [virtualenv](https://virtualenv.pypa.io/en/latest/), [pyenv](https://github.com/pyenv/pyenv), and [conda](https://conda.io/docs/) in Python and [rbenv](https://github.com/rbenv/rbenv) in Ruby.
Importantly, isolated software environments greatly facilitate reproducibility in research by enabling the straightforward management and dissemination of project dependencies.
With these ideas in mind, we propose [beRi](https://github.com/datasnakes/beRi), a toolset for managing R installations, R packages, and R environments.
Following the [UNIX philosophy](https://en.wikipedia.org/wiki/Unix_philosophy), we plan to divide these three goals into three modular packages, namely rinse, rut, and renv, respectively.
These packages will form the basis for beRi, which will leverage their functionality and provide a unified and user-friendly experience for configuring R environments.

## Comparing R Workflows

Currently, many workflow options exist that allow R users to manage environments to some degree.
However, our experience with them always left us wanting more, whether that was speed, flexibility, or convenient command-line usage.
The most popular options are Docker (rocker/containerit), packrat, and conda.
Docker offers containerization that results in exact replicates of your environment including the operating system.
The primary barriers for Docker are its complexity and its requirement for administrative privileges, which stunt its adoption in settings where time is limited for users and/or system administrators (*e.g.* academia).
While packrat is an excellent tool for for maintaining isolated, portable and reproducible environments for each project, it becomes intractably slow when projects grow in complexity, measured here as the number of package dependencies.
In response to the idea of beRi, the [r/rstats](https://www.reddit.com/r/rstats/) Reddit and #rstats Twitter communities rightfully asked how it differed from conda:

> "How does this differ from conda?"  
> "What are the benefits of using beRi over conda?"

First of all, CRAN or Bioconductor R packages need to be adapted for
Anaconda Cloud in order to be installable via conda.
At the time of writing, one [list](https://docs.anaconda.com/anaconda/packages/r-language-pkg-docs/) of R packages on Anaconda Cloud counted 328 packages, which represents a small fraction of [CRAN](https://cran.rstudio.com/web/packages/index.html)'s 13,285 packages.
We do not see the need to reimplement excellent repositories such as CRAN or Bioconductor.
We simply wish to expose a practical, intuitive, and convenient command-line interface for installing packages from CRAN, Bioconductor and other sources (*e.g.* MRAN, GitHub) and managing R environments.
Additionally, we have had several issues when installing R packages using the `install.packages()` function with an R installation managed through conda.
This often leads to compilation errors and library conflicts that are hard to resolve.
We found that maintaining a local installation of R eliminates these issues.
Thus, while conda is beginning to offer what we seek for R, it falls short due to the above issues.

## Target audience

R is a popular data science language, especially in the academic and healthcare sectors.[@Robinson2017-xv] Our target audience are those who can immediately benefit from better environment manangement in R, including computational researchers, data scientists, system administrators, and R package developers.
By design, beRi will also appeal to new R users who come from Python and/or have experience using the command line.
We realize that a command-line tool is not accessible to most R users.
That being said, we believe that R users who informed about software environments are much more likely to know how to use the command line.

## What beRi is not

Unlike conda and Anaconda Cloud, we do not aim to implement a dependency resolution system, nor do we wish to replicate existing repositories.
We plan on leveraging the existing facilities in R for installing packages from repositories such as CRAN and Bioconductor.

# The Team

[Team-beRi](https://github.com/orgs/datasnakes/teams/team-beri) is a group of individuals who are passionate about reproducibility in science but have experienced the limitations of existing tools.
Bridging that gap was the motivation for prototyping beRi at [hackseq18](https://www.hackseq.com/) and winning the popular votes exhibits that there is a strong interest and need for this tool in at least the bioinformatics community.

We are familiar with Python, R, and Bash from our daily work and plan to use these languages as the building blocks of the project.
Some of us have software engineering experience to help with the development process.
Since we will be both makers and users of this tool and are surrounded by fellow R users in science, we can continually improve beRi by adapting frequent feedback from ourselves and our community, who will be the end users of beRi.

Our team is divided into "tiers" of individuals based on experience level and willingness to contribute in the long term.
We are using the terms "Project Manager", "Lead Maintainer", and "Contributor" to manage our responsibilities.
The beRi team will develop under the [DataSnakes](https://github.com/datasnakes) organization on GitHub.
All of the current contributors participated in developing beRi at the
Hackseq 2018 bioinformatics hackathon in Vancouver, BC, Canada.

## Project Manager

beRi was conceived by [**Rob Gilmore**](https://www.linkedin.com/in/robert-gilmore-7b451592).
At hackseq18, his leadership skills and project management experience guided the team to success as the winners of the hackathon.
Gilmore's skill sets come from his education in [Biomedical Engineering](https://www.abe.msstate.edu/academics/undergraduate/biomedical-engineering/curriculum/) at Mississippi State University, professional development at [Engineering Research and Development Center](https://www.erdc.usace.army.mil/), and his extensive Bioinformatics experience as a Researcher in the fields of Genetics and Psychiatry using non-human primate models.
In his current position at the [University of Mississippi Medical Center](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html), he has utilized the [Mississippi Center for Computational Research's](https://mcsr.olemiss.edu/research/) HPC system to manage and develop large-scale bioinformatics pipelines using Linux, Windows, Python, R, and Bash.

## Lead Maintainers

-   **Shaurita D. Hutchins** is a Researcher II at the [University of Mississippi Medical Center](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html).
    She received her Master of Science in Biological Sciences from Mississippi College.
    She is proficient with Python and R package development, web development (including shiny apps and flask apps), genomics pipeline development, data analysis using Python, and data visualization using R.
    She is the co-creator of [Datasnakes](https://www.datasnakes.org/), an open-source and open-science organization and acts as the maintainer for multiple repositories/projects in it.

-   **Santina Lin** is a software engineer at Microsoft and works on maintaining the [RevoScaleR](https://docs.microsoft.com/en-us/machine-learning-server/r-reference/revoscaler/revoscaler) package as well as adding R and Python runtime to SQL server.
    She completed her master's degree in Bioinformatics at the University of British Columbia where she used R for data analysis and visualization and taught R and statistics in a graduate level course as a teacher assistant.

-   **Bruno M. Grande** is a Ph.D. candidate working on cancer genomics in [Ryan Morin's lab](https://morinlab.github.io/) at Simon Fraser University.
    An advocate for best practices in data science, he was a regular instructor and organizer in [Software Carpentry](https://software-carpentry.org/) and [SciProg](http://sciprog.ca/) workshops to help people overcome the reproducibility crisis in research.
    He dreams of publishing a Git repository that can reproduce every analysis from his thesis with a single command.
    beRi plays an important role in helping him achieve that goal.

# The Plan

beRi is a suite of Python packages which is composed of a virtual environment manager for R ([renv](https://github.com/datasnakes/renv)), an R installation and R version manager ([rinse](https://github.com/datasnakes/rinse)), and an R utility tool for installing packages, managing native R configuration files, and setting up local CRAN-like repositories ([rut](https://github.com/datasnakes/rut)).
These packages will be developed in separate repositories as standalone or fully independent CLI's, and [beRi](https://github.com/datasnakes/beRi) will also be developed in a separate repository.
However, the beRi CLI will depend on the other three packages, which will provide an enhanced and comprehensive experience as well as project management.

To begin, these packages will be built with Python for Linux systems starting with the latest Ubuntu release with long-term.
We will optimize our Python packages for the R ecosystem by using the [R manuals](https://cran.r-project.org/doc/manuals/), which can be found on the CRAN website.
Following stable builds, we will test under various
Linux distributions, followed by Windows 7 and above, and finally, macOS systems.
These Python packages are currently being managed by [poetry](https://github.com/sdispater/poetry) for ease of use and intuitive workflow; however, we have also considered switching to [pipenv](https://github.com/pypa/pipenv) in the future if poetry does not satisfy our needs. [click](https://palletsprojects.com/p/click/) will be used to build the command line interface.

## Standalone CLI - renv

renv, short for R virtual environment, is a Python-style virtual environment package for R.
It will use Python's venv [EnvBuilder](https://github.com/python/cpython/blob/3.6/Lib/venv/__init__.py#L17) module along with [cookiecutter](https://github.com/audreyr/cookiecutter) in order to deploy the proper shell scripts (activate) and R configuration files (.Rprofile, .Renviron).
A directory will be utilized in the users home directory (`~/.renv`) as a repository for any environment created.
The environments created with renv will not only be configurable with the .Rprofile/.Renviron files, but also a custom renv.yaml config file located in the root of the environment directory (`~/.renv/<env-name>/renv.yaml`).
This file will also allow the user to set up default environment variables, CRAN mirrors, and to set default packages to install.
The renv CLI will also contain a default yaml file for all newly created environments.

## Standalone CLI - rinse

rinse, short for R installer, is currently a simple installer for the latest version of R.
In the future, we would like to implement features similar to [pyvenv](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md).
This includes installing/uninstalling R from source, managing some of R's dependencies, and switching in between version of R.
Installing
Microsoft R-Open or [other R implementations](https://en.wikipedia.org/wiki/R_(programming_language)#Implementations) will also be considered.
After Linux development is stable, a long-term goal is to provide for the installation of Rtools on Windows.

## Standalone CLI - rut

rut, short for R utilities, will be an R package manager that will also be able to set up local CRAN-like repositories and manage custom R configuration files such as .Rprofile and .Renviron.
In its current iteration, rut can install packages from CRAN using R's remotes package.
As the least developed package and likely the most difficult to implement, rut will require more attention in order to develop it with best practices and effectively integrate it with beRi.
With regards to rut, we have discussed wrapping the R CMD INSTALL command, integrating a simpler version of [jetpack](https://github.com/ankane/jetpack), or utilizing [rpy2](https://rpy2.readthedocs.io/en/version_2.8.x/) for getting interpreter specific information.

## Integrated CLI - beRi


The primary and initial goal for the beRi suite of tools was to create standalone packages.
We looked to other programming languages for inspiration and R manuals and various documentations to keep the R ecosystem and R community in mind.
The integration of the three tools under the beRi CLI will help us develop a custom project workflow with one cohesive interface.
Once the standalone tools are functional, we will develop an analogous YAML configuration workflow for each package so that we can configure our environment with one YAML file and develop a beRi file-system.
The `~/.beRi` folder, for instance, will contain any renv environments, R installations, CRAN-like repositories, and `.libPaths()` associated with beRi processes by default.
While the development of beRi will affect some of the changes we make to the standalone CLI, we will never limit renv, rinse, or rut by linking them to each other or to beRi.

# Project Milestones

The planning phase of this project requires a continuous effort. [ZenHub](https://www.zenhub.com/blog/how-to-use-github-agile-project-management/), which integrates with GitHub, provides agile project management that will help contributors understand the project from a hierarchical view.
Using ZenHub Milestones, Estimates, Epics, and Pipelines will help us generate timelines based on criteria such as difficulty, stage of development, and the priority level of various tasks. [Todoist](https://todoist.com/tour) will be used for general task management. [Slack](https://slackdemo.com/?cvosrc=ppc.google.d_ppc_google_sitelink-slack-demo&cvo_creative=257483843273&utm_medium=ppc&utm_source=google&utm_campaign=d_ppc_google_sitelink-slack-demo&utm_term=slack&gclid=CjwKCAjwvNXeBRAjEiwAjqYhFklAL-7zPDBj92XOJ0hUoyRYdLL8cJIa_xaP8NtVsfb-cWKoU0IRbxoCNN8QAvD_BwE&gclsrc=aw.ds) will be another interface for higher level discussion and communication between collaborators and users of beRi.

## beRi for Linux (Version 1.0)

The beRi suite of tools will first be tested under the Linux OS.
The dates below represent estimated points of completion and maybe be modified after further analysis of our workload with ZenHub.

-   Development for Linux specifically Ubuntu LTS - June 2019
-   renv - February 2019
    -   Successfully Create Virtual Environments - Completed
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

## How Can The ISC Help

The total costs of this project will be $22,700, split into the following items:

-   Travel costs for a project mid-completion meeting in a location where the team leaders can convene ($3000).
-   Compensation to the project manager and lead maintainers as well as project dissemination including publication costs ($19,000).
-   Project management tools/subscriptions including Slack and Todoist and a `.com` or `.org` website via Namecheap ($700).

## Dissemination

This project will use the MIT open-source license.
The development process will take place in the [*datasnakes*](https://github.com/datasnakes) organization on GitHub, and beRi and its suite of tools will be submitted to PyPi when it is completed.
We will also encourage discussion and contributions through GitHub issues and the R mailing lists.

We will regularly post updates about this project on beRi's [website](https://www.getberi.site/), on the datasnakes' [Twitter](https://twitter.com/datasnakes) account, and on the [r/rstats](https://www.reddit.com/r/rstats/) Reddit community.
We have created a [gitter](https://gitter.im/CRANbeRi/Lobby) chat room to allow other developers to interact with us and collaborate with us.
Additionally, we intend to write articles for the R Consortium blog at the start, middle, and end of the project to document the project's progress and prepare for a publication in a scientific journal as we complete beRi.

# References