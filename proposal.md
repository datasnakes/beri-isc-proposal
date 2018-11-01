beRi Environments for R Installations
================
October 28th, 2018

**[Robert Gilmore](https://github.com/grabear)<sup>1</sup>, [Shaurita Hutchins](https://github.com/sdhutchins)<sup>1</sup>, [Bruno Grande](https://github.com/edzer)<sup>2</sup>, [Santina Lin](https://github.com/santina)<sup>3</sup>**

<sup>1</sup> Department of Psychiatry and Human Behavior, University of Mississippi Medical Center, Jackson, MS, United States
<sup>2</sup> Department of Molecular Biology and Biochemistry, Simon Fraser University, Burnaby, BC, Canada
<sup>3</sup> Microsoft Corporation, Redmond, WA, United States

Preface
=======

Reproducibility is crucial to scientific advancement according to the [NIH](https://www.nih.gov/research-training/rigor-reproducibility). Organizations such as RStudio and rOpenSci serve as proponents of reproducibility, which makes R-based data analysis increasingly more attractive. This is especially true in academia and in the healthcare industry, where R’s popularity has created a demand for easy to use tools that improve reproducible research. Currently, several technologies exist that seek to accomplish this goal. The [containerit](https://github.com/o2r-project/containerit) and [rocker](https://github.com/rocker-org/rocker) projects are examples that incorporate containerization via Docker. The Docker strategy is a way to perfectly reproduce a project on any system. However, many R users in academia and in the healthcare industry usually cannot access Docker, primarily because of system [vulnerabilities](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface) and because it requires administrative privileges. This is also true of the [*non-sudo work-around*](https://docs.docker.com/install/linux/linux-postinstall/) for Docker as it grants the user sudo level permissions in the background. These issues are compounded in situations where system administrators are unable or unwilling to make time for configuring Docker.

With the intent of addressing these issues, [conda](https://docs.anaconda.com/anaconda/user-guide/tasks/use-r-language/) has emerged as one of the most popular options for reproducibility. In other use cases, researchers also prefer various R packages including [packrat](https://github.com/rstudio/packrat), [checkpoint](https://github.com/RevolutionAnalytics/checkpoint) (MRAN), [miniCRAN](https://github.com/andrie/miniCRAN), [drat](https://github.com/eddelbuettel/drat), [workflowr](https://github.com/jdblischak/workflowr), and [drake](https://github.com/ropensci/drake). While these packages add tremendous value, they limit users to using an interactive R session (the R shell). The proposed solution is the beRi suite of tools, which is an integrated command-line interface (CLI) for managing R. beRi is a recursive acronym, which stands for “beRi Environments for R Installations”. BeRi will aim to support reproducible workflows by allowing virtualization and standardization for data analysis, while also supporting R’s native infrastructure by developing a command-line tool similar to what’s present in other popular programming languages. By creating user level environments to manage R workflows, we hope to enhance the way users are able to manage their R related projects.

Scope
=====

Comparing Programming Languages
-------------------------------

Most programming languages commonly used by the data science community have a command-line package manager, which offers access to a standard or custom package repository. Python, for example, has [pip](https://pypi.org/project/pip/) for [PyPi](https://pypi.org/) and [conda](https://conda.io/docs/) for [Anaconda Cloud](https://anaconda.org/). JavaScript has the [npm-client](https://github.com/npm/cli) for the [npm-registry](https://docs.npmjs.com/misc/registry). Ruby has the [gem-client](https://github.com/rubygems/rubygems) for [RubyGems.org](https://rubygems.org/). R installations come with the command “R CMD INSTALL” to install from [CRAN](https://cran.r-project.org/) and from manually downloaded tarballs.

In addition to package management, researchers often need to locally install software. HPC systems, for instance, often lack a required piece of software, have fewer choices for different versions of software, or have the software configured inappropriately for the user’s needs. This is especially true for programming languages that frequently receive updates to their source code. To alleviate this Python and Ruby have third-party interfaces ([pyenv](https://github.com/pyenv/pyenv), [rbenv](https://github.com/rbenv/rbenv), and [ruby-build](https://github.com/jdblischak/workflowr)) that manage the installation and management of various versions of the programming language. While [conda](https://conda.io/docs/) can manage installing R, it cannot manage multiple installed versions of the language.

Comparing R Workflows
---------------------

Currently, many workflow options exist that allow R users to manage their local environment to some degree. The most popular options are Docker (rocker/containerit), packrat, and conda. Docker offers containerization that results in exact replicates of your system environment. The primary barrier for Docker are its requirement for administrative privileges, which stunt its adoption in settings where time is limited for users and/or system administrators (*e.g.* academia).

While packrat is an excellent tool for for maintaining isolated, portable and reproducible local environments for R, larger projects tend to have more dependencies, which can increase the time in between packrat builds. Anaconda’s ecosystem for R is most similar to our approach and has been the main subject of debate in the [r/rstats](https://www.reddit.com/r/rstats/) subreddit and the [\#rstats](https://twitter.com/hashtag/rstats) Twitter forum.

Comparing beRi and conda
------------------------

[beRi](https://github.com/datasnakes/beRi) is a young project, which is being developed as a tool for managing R virtual environments, R installations, and R packages. Following the [UNIX philosophy](https://en.wikipedia.org/wiki/Unix_philosophy), we plan to divide these three goals into three modular packages, namely renv, rinse, and rut, respectively. These packages will form the basis for beRi, which will leverage their functionality and provide a unified and user-friendly experience for configuring local environments for R.

Conversely, conda is a well-developed and popular data science framework for package and local environment management that was initially developed for Python distributions. While it is similar to beRi, there are some key differences that set conda and beRi apart for managing the R language.

BeRi caters to the R community by providing a practical, intuitive, and convenient command-line interface for installing packages from CRAN, Bioconductor and other popular repositories such as MRAN, GitHub, and OmegaHat. [CRAN](https://cran.rstudio.com/web/packages/index.html) alone has a package count of 13,285 with around 6,600 packages being actively maintained or updated within the past year. Currently, Anaconda [lists](https://docs.anaconda.com/anaconda/packages/r-language-pkg-docs/) 328 available R packages on Anaconda Cloud, which is conda’s proprietary package management system. Bioconda is a third party repository for bioinformatics recipes for the conda framework which includes Bioconductor recipes which correspond to Bioconductor packages. BeRi can serve as an equally if not more useful alternative in cases where users want access to GitHub packages in addition to the entire CRAN repository, Bioconductor, or other parts of the R ecosystem.

Target Audience
---------------

R is a popular data science language, especially in the academic and healthcare industries.(1) Our target audience are those who would immediately benefit from a better local environment manager for the R language. They include computational researchers, R package developers, data scientists, and system administrators. By design, beRi will also appeal to new R users who come from Python and/or have experience using the command-line.

What beRi is not
----------------

Unlike conda and Anaconda Cloud, we do not aim to implement a dependency resolution system, nor do we wish to maintain a package repository. We plan on leveraging the existing facilities in R for installing packages from existing R repositories.

The Team - Datasnakes
=====================

[Team-beRi](https://github.com/orgs/datasnakes/teams/team-beri), also known as the [Datasnakes](https://www.datasnakes.org/), is a group of individuals who are integrated in the bioinformatics community as researchers and engineers. We also create open source tools for ourselves and others in the bioinformatics community. Our motivations for prototyping beRi are rooted in our own needs for tools that allow you to manage your local environment for using the R language. At the [hackseq18](https://www.hackseq.com/) genomics hackathon in Vancouver, BC, Canada, beRi was developed and put to the test by the Datasnakes, where it ultimately won by popular vote.

Project Manager
---------------

[**Rob Gilmore**](https://www.linkedin.com/in/robert-gilmore-7b451592) conceived and designed the beRi toolset. At hackseq18, he led the team to success as the winners of the hackathon. Gilmore was trained in [Biomedical Engineering](https://www.abe.msstate.edu/academics/undergraduate/biomedical-engineering/curriculum/) at Mississippi State University, developed professionally at [Engineering Research and Development Center](https://www.erdc.usace.army.mil/), and gained extensive Bioinformatics experience as a Researcher in the fields of Genetics and Psychiatry using non-human primate models. In his current position at the [University of Mississippi Medical Center](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html), he has used the HPC system at [Mississippi Center for Computational Research](https://mcsr.olemiss.edu/research/) to develop and manage large-scale bioinformatics pipelines using Linux, Windows, Python, R, and Bash.

Lead Maintainers
----------------

-   **Shaurita D. Hutchins** is a Researcher II at the [University of Mississippi Medical Center](https://www.umc.edu/som/Departments%20and%20Offices/SOM%20Departments/Psychiatry-and-Human-Behavior/Centers/Divisions,%20Centers,%20Divisions%20and%20Research/Neurobiology-and-Behavior-Research/Overview.html). She received her Master of Science in Biological Sciences from Mississippi College and is proficient with Python and R package development, web development, genomics pipeline development, data analysis using Python, and data visualization using R. She co-created the [Datasnakes](https://www.datasnakes.org/), an open-source organization and maintains several projects in it.

-   **Bruno M. Grande** is a Ph.D. candidate working on cancer genomics in [Ryan Morin’s lab](https://morinlab.github.io/) at Simon Fraser University. An advocate for best practices in data science, he was a regular instructor and organizer of [Software Carpentry](https://software-carpentry.org/) and [SciProg](http://sciprog.ca/) workshops to help people utilize reproducible workflows. He dreams of publishing a Git repository that can reproduce every analysis from his thesis with a single command. beRi plays an important role in helping him achieve that goal.

-   **Santina Lin** is a software engineer at Microsoft working on [RevoScaleR](https://docs.microsoft.com/en-us/machine-learning-server/r-reference/revoscaler/revoscaler) package and R and Python in SQL server. She completed her Master’s degree in Bioinformatics at the University of British Columbia, where she used R for data analysis and has taught people R and statistics in a graduate-level course as a teaching assistant.

The Plan
========

BeRi is a command-line interface composed of the following subcomponents: (1) [renv](https://github.com/datasnakes/renv), a virtual environment manager for R; (2) [rinse](https://github.com/datasnakes/rinse), an R installation and R version manager; and (3) [rut](https://github.com/datasnakes/rut), an R utility tool for installing packages, managing native R configuration files, and setting up local CRAN-like repositories. These packages will be developed in separate repositories as standalone command-line interfaces (CLIs). [*BeRi*](https://github.com/datasnakes/beRi) will also be developed in a separate repository but will integrate the other three packages into one tool.

These packages will be built in Python for Linux operating systems.. We have opted to use Python because of the ease of developing CLIs with the powerful tooling available in Python compared to R or Bash. Specifically, we are using the [click](https://palletsprojects.com/p/click/) Python package to build the CLIs. We will optimize our Python packages for the R ecosystem by using the [R manuals](https://cran.r-project.org/doc/manuals/). Once we have achieved stable builds on Ubuntu, we will broaden our compatibility to include additional popular Linux distributions, Windows, and macOS.

renv (Standalone CLI)
---------------------

renv, short for R virtual environment, is a Python-style virtual environment manager for R. It will use Python’s venv [EnvBuilder](https://github.com/python/cpython/blob/3.6/Lib/venv/__init__.py#L17) module along with [cookiecutter](https://github.com/audreyr/cookiecutter) in order to deploy the proper shell scripts and R configuration files (.Rprofile, .Renviron). A directory will be utilized in the user’s home directory (~/.renv) as a repository for any virtual environments created. These virtual environments will be configurable with the .Rprofile and .Renviron files as well as a custom YAML file.

rinse (Standalone CLI)
----------------------

rinse, short for R installer, is currently a simple installer for the latest version of R. In the future, we would like to implement features similar to [pyvenv](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md). This includes installing/uninstalling R from source, managing R dependencies, and switching between versions of R. Installing [Microsoft R Open](https://mran.microsoft.com/open) or [other R implementations](https://en.wikipedia.org/wiki/R_(programming_language)#Implementations) will also be considered.

rut (Standalone CLI)
--------------------

Currently, rut can install packages from CRAN using [r-lib](https://github.com/r-lib)’s [remotes](https://github.com/r-lib/remotes) package. As the least developed package and most difficult to implement, rut will require more attention in order to develop it with best practices and integrate it with beRi. We have considered wrapping the R CMD INSTALL command, integrating a simpler version of [jetpack](https://github.com/ankane/jetpack), or utilizing [rpy2](https://rpy2.readthedocs.io/en/version_2.8.x/) for getting interpreter specific information.

beRi (Integrated CLI)
---------------------

The main goals for the beRi CLI include seamlessly integrating renv, rinse, and rut by establishing a unified directory structure and creating a portable project workflow. By creating a user level configuration directory, , we hope to enhance our ability to configure R for beRi projects. The ~/.beri folder, for instance, will contain any renv environments, rinse R installations, and rut package installations associated with beRi processes by default. Our configuration and management process for beRi projects will involve the use of YAML files for each standalone package (*renv.yaml, rinse.yaml, and rut.yaml*). A top level YAML file will also be used with beRi (*beRi.yaml*) as a master configuration file for making our projects portable.

Project Milestones
==================

beRi for Linux (Version 1.0)
----------------------------

The beRi suite of tools will first be tested under the Linux OS. The dates below represent estimated points of completion and maybe be modified after further analysis of our workload with ZenHub.

-   Development for Linux specifically Ubuntu LTS - June 2019
-   **renv** - February 2019
    -   Successfully Create Virtual Environments - Completed
    -   Resolve minor R warnings - January 2019
    -   Code testing and linting; Create documentation and tutorials; Publish package to PyPi - February 2019
-   **rut** - March 2019
    -   Extend to global package management - February 2019
    -   Code testing and linting; Create documentation and tutorials; Publish package to PyPi - March 2019
    -   Secondary Goal - Manage local CRAN-like repositories
    -   Secondary Goal - Extend rut for managing projects with packrat
-   **rinse** - April 2019
    -   Manage the Installation of R from source - March 2019
    -   Integrate Microsoft R Open - April 2019
    -   Code testing and linting; Create documentation using sphinx and tutorials; Publish package to PyPi - May 2019
    -   Secondary Goal - Reverse engineer pyenv
-   **beRi** - May 2019
    -   Incorporate YAML configuration for renv, rinse, rut; Begin writing manuscript for beRi - April 2019
    -   Incorporate YAML configuration for beRi; Create a beRi user configuration directory- May 2019
    -   Code testing and linting; Create documentation using sphinx and tutorials; Publish package to PyPi - June 2019
    -   Complete writing manuscript for beRi - July 2019
-   Development for other Linux Distributions - July 2019

How Can The ISC Help
====================

The total costs of this project will be **$20,000** split into the following items:

| **Item**           | **Purpose**                                                          | **Cost** |
|:-------------------|:---------------------------------------------------------------------|:---------|
| Development        | To compensate the project’s developers                               | $16,00   |
| Travel             | To cover travel costs for a conference presentation and team meeting | $2,000   |
| Project Management | To purchase project management tools including Slack & Digital Ocean | $1,000   |
| Dissemination      | To pay for any publication costs                                     | $1,000   |

Dissemination
-------------

This project will use the MIT open-source license. The development process will take place in the *D[atasnakes](https://github.com/datasnakes)* organization on GitHub. We will also encourage discussion and contributions through GitHub issues and the R mailing lists. Additionally, we would like for the ISC to help us start a *working group* titled R Environments, which would aim to shed light on the usefulness of the .Rprofile/.Renviron for reproducibility in projects and to standardize the use of those files in popular R workflow packages especially packages supported by rOpenSci and RStudio.

We plan to post regular updates about this project on beRi’s [website](https://www.getberi.site/), on the Datasnakes’ [Twitter](https://twitter.com/datasnakes) account and on the [r/rstats](https://www.reddit.com/r/rstats/) Reddit community. Additionally, we intend to write articles for the R Consortium blog at the start, middle, and end of the project to document the project’s progress. We will submit beRi and its suite of tools to PyPi and prepare for a publication in a scientific journal as we complete beRi.

References
==========

1. Robinson D. The impressive growth of R - stack overflow blog. <https://stackoverflow.blog/2017/10/10/impressive-growth-r/>; 2017.
