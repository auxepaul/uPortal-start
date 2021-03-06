![uPortal logo](docs/images/uPortal-logo.jpg)

[![Linux Build Status](https://travis-ci.org/Jasig/uPortal-start.svg?branch=master)](https://travis-ci.org/Jasig/uPortal-start)

## About uPortal-start

uPortal-start is the mechanism through which individuals and institutions adopt [Apereo uPortal][],
the leading open source enterprise portal framework built by and for higher education institutions,
K-12 schools, and research communities.  **uPortal-start is new for uPortal 5.0**

uPortal-start help you manage:

  - Your uPortal configuration
  - Your uPortal skin
  - Your uPortal data
  - And your uPortal deployments through an integrated suite of CLI tools

### Prerequisites

The following software packages are required for working with uPortal-start:

  - A [Java Development Kit][] (JDK)
  - A suitable [Git Client][] for your OS

Download and install the **latest JDK 8 release**.  Make sure you select the full JDK;  _a JRE is
not sufficient!_

:warning: _Always use Git_ to obtain a copy of uPortal-start.  (Please ignore the _Download ZIP_
option provided by GitHub.)  The uPortal Developer Community makes improvements to uPortal-start
every week:  new features, bug fixes, performance enhancements, additions to documentation, _&c._
It is extremely important to be able to update your local copy of uPortal-start and (thereby)
benefit from these contributions.

### uPortal 5.0 Manual

This `README` provides some high-level information on the uPortal-start component, plus some how-to
examples of performing many of the most common tasks.  The complete uPortal 5.0 Manual is hosted in
GitHUb Pages.

  - [uPortal 5.0 Manual][]

As far as possible, **the examples in this `README` are presented in the order in which you will
want to perform them** when you set up a local uPortal dev environment.

## Using uPortal-start

uPortal-start provides a build system and several CLI tools through Gradle, and it even comes with a
_Gradle Wrapper_ so you don't have to install Gradle to use it.

Invoking the Gradle Wrapper on \*nix:

```console
    $ ./gradlew {taskname} [{taskname}...]
```

Invoking the Gradle Wrapper on Windows:

```console
    > gradlew.bat {taskname} [{taskname}...]
```

_NOTE:  For the sake of brevity, the remaining examples in this document are \*nix-only._

You can view a comprehensive list of Gradle tasks -- with short descriptions of what they do -- by
running the following command:

```console
    $ ./gradlew tasks
```

### List of Examples:

  - [How To Set Up Everything the First Time](#how-to-set-up-everything-the-first-time)
  - [How To Install Tomcat](#how-to-install-tomcat)
  - [How To Start the Embedded Database](#how-to-start-the-embedded-database)
  - [How To Deploy uPortal Technology to Tomcat](#how-to-deploy-uportal-technology-to-tomcat)
  - [How To Create and Initialize the Database Schema](#how-to-create-and-initialize-the-database-schema)
  - [How To Start Tomcat](#how-to-start-tomcat)

### How To Set Up Everything the First Time

The remaining examples (below) illustrate how to perform the most common uPortal tasks
individually;  but there's an easy way to do all of them at once when you're just starting out.

Use the following command to set up your portal the first time:

```console
    $ ./gradlew portalInit
```

This command performs the following steps:

  - Starts the integrated HSQLDB instance (`hsqlStart`)
  - Downloads, installs, and configures the integrated Tomcat servlet container (`tomcatInstall`)
  - Deploys all uPortal web applications to Tomcat (`tomcatDeploy`)
  - Creates the database schema, and imports both the Base & Implementation data sets (`dataInit`)

:warning:  After this command, your HSQLDB instance will be running.  That's normally a good thing,
but don't forget to stop it if you need to.

:notebook:  Your Tomcat server, on the other hand, _will not be running_ when this command
finishes.  Don't forget to [follow these instructions](#how-to-start-tomcat) to start it.

:notebook:  You can run this command again later if you want to "reset" your environment to a clean
state.  It's a good idea to **make sure both the Tomcat container and the HSQLDB instance are not
running** when you do.

### How To Install Tomcat

uPortal-start comes pre-integrated with the [Apache Tomcat Servlet Container][], which is a
requirement for running uPortal.  Several Tomcat configuration steps must be performed, moreover,
before the uPortal application software will function properly within it.  uPortal-start handles
these configuration tasks for you.

You can download (from [Maven Central][]), install, and properly configure an appropriate Tomcat
container by running the following command:

```console
    $ ./gradlew tomcatInstall
```

You can run this command again at any time to reset your Tomcat container to the defaults defined
by uPortal-start.

### How To Start the Embedded Database

uPortal-start also comes pre-integrated with a Relational Database Management System (RDBMS) called
[HSQLDB][].  A supported RDBMS instance is another uPortal requirement.  For uPortal server
deployments, you will want to choose a different RDBMS platform:  most likely Oracle, MS SQL
Server, MySQL, or PostgreSQL.  The integrated HSQLDB instance, however, is recommended for local
dev environments of uPortal.

Use the following command to start the embedded HSQLDB instance:

```console
    $ ./gradlew hsqlStart
```

:notebook:  the database must be running at all times when uPortal is running, and it also must be
running whenever several of the Import/Export tools are invoked.  (See examples below.)  It is
customary to leave HSQLDB running all day, or as long as you're actively working on uPortal.

You can stop the HSQLDB instance with the following command:

```console
    $ ./gradlew hsqlStop
```

### How To Deploy uPortal Technology to Tomcat

When(ever) you perform the `tomcatInstall` task, the Tomcat container will be empty.  You need to
build your uPortal application software and deploy it to Tomcat before you will be able to see it
working.

You can do that with the following command:

```console
    $ ./gradlew tomcatDeploy
```

:notebook:  you will need to _run this command again_ any time you make changes to anything inside
the `overlays` folder.

You can also run this command for one project at a time, for example...

```console
    $ ./gradlew :overlays:Announcements:tomcatDeploy
```

This is a great way to save time when you're working on a specific subproject.

### How To Create and Initialize the Database Schema

uPortal-start provides several Command Line Interface (CLI) tools that allow you to manage the
portal database.  The most important of these is the `dataInit` task.

Use the following command to create the database schema and fill it with _base portal data_ as well
as your _implementation data set_:

```console
    $ ./gradlew dataInit
```

:warning:  This command also _drops the existing database schema_ (beforehand) if necessary.
You probably want to perform this task against the production portal database exactly one time (in
the beginning).  In the case of non-production deployments, however, using `dataInit` for a full
"database reset" is fairly common.

### How To Start Tomcat

Once you have deployed uPortal technology, you will need to start the Tomcat server before you can
see your portal working.  You can do that with the following command:

```console
    $ ./gradlew tomcatStart
```

:warning:  It is safest to run Gradle tasks in uPortal-start _only when Tomcat is not running_.
This provision applies to tasks that build and deploy uPortal technology, as well as tasks that
manipulate the portal database.

You can stop the Tomcat server using with this command:

```console
    $ ./gradlew tomcatStop
```
### First Time Running uPortal via uPortal-start
Assuming all the defaults were left:
* The URL to access uPortal is:  <http://localhost:8080/uPortal/>
* Using the example credentials, you can bypass CAS when testing locally.  Available default logins / URLs:
  * admin: <http://localhost:8080/uPortal/Login?userName=admin&password=admin>
  * faculty: <http://localhost:8080/uPortal/Login?userName=faculty&password=faculty>
  * staff <http://localhost:8080/uPortal/Login?userName=staff&password=staff>
  * student <http://localhost:8080/uPortal/Login?userName=student&password=student>
  * guest <http://localhost:8080/uPortal/render.userLayoutRootNode.uP>
* The default tomcat install is:  _uPortal-start/.gradle/tomcat_
* The logs to watch for issues are located in:  _uPortal-start/.gradle/tomcat/logs_


[Apereo uPortal]: https://www.apereo.org/projects/uportal
[uPortal 5.0 Manual]: https://jasig.github.io/uPortal
[Java Development Kit]: http://www.oracle.com/technetwork/java/javase/downloads/index.html
[Git Client]: https://git-scm.com/downloads
[Apache Tomcat Servlet Container]: https://tomcat.apache.org/
[Maven Central]: https://search.maven.org/
[HSQLDB]: http://hsqldb.org/
