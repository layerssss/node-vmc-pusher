AppFog Github Pusher: A Github Service Hook for AppFog
=========================================================


Introduction
------------

4/22/12

This is a github service hook application that works with AppFog.

It can be deployed as an application on appfog.com, and then
that application can be listed as a service hook for a repository on
github.com.  Then upon pushing to github, the service hook will be
called and the application automatically deployed to appfog.com.


Instructions
------------

The basic instructions involve setting up the application on
appfog.com, setting authentication variables to the
application, then adding the servicehook to your github application.


#### Step 1. Push the application into your appfog environment. 

     af push appfog-github-pusher

You will need to pick your own a unique name for the service hook.

#### Step 2. Set the environment variables on the application so that it can login to your appfog account.  

     af env-add appfog-github-pusher CF_TARGET=https://api.appfog.com
     af env-add appfog-github-pusher CF_USER=<username>
     af env-add appfog-github-pusher CF_PWD=<password>

#### Authorization: the code checks that the github pusher email address is the same as the appfog user specified.  Optionally you can whitelist other addresses to use the pusher.

     af env-add appfog-github-pusher CF_WHITELIST=<emailaddress>,<emailaddress>

#### Step 3. Set the github service hook for your repository to this url.

     Repository -> Admin -> Service Hooks -> PostReceive URLs
     http://appfog-github-pusher.aws.af.cm/pusher


#### Step 4.  You are ready to go.  

     git push <repository>
     af apps

This will take the repository name as the application name and push that to appfo.com when pushed.

FAQ
-----

#### It doesn't work with my organization account!

For repositories in an organization account, no email is binding to a push. So you need to add the email `undefined` in whitelist.

     af env-add appfog-github-pusher CF_WHITELIST=undefined

Notes
-----

As of today, this service hook requires all assets (i.e., packages) to
be part of the checked in repository. It is possible to modify this
servicehook to try to stage packages into the repository as well
during the execution of the hook, but because the package managers are
not installed on appfog.com, this will require running the
application on a self hosted server.

The fixtures needs a binary copy of git and a .fixtures/tmp/ directory
for the application to properly function.  In particular, git = Ubunto
10.04 LTS version compatible.  And there is no compatible system tmp
directory in cloud foundry, so the tmp directory needs to be stored
locally.  Do not move or rename these files.


