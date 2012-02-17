Module Manager
==============

Developing extensions for software that doesn't allow you to separate your files from core files, and keeping that extension under version control and making it easy to deploy is now much, much easier. Development of this script was inspired by Magento which forces you to mix your extension files all throughout the core code directories. With Module Manager, you can specify in a text file where you want your directories and files to be mapped to, and it will create softlinks for you so that all of your versioned code is kept in a directory that is easy to maintain and deploy.

Requirements
------------

* bash
* Filesystem with symlink support (not cygwin)
* Available in your path: grep (POSIX), find, ln, cp, basename, dirname
* Web server must follow symlinks
* git and/or subversion are optional (not required for "deploy" command) 

Installation
------------

* Download or checkout
* Move to or create link in your path 

Getting Started
---------------

See the [Tutorial](http://code.google.com/p/module-manager/wiki/Tutorial) or simply run modman to get the usage summary.

Changes in this Fork
--------------------

This fork adds the ability to map from an external Git repository instead of the default behavior of having the Git repository cloned locally within the .modman directory.  The use-case is to facilitate development within Eclipse, with an Eclipse project under Git mapped by modman into another Eclipse project (Eclipse does not deal well with version controlled code within a project unless the entire project is under version control).  An example setup:

<pre>
workspace
|-- MyExtension
|    |-- app
|    |    |-- etc
|    |    |
|    |   ...
|    +-- modman
|
+-- Magento
     |-- .modman
     |    |-- myextension
     |         |-- .sourcedir
     |-- app
     |    |-- code
     |    |    |-- community
     |    |    |    |-- MyPackage
     |    |    |    |    |-- MyExtension
     |    |    |    |    |
     |    |    |    |   ...
     |    |    |   ...
     |    |   ...   
     |   ...
    ...
</pre>

Where 'workspace' is your Eclipse workspace, 'MyExtension' is a Git repository with a modman configuration file, 'Magento' is your core Magento, and '.sourcedir' references the workspace/MyExtension repository, which is mapped to Magento/app/code/community/MyPackage/MyExtension/* by the modman configuration file therein.

This is achieved by adding a new flag to the modman script <code>--external-source</code> which would be used like the following to create the above example:

<pre>
$ cd /path/to/workspace/Magento
$ modman clone myextension --external-source /path/to/workspace/MyExtension
</pre>

Because an external local repository is used, the 'deploy' rather than 'update' command must be used to map new changes to the modman configuration:

<pre>
$ cd /path/to/workspace/Magento
$ modman deploy myextension
</pre>

When not in <code>--external-source</code> mode, this version of modman should function the same as the original, so you can refer to the [Tutorial](http://code.google.com/p/module-manager/wiki/Tutorial) for a more complete usage.  The one exception perhaps is nested modules, a feature I do not use and did not attempt to code for/test.  Also, no attempt was made to maintain the functionality of the svn commands, so things may be broken there as well.

Fork Changelog
--------------

#### 2/17/2012 ####
* REGEX_ACTION pattern fixed to remove '\s+' which caused module names containing the letter 's' to break the modman script
* --external-source option for local external Git repositories added
* Forked from modman 1.4.8

Version Control Systems
-----------------------

Modman supports subversion and git. Other VCSs could be used by manually checking out the source code into the proper directory and using the "deploy" command.

Fork Project Author
-------------------

Justin Stern

Parent Project Author
---------------------

Colin Mollenhour

http://colin.mollenhour.com

[Follow on github!](https://github.com/colinmollenhour) 