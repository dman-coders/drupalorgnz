# Whoa to go.

## Ideas:

* Build new D8 site, using industry best-practices.
* Migrate site as-is, before attempting any improvements.
* Prioritise getting used to clean and automated deployment processes
  before starting custom build work.

## Requirements

* Open, documented processes for project build, change and release.
* Re-use existing tools and processes where they are available -
  Invest time into understanding how enterprise professionals do it
  and avoid inventing work-arounds due to lack of familiarity with other ways.


## Start project

Choose to use Acquia BLT as a project foundation.

[An overview of how to work within this framework may be helpful](http://blt.readthedocs.io/en/8.x/readme/onboarding/)
, refer to that later for reminders and hints as you come up against
 different aspects of it.

* Verify development environment requirements (composer/LAMP/vagrant/etc)
* Follow instructions for a new D8 Drupal Project per http://blt.readthedocs.io/en/8.x/readme/creating-new-project/

    export PROJECT=drupal_org_nz_d8
    export PROJECT_DIR=/var/www/$PROJECT

    export COMPOSER_PROCESS_TIMEOUT=2000
    composer create-project acquia/blt-project $PROJECT_DIR --no-interaction
    cd $PROJECT_DIR
    composer blt-alias

This performs a hundred setup tasks.
You end up with a (scaffolding) project folder to work within.
The project folder contains a number of files that have been copied from templates,
 but are now yours to own.
 The SCM process has already been started for you, and a git repo has been initialized.

[A summary of the project files this provides can be found in the BLT docs](http://blt.readthedocs.io/en/8.x/readme/repo-architecture/)

## Begin your local project.

If using an IDE like PHPStorm - set up a new project to manage this folder.
(New project from existing files...)
(Source files are in a local directory, no web server is yet configured)

> HINT: *EXCLUDE* the 'vendor' folder from indexing for better performance.
  If your IDE indexes that lot, you'll be sitting around for a while.

### Some gotchas that may help understanding early

* From the very beginning, you should name your project after the final, real, live hostname.
  If your project is `drupal.org.nz` - that becomes your project identifier and machine_name.
  Trying to avoid that leads to a clash of assumptions later inside BLT.
  Roll with it. Don't even try to paraphrase, flatten, or acronym it.

* When using the Vagrant/Drupal-vm local development build, the blt defaults
  will set it up such that your local development url appears to be a subdomain
  of the live URL. 
  * Final project hostname: `drupal.org.nz`
  * Locally hosted development VM Url: `local.drupal.org.nz`
    
  This may feel unintuative, depending on you expected workflow.
  It's done through your local dev environments /etc/hosts and the Vagrant hosts-updater plugin.
  
  


## Keep following the BLT instructions

http://blt.readthedocs.io/en/8.x/readme/creating-new-project/

> Customize your project.yml

Have a look at the options in there. Knowing they exist will help you manage the project
later.
Important settings at the beginning were:

### CHANGE project settings

    project:machine_name: drupal.org.nz
    # 'prefix' is used/required for git commit messages.
    # Should be the same as the project machine_name?
    project:prefix: DRUPALNZ
    # This is used for the virtual hostname. Your local access to your dev site:
    # IMPORTANT - modify it to reflect your real, final host!
    project:local:hostname: 'local.drupal.org.nz'

Git commit those changes, any time you do something significant.
 Commit early, commit often.

> **Important** The project structure we will be using will include some
  "Acquia/BLT-specific *git commit hooks*"(http://blt.readthedocs.io/en/8.x/scripts/git-hooks/README/)
  that will start to *validate* the format of your commit messages.
  Expectations are that every commit is a response to an issue task change,
  with an issue number.
  OOTB, Commit messages must be of the format:
  "``DRUPALNZ-0000: Updated install instructions.``"
  The discipline it introduces is good, even though it may seem like a pain at first.
  We may want to revise those rules ourselves later.
  For the first phase, unless you have a ticket number to set everything up, use 000 for starters.
  OR, - if you are on a development branch, this rule is not enforced 
  ... so always do the messy stuff in a development branch!
  The git commit hooks also perform code cleanup analysis!

## Setting up Drupal VM - the Dev OS

I'm using using Drupal VM as recommended, but The Acquia Dev Desktop works well also.
 If that doesn't suit you, see the docs for alternatives.
 This choice may mean may encounter troubleshooting issues not covered here.

 I'm using the VM in order to isolate this work from my own dev environment,
 that already includes a dozen styles of platform, PHP version and environment setups.
 This is to avoid assumptions about other folks development style.

    blt vm

This is somewhat just 'Vagrant up'
It will take a while if it's the first time you've used vagrant or drupal-vm.
Some large VM downloads may be performed in the background.

**Be prepared for half an hour here.**

... Go have a look at "some nearby documentation"(http://blt.readthedocs.io/en/8.x/readme/best-practices/)
while you wait.

--------------------------

Among many other tasks, it:

* Downloads and initializes a whole working VM image.
* Maps the important working directories to the copy of Drupal that's already been
  downloaded into your project.
* Initializes the VM OS with the required downloaded libraries - provisioned by Ansible.
  Piles of randomness comes along too - Java, Ruby, whatever..
* Drupal-vm provides a hundred more useful tools also.
  See more at https://www.drupalvm.com/
* Drupal-dev-specific stuff also includes Drush, adminer, xhprof, and more.
  Look into these, they are provided to help you.

Paths to these tools include:

    dashboard.local.drupal.org.nz (Start here)
    adminer.local.drupal.org.nz (MySQL UI)
    local.drupal.org.nz
    xhprof.local.drupal.org.nz
    pimpmylog.local.drupal.org.nz

(These URLs only work if you followed step #1
 "Verify development environment requirements"
 inc vagrant-hostsupdater )

**Gotcha**
Pay attention to the hostnames here - it's much less confusing for everyone if you use
 the real, live, final hostname as the suffix.
 If you get ``local.projectname.com``, it's not wrong, but it will be inconsistent
 and frustrating later. If this is wrong, now is the best time to edit your
 ``project.yml``, maybe edit your ``box/config.yml`` if it's been created,
 ``vagrant destroy`` and run the setup again.
 It may also be enough to make the changes and re-run ``vagrant reload --provision``
 If that works, it should save you half an hour of rebuild.

### Using Drupal VM

This is just a vagrant+virtualbox setup.

Useful commands to run when you are inside the project directory:

To log in

    vagrant ssh

To stop it when unwanted

    vagrant halt

To start it when not running

    vagrant stop

## Still setting up Drupal VM - The Drupal site instance

If the previous step took a while, you may have forgotten what we were doing.

**Welcome back**

We are now ready to actually

### Make a new Drupal site.

    blt local:setup

If all went well, You can visit the dashboard,
    [http://dashboard.local.drupalorgnz.com/]

or just go directly to
    [http://local.drupalorgnz.com]

To finally see the new site.

Hoorah. That's the first "Hello World"/"It Works" step!

Now we need to actually start turning this into a workable project for developing with.

#### Login details

The Drupal Site Build process happened somewhere inside the ``local:setup`` tasks.
If you read back through those, somewhere in the middle you will see the output
of the drush site-install command:

    blt > setup:drupal:install:
    ...
    Installation complete.  User name: admin  User password: admin``
    
You should also be able to run `drush uli` on the VM.

#### drush remote access

Now is a good time to test that drush, and specifically the drush connection 
from your host to the VM site instance works.
The setup should have updated an alias in `drush/site-aliases/aliases.drushrc.php`
for you, and you should be able to run commands like

    drush @drupal.org.nz.local status

which will communicate from your desktop to the VM.

Future blt commands will assume that your desktop environment can invoke drush in this way.

[Troubleshooting drush connection from host to drupal-vm via blt](#troubleshooting-drush-connection-from-host-to-drupal-vm-via-blt)

> Suggestion - `blt doctor` should really have tested this for us.

Review the other settings in the aliases.drushrc.php file also.


## Next steps

[Follow along with the BLT documentation](http://blt.readthedocs.io/en/8.x/readme/next-steps/)


Initializing the upstream repo would be good soon,
 but first let's capture the local changes we have just initiated...

### Set up Git

Reading the BLT docs on "Repository Architecture"(http://blt.readthedocs.io/en/8.x/readme/repo-architecture/)
 seems a little confusing, possibly outdated?
As a rule of thumb, seeing as the project comes with some very good ``.gitignore``
 already, I presume that things not being ignored are to be owned by the project.

#### Git commit the local config additions

The setup process has added or updated some settings files with changes that are
 important to your local dev environment.
 *Normally I would think these sorts of settings
  should NOT be committed to the project source control*,
  but the project scaffolding does *not* `.gitignore` them,
  and the project structure seems to know what it's doing better than I do,
  so I guess they do go in (?).

I will inspect, add and commit:

* ``salt.txt``
  This is used to generate the unique ID of the site. It's needed to be deployed
  with the project so that other development clones know they are the same site.
* ``drush/site-aliases/aliases.drushrc.php``
  This will allow local drush commands to interact with the new site.
  Note the project-specific ``drush`` and ``drush/site-aliases`` directories.
  This is a useful convention and place to put project-specific drush-isms.
* ``docroot/sites/default/settings.php``
  This was modified to include some additional ``blt.settings.php``
  ... Not sure what's happening there, need to investigate.
* ``docroot/sites/default/settings/default.local.settings.php``
  Presumably we copy the ``default.*`` template file into our own ``local.settings.php``
  file for localizations. Not sure why blt needed to put that into an unconventional
  subfolder though.

    git add \
      salt.txt \
      drush/site-aliases/aliases.drushrc.php \
      docroot/sites/default/settings.php \
      docroot/sites/default/settings/default.local.settings.php
    git commit -m "DONZ-0000: Added project-local settings files."

#### Git commit the Vagrant additions.

Running ``blt vm`` earlier also gave us a ``Vagrantfile`` and a ``box`` folder
 - which is now part of our project to be tracked with git.
Have a look at those files to see what they provide.
``box/config.yml`` Has a bunch of project-specific parameters that were used to build
 the VM.

As using Vagrant is not a required part of setting up an environment
 (you could have chosen ADD, and it's not relevant to the live deploys)
 I guess it's optional.
 However, I want to capture everything and keep git clean, so I'm adding this.
 It's not being .gitignored by the scaffolding, so I presume that's correct.

    git add Vagrantfile box composer.json composer.lock
    git commit -m "DONZ-0000: Added Vagrant settups."

#### Set up Git repository

Creating a remote repository needs to be done manually.
We are placing this one on github (where the previous work was done).
I'm not going to branch the existing D7 site, as there is pretty much nothing in common,
 and the code structure is violently different now.

So the new home for this project is

    git@github.com:dman-coders/drupalorgnz.git
    https://github.com/dman-coders/drupalorgnz.git

I'm honestly not 100% sure about branch names and git-flow stuff yet.
The BLT docs say (http://blt.readthedocs.io/en/8.x/readme/dev-workflow/)

* We should use a branch literally called ``develop`` for development, and reserve
  ``master`` branch as a place to eventually merge stable, versioned releases.

Therefore, for now, ``master`` is empty, and work-to-date is ``develop``.
Feature branches may come later, and will hang off ``develop``.

    git checkout -b develop
    git remote add origin git@github.com:dman-coders/drupalorgnz.git
    git push origin develop

As my upstream repository was empty, this has made ``develop`` the default one for now,
and there is no ``master`` at all yet.

I think that's a place to go on from.

## Build a UAT before starting development!

Against all conventional habits, we need to embrace the habits of *deploying* safely
 **before** actually making changes.
This will (hopefully) train the mindset of thinking about deployment
 (and eventually testing) of changes first - before just hacking code.

 Making a change in Drupal is easy - just press buttons.
 Deploying all those button presses to an automatically-provisioned site on a live
 running database without regression conflicts - is harder.
 But that's what we have to be able to do.

To this end, we will set up the UAT environment, and set up the processes for mirroring
 and releasing what we have got, **first**.

We want:
* A public(ish) snapshot fo the work-in-progress
* On a different machine
* that we can push local changes to
* with as little friction as possible.

This will involve:
* Provisioning and setting up conections to a UAT server
* Where we can easily push the 'soft' UAT changes (the ``develop`` branch)
* which can build all the dependencies
* and upgrade/import the current content for previewing the changes

Later, the UAT environment may also be a place where it can do the same for tagged
 pre-releases of the ``master`` tags.

> Aside - at this point, I realized that the the hostname that the wizard had given me
  ``drupalorgnz.com`` was not going to work later.
  I had to go back to the initial project.yml file, fix that, and rebuild everything.
  I've done that, and updated the previous docs above already.
  Lesson: It's REALLY important to change
  ``project:local:hostname: 'local.${project.machine_name}.com'``
  to a more accurate version
  ``project:local:hostname: 'local.drupal.org.nz'``

## Using the BLT `deploy` process

I need to get used to the 'artifact' generation method for modern releases.
Reading the [BLT docs on suggested Deployment Workflow](http://blt.readthedocs.io/en/8.x/readme/deploy/)
would be good about now.

Using the `blt deploy` command 
(once the git repository settings are set up - specifically the connection to the upstream repo and branch)
Does a number of things:

The cli help is no help. Really must read the full docs.

> Builds separate artifact and pushes to git.remotes defined project.yml.

Points of interest:

* Where the docs refer to `git:remotes` ... The **plural** there is important.
  We may have one 'remote' where the development commits happen, and another 
  'build' remote where deployment artifacts get committed.
  **These can be different branches within the same repo** but the actual design
  is for these to be different repos - one dedicated to maintaining the un-processed
  sparse working files and build scripts - and one holding static copies of
  all the downloaded and build and cooked files that make up a live site - 
  the "build artifacts"


From verbose inspection:

* It marks the local status of the repo (optionally) with a tag.
* It performs an ENTIRE new composer build in a nearby directory.
    * It creates/updates a new BRANCH for our project in git. Mine got called `develop-build`
    * It adds everything is just built, while NOT .gitignoring the usual things. This branch gets all files committed.
    * It pushes that branch to the upstream repo. Everything there seems to have been created with a single commit.
* Additional hooks for deployment were available at several phases. Presumably a post-build-artifact step would be to forward this artifact for testing etc.

However - what it does not do is 'deploy' anything anywhere.
It builds the artifacts - which are a snapshot of the desired site files,
commits them, then stops.
... OK, it seems that due to annoying built-in magic, `blt deploy` is actually an
alias for `blt deploy:build`. 

... Looks like BLT does NOT actually do a push deploy to remote targets.
Presumably that would be the job of [BLT recommendations for upstream orchestration and CI](http://blt.readthedocs.io/en/8.x/readme/ci/).

BLT can be made to push the 'artifact' entire site into a 'build' branch of the repo.
By default this will be named `{current-branch}-build`, which is how we got `develop-build`.

If you are in a feature branch, but want to deploy as if your feature had already been merged to 
develop, then the command `blt deploy --branch=develop-build` would get that happening.

Upstream we place a jenjins job that waits for activity in that branch, and triggers builds when a new artifact is committed.

## Using BLT to down-sync

Well, the more common case is to get a fresh copy of the master DB.
Let's try that instead.

> **Insert disclaimer about already having a prepared upstream master site .. this is not actually a from-scratch walkthrough.**
> .. however, the 'master' was not even using BLT properly - it was just a strawman D8 experiment.
Hopefull we will be able to figure out how to do a clean 'push' deploy to master later.

### Define the _master_ site instance drush connection

In daily use (outside of the very-first-time setups) there will exist a cannonic
master copy of the site. Once in production, that will be `@live`.
In pre-production, that could be in a number of states or locations.

The point is however that OUR local dev environment should be told how to reach it
and how to get a copy of it.

Add the master site alias to your project `drush/site-aliases/aliases.drushrc.php`.

This information will be shared by all project members, and is the most useful thing
everyone needs to know.

* Edit the file `drush/site-aliases/aliases.drushrc.php` with valid connection details
  that will let you connect to the master instance.
* Label or alias it in a way that matches
  the blt settings in `blt/project.yml` eg `drush:aliases:remote:'live'`.
* As with all drush remote-site instances, you will also have to ensure that
  your account, and the account you will be using on the target remote-host
  have the right combination of ssh identity keys and entries in
   the remote targets `~/.ssh/authorised_keys` already installed.
  * This must be done by an security administrator outside of this BLT process.
  * In short, you must already be able to `ssh $remote-user@$remote-host` using key-only authentication.
  * TMI: The connection settings that Vagrant uses *should* already be correctly using
    ssh key-forwarding. 
    Which is a little bit of extra magic that allows you,
    when logged in to the VM,
    to make outgoing connections to the master remote-host,
    using your local desktop credentials.
    If this is failing to happen transparently, further troubleshooing may be needed.
  
* Test that you can access the master instance, using normal drush commands.
  `drush @live status`

The master remote site instance may be named 'pre-live' or `projectname.test' or
whatever, depending on if it's a running site, a new site, or a migration-in-progress etc.

### Confirm drush connection to the _local_ site instance

If the previous project and vagrant bootstrapping went well, you should be able to run:

    drush -v @drupal.org.nz.local status

(where 'projectname.local' is whatever `blt/project.yml drush:aliases:local` resolves to
and has a valid entry in our locally customised `drush/site-aliases/aliases.drushrc.php`)

If so, good.

#### Troubleshooting drush connection from host to drupal-vm via BLT

I encountered a roadblock where I cannot even run 

`drush @drupal.org.nz.local status`

I was not able to use drush to connect to the VM.
I was getting a password challenge - which cannot be right.

On inspection (running drush with --verbose),
I found that the attempt failed because the (autogenerated)
`drush/site-aliases/aliases.drushrc.php` *almost* had the settings right.

Comparing: 

`vagrant ssh-config`

    Host drupal.org.nz
      HostName 127.0.0.1
      User vagrant
      Port 2222
      UserKnownHostsFile /dev/null
      StrictHostKeyChecking no
      PasswordAuthentication no
      IdentityFile /Users/dan/.vagrant.d/insecure_private_key
      IdentitiesOnly yes
      LogLevel FATAL
      ForwardAgent yes


`drush site-alias @drupal.org.nz.local` 

    $aliases["drupal.org.nz.local"] = array (
      'root' => '/var/www/drupal.org.nz/docroot',
      'uri' => 'http://local.drupal.org.nz',
      'remote-host' => 'local.drupal.org.nz',
      'remote-user' => 'vagrant',
      'ssh-options' => '-o PasswordAuthentication=no -i /Users/dan/.vagrant.d/insecure_private_key',
    );

Revealed that **vagrant uses a special ssh port** - while the drush site-alias
seems unaware of it.

> This port number 2222 does not seem to be explicitly chosen in oany of our configs
> such as `box/config.yml`, or even `vendor/geerlingguy/drupal-vm/default.config.yml`.
> It may have been chosen by Vagrant itself during setup to avoid ssh port conflicts on my
> host machine.

It seems that from now on, `drush/site-aliases/aliases.drushrc.php` will become 
 our own responsibility, so the thing to do is to add that port to our ssh-options.
 
        'ssh-options' => '-p 2222 -o PasswordAuthentication=no -i ' . drush_server_home() . '/.vagrant.d/insecure_private_key'

Now I can successfully run 

    drush -v @drupal.org.nz.local status
    
And now future blt commands that try to leverage drush will work again.

> It does not seem that we can ask drush ssh-command to be replaced completely 
> with `vagrant ssh` - though that seems like a suggestion to look into.


#### `blt sync`

Once you have confirmed that you can successfully, manually run 
`drush @live status`
and 
`drush @local status`
( where those aliases are the ones named in `blt/project.yml` `drush:aliases:`)
then it's time to try:

`blt sync`

To re-cap ... I'm running this command 
* from my desktop environment, 
* trying to update the site living on my VM, 
* Using data pulled down from the nominated remote master site.

Once other troubleshooting issues were squashed, 
It went through the usual drush sql-sync steps and made my local 
copy reflect the remote one.

I have a working local copy now!