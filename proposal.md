beRi Environments for R Installations
================
October 28th, 2018

**[Robert Gilmore](https://github.com/grabear)<sup>1</sup>, [Bruno Grande](https://github.com/edzer)<sup>2</sup>, [Shaurita Hutchins](https://github.com/sdhutchins)<sup>1</sup>, [Santina Lin](https://github.com/santina)<sup>3</sup>**

<sup>1</sup> Department of Psychiatry and Human Behavior, University of Mississippi Medical Center, Jackson, MS, United States
<sup>2</sup> Department of Molecular Biology and Biochemistry, Simon Fraser University, Burnaby, BC, Canada
<sup>3</sup> Microsoft Corporation, Redmond, WA, United States

Preface
=======

Reproducibility is one of the cornerstones of scientific advancement according to the [NIH](https://www.nih.gov/research-training/rigor-reproducibility). Organizations such as RStudio and rOpenSci promote these values, which makes R based data analysis increasingly more attractive. This is especially true in academia and in the health care industry, where R's popularity has created a demand for easy to use tools that improve reproducible research. Currently, several technologies exist that seek to accomplish this goal. The [containerit](https://github.com/o2r-project/containerit) and [rocker](https://github.com/rocker-org/rocker) projects are examples that incorporate containerization via docker. The Docker strategy is a way to perfectly reproduce a project on any system. However, many R users in academia and in the health care industry usually don't have access to docker, primarily because of system [vulnerabilities](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface), and because it typically requires administrative privileges. These issues are compounded in situations where system administrators are either inexperienced, overworked, or both. To ameliorate these issues, [conda](https://docs.anaconda.com/anaconda/user-guide/tasks/use-r-language/) is the preferred framework and is by far one of the most popular options for reproducibility. In other use cases, researchers also prefer various R packages including [packrat](https://github.com/rstudio/packrat), [checkpoint](https://github.com/RevolutionAnalytics/checkpoint) (MRAN), [miniCRAN](https://github.com/andrie/miniCRAN), [drat](https://github.com/eddelbuettel/drat), [workflowr](https://github.com/jdblischak/workflowr), and [drake](https://github.com/ropensci/drake). While these packages add tremendous value to data analysis projects, they limit users to using an interactive R session (the R shell). The proposed solution is the beRi Suite of Tools for managing R. beRi is a recursive acronym, which stands for "beRi Environments for R Installations". The tools created for beRi will aim to support reproducibility by allowing virtualization and standardization for data analysis, while also supporting R's native infrastructure by developing command-line tools present in other popular programming languages.

Scope
=====

Comparing Programming Languages
-------------------------------

Many popular programming languages in the bioinformatics community have a command-line package manager that offers access to a standard or custom package repository. Python, for example, has [pip](https://pypi.org/project/pip/) for [PyPi](https://pypi.org/) and [conda](https://conda.io/docs/) for [Anaconda Cloud](https://anaconda.org/). JavaScript has the [npm-client](https://github.com/npm/cli) for the [npm-registry](https://docs.npmjs.com/misc/registry). Ruby has the [gem-client](https://github.com/rubygems/rubygems) for [RubyGems.org](https://rubygems.org/). While R has [CRAN](https://cran.r-project.org/) and [Bioconductor](https://www.bioconductor.org/), it doesn't have a way to install packages globally or locally via the command-line.

In addition to package management, it is often critical for researchers to install various software in their local environment. HPC systems, for instance, are more likely to not have a required piece of software, to have fewer choices for different versions of software, or to have the software configured inappropriately for the user's needs. This is especially true for programming languages that frequently receive updates to their source code. To alleviate this Python and Ruby have third-party interfaces ([pyenv](https://github.com/pyenv/pyenv), [rbenv](https://github.com/rbenv/rbenv), and [ruby-build](https://github.com/jdblischak/workflowr)) that manage the installation and management of various versions of the programming language. While [conda](https://conda.io/docs/) can manage installing R, it cannot manage multiple installed versions of the language.

Comparing R Workflows
---------------------

Currently, many workflow options exist that allow R users to manage environments to some degree. However, our experience with them always left us wanting more, whether that was speed, flexibility, or convenient command-line usage. The most popular options are Docker (rocker/containerit), packrat, and conda. Docker offers containerization that results in exact replicates of your environment including the operating system. The primary barriers for Docker are its complexity and its requirement for administrative privileges, which stunt its adoption in settings where time is limited for users and/or system administrators (*e.g.* academia).

While packrat is an excellent tool for for maintaining isolated, portable and reproducible environments, larger projects tend to have more dependencies, which can increase the time in between packrat builds. Anaconda's ecosystem for R is most similar to our approach and has been the main subject of debate in the [r/rstats](https://www.reddit.com/r/rstats/) Reddit and [\#rstats](https://twitter.com/hashtag/rstats) Twitter hashtag with people asking *how does this differ from conda* and *what are the benefits of using beRi over conda*.

Comparing beRi and Conda
------------------------

Without further ado, we propose [beRi](https://github.com/datasnakes/beRi), a toolset for managing R installations, R packages, and R environments. Following the [UNIX philosophy](https://en.wikipedia.org/wiki/Unix_philosophy), we plan to divide these three goals into three modular packages, namely rinse, rut, and renv, respectively. These packages will form the basis for beRi, which will leverage their functionality and provide a unified and user-friendly experience for configuring R environments.

First of all, CRAN or Bioconductor R packages need to be adapted for Anaconda Cloud in order to be installable via conda. At the time of writing, one [list](https://docs.anaconda.com/anaconda/packages/r-language-pkg-docs/) of R packages on Anaconda Cloud counted 328 packages, which represents a small fraction of [CRAN](https://cran.rstudio.com/web/packages/index.html)'s 13,285 packages. We do not see the need to reimplement excellent repositories such as CRAN or Bioconductor. We simply wish to expose a practical, intuitive, and convenient command-line interface for installing packages from CRAN, Bioconductor and other sources (*e.g.* MRAN, GitHub) and managing R environments. Additionally, we have had several issues when installing R packages using the `install.packages()` function with an R installation managed through conda. This often leads to compilation errors and library conflicts that are hard to resolve. We found that maintaining a local installation of R eliminates these issues. Thus, while conda is beginning to offer what we seek for R, it falls short due to the above issues.

Target audience
---------------

R is a popular data science language, especially in the academic and healthcare sectors.(1) Our target audience is those who can immediately benefit from a better environment management in R. They include computational researchers, data scientists, system administrators, and R package developers. By design, beRi will also appeal to new R users who come from Python and/or have experience using the command line. We realize that a command-line tool is not accessible to most R users. That being said, we believe that R users who informed about software environments are much more likely to know how to use the command line.

What beRi is not
----------------

Unlike conda and Anaconda Cloud, we do not aim to implement a dependency resolution system, nor do we wish to replicate existing repositories. We plan on leveraging the existing facilities in R for installing packages from repositories such as CRAN and Bioconductor.

The Team - Datasnakes
=====================

[Team-beRi](https://github.com/orgs/datasnakes/teams/team-beri) also known as the [Datasnakes](https://www.datasnakes.org/) is a group of individuals who are involved in various bioinformatics related research projects. We also like creating open source tools for ourselves and others in the bioinformatics community. Our motivations for prototyping beRi are rooted in our own needs for tools that allow you to manipulate system environments for the R language. At the [hackseq18](https://www.hackseq.com/) genomics hackathon in Vancouver, BC, Canada, beRi was developed and put to the test by the Datasnakes, where it ultimately won by popular vote.

Project Manager
---------------

[**Rob Gilmore**](https://www.linkedin.com/in/robert-gilmore-7b451592) conceived and designed the beRi toolset. At hackseq18, his leadership skills and project management experience guided the team to success as the winners of the hackathon. Gilmore was trained in [Biomedical Engineering](https://www.abe.msstate.edu/academics/undergraduate/biomedical-engineering/curriculum/) at Mississippi State University, professional development at [Engineering Research and Development Center](https://www.erdc.usace.army.mil/), and his extensive Bioinformatics experience as a Researcher in the fields of Genetics and Psychiatry using non-human primate models. In his current position at the [University of Mississippi Medical Center](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html), he has utilized the [Mississippi Center for Computational Research's](https://mcsr.olemiss.edu/research/) HPC system to develop and manage large-scale bioinformatics pipelines using Linux, Windows, Python, R, and Bash.

Lead Maintainers
----------------

-   **Shaurita D. Hutchins** is a Researcher II at the [University of Mississippi Medical Center](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html). She received her Master of Science in Biological Sciences from Mississippi College and is proficient with Python and R package development, web development, genomics pipeline development, data analysis using Python, and data visualization using R. She co-created [Datasnakes](https://www.datasnakes.org/), an open-source organization and maintains several projects in it.

-   **Santina Lin** is a software engineer at Microsoft working on [RevoScaleR](https://docs.microsoft.com/en-us/machine-learning-server/r-reference/revoscaler/revoscaler) package development as well as adding R and Python runtime to SQL server. She completed her Master's degree in Bioinformatics at the University of British Columbia, where she used R for data analysis and visualization in addition to instructing R and statistics in a graduate-level course as a teaching assistant.

-   **Bruno M. Grande** is a Ph.D. candidate working on cancer genomics in [Ryan Morin's lab](https://morinlab.github.io/) at Simon Fraser University. An advocate for best practices in data science, he was a regular instructor and organizer of [Software Carpentry](https://software-carpentry.org/) and [SciProg](http://sciprog.ca/) workshops to help people overcome the reproducibility crisis in research. He dreams of publishing a Git repository that can reproduce every analysis from his thesis with a single command. beRi plays an important role in helping him achieve that goal.

The Plan
========

beRi is a suite of Python packages composed of the following components: (1) [renv](https://github.com/datasnakes/renv), a virtual environment manager for R; [rinse](https://github.com/datasnakes/rinse), an R installation and R version manager; and (3) [rut](https://github.com/datasnakes/rut), an R utility tool for installing packages, managing native R configuration files, and setting up local CRAN-like repositories. These packages will be developed in separate repositories as standalone command-line interfaces (CLIs). [beRi](https://github.com/datasnakes/beRi) will also be developed in a separate repository but will depend on the other three packages.

These packages will be built in Python for Linux systems starting with the latest long-term support (LTS) Ubuntu release. We have opted to use Python because of the ease of developing CLIs with the powerful tooling available in Python compared to R or Bash. Specifically, we are using the [click](https://palletsprojects.com/p/click/) Python package to build the CLIs. We will optimize our Python packages for the R ecosystem by using the [R manuals](https://cran.r-project.org/doc/manuals/), which can be found on the CRAN website. Once we have achieved stable builds on Ubuntu, we will broaden our compatibility to include additional popular Linux distributions, Windows 7 and above, and macOS.

renv (Standalone CLI)
---------------------

renv, short for R virtual environment, is a Python-style virtual environment manager for R. It will use Python's venv [EnvBuilder](https://github.com/python/cpython/blob/3.6/Lib/venv/__init__.py#L17) module along with [cookiecutter](https://github.com/audreyr/cookiecutter) in order to deploy the proper shell scripts (activate) and R configuration files (`.Rprofile`, `.Renviron`). A directory will be utilized in the user's home directory (`~/.renv`) as a repository for any environment created. The environments created with renv will be configurable with the `.Rprofile` and `.Renviron` files as well as a YAML file located in the root of the environment directory (`~/.renv/<env-name>/renv.yaml`). This file will allow the user to set up default environment variables, CRAN mirrors, and default packages to install.

rinse (Standalone CLI)
----------------------

rinse, short for R installer, is currently a simple installer for the latest version of R. In the future, we would like to implement features similar to [pyvenv](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md). This includes installing/uninstalling R from source, managing R dependencies, and switching between versions of R. Installing [Microsoft R Open](https://mran.microsoft.com/open) or [other R implementations](https://en.wikipedia.org/wiki/R_(programming_language)#Implementations) will also be considered. We will also attempt to leverage pre-compiled versions of R (*e.g.* [Linuxbrew bottles](https://github.com/Linuxbrew/brew/wiki/Bottles)) to simplify the process of installing R without administrative privileges. After development for Linux is stable, a long-term goal is to provide support for Rtools on Windows.

rut (Standalone CLI)
--------------------

Currently, rut can install packages from CRAN using [r-lib](https://github.com/r-lib)'s [remotes](https://github.com/r-lib/remotes) package. As the least developed package and most difficult to implement, rut will require more attention in order to develop it with best practices and integrate it with beRi. We have considered wrapping the R CMD INSTALL command, integrating a simpler version of [jetpack](https://github.com/ankane/jetpack), or utilizing [rpy2](https://rpy2.readthedocs.io/en/version_2.8.x/) for getting interpreter specific information.

beRi (Integrated CLI)
---------------------

The primary and initial goal for the beRi suite of tools was to create standalone packages. We looked to other programming languages for inspiration and R manuals and various documentations to keep the R ecosystem and R community in mind. The integration of the three tools under the beRi CLI will help us develop a custom project workflow with one cohesive interface. Once the standalone tools are functional, we will develop an analogous YAML configuration workflow for each package so that we can configure our environment with one YAML file and develop a beRi file-system. The `~/.beRi` folder, for instance, will contain any renv environments, R installations, CRAN-like repositories, and `.libPaths()` associated with beRi processes by default. While the development of beRi will affect some of the changes we make to the standalone CLI, we will never limit renv, rinse, or rut by linking them to each other or to beRi.

Project Milestones
==================

beRi for Linux (Version 1.0)
----------------------------

The beRi suite of tools will first be tested under the Linux OS. The dates below represent estimated points of completion and maybe be modified after further analysis of our workload with ZenHub.

-   Development for Linux specifically Ubuntu LTS - June 2019
-   **renv** - February 2019
    -   Successfully Create Virtual Environments - Completed
    -   Resolve minor R warnings - January 2019
    -   Code testing and linting; Create documentation using sphinx and tutorials; Publish package to PyPi - February 2019
-   **rut** - March 2019
    -   Extend to global package management - February 2019
    -   Code testing and linting; Create documentation using sphinx and tutorials; Publish package to PyPi - March 2019
    -   Secondary Goal - Manage local CRAN-like repositories
    -   Secondary Goal - Extend rut for managing projects with packrat
-   **rinse** - April 2019
    -   Manage the Installation of R from source - March 2019
    -   Integrate Microsoft R Open - April 2019
    -   Code testing and linting; Create documentation using sphinx and tutorials; Publish package to PyPi - May 2019
    -   Secondary Goal - Reverse engineer pyenv
    -   Consider other distributions of R
-   **beRi** - May 2019
    -   Incorporate YAML configuration for renv, rut, rinse; Begin writing manuscript for beRi - April 2019
    -   Incorporate YAML configuration for beRi; Create a beRi file system - May 2019
    -   Code testing and linting; Create documentation using sphinx and tutorials; Publish package to PyPi - June 2019
    -   Complete writing manuscript for beRi - July 2019
    -   Secondary Goal - Add beRi projects
-   Development for other Linux Distributions - July 2019

How Can The ISC Help
====================

The total costs of this project will be **$20,000** split into the following items:

<table style="width:100%;">
<colgroup>
<col width="20%" />
<col width="69%" />
<col width="9%" />
</colgroup>
<thead>
<tr class="header">
<th>Item</th>
<th>Purpose</th>
<th>Cost</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Development</td>
<td>To compensate the project's developers</td>
<td>$16,000</td>
</tr>
<tr class="even">
<td>Travel</td>
<td>To cover travel costs for a conference presentation and team meeting</td>
<td>$2,000</td>
</tr>
<tr class="odd">
<td>Project Management</td>
<td>To purchase project management tools including Slack &amp; Digital Ocean</td>
<td>$1,000</td>
</tr>
<tr class="even">
<td>Dissemination</td>
<td>To pay for any publication costs</td>
<td>$1,000</td>
</tr>
</tbody>
</table>

Project Management
------------------

We are curently using [ZenHub](https://www.zenhub.com/blog/how-to-use-github-agile-project-management/), which integrates with GitHub and provides agile project management. Using ZenHub Milestones, Estimates, Epics, and Pipelines will help us generate timelines based on criteria such as difficulty, stage of development, and the priority level of tasks and issues. [Slack](https://slackdemo.com/), which also integrates with GitHub, will allow for higher level communication between collaborators and users of beRi.

Dissemination
-------------

This project will use the MIT open-source license. The development process will take place in the [*datasnakes*](https://github.com/datasnakes) organization on GitHub, and beRi and its suite of tools will be submitted to PyPi when it is completed. We will also encourage discussion and contributions through GitHub issues and the R mailing lists.

We will regularly post updates about this project on beRi's [website](https://www.getberi.site/), on the datasnakes' [Twitter](https://twitter.com/datasnakes) account, in our [gitter](https://gitter.im/CRANbeRi/Lobby) chat room, and on the [r/rstats](https://www.reddit.com/r/rstats/) Reddit community. Additionally, we intend to write articles for the R Consortium blog at the start, middle, and end of the project to document the project's progress and prepare for a publication in a scientific journal as we complete beRi.

References
==========

1. Robinson D. The impressive growth of R - stack overflow blog. <https://stackoverflow.blog/2017/10/10/impressive-growth-r/>; 2017.
