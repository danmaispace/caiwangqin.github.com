---
layout: post
title: "Setup Hudson CI and Auto Deploy to DailyBuild Server"
description: ""
category: DevOps
tags: [Hudson, CI, Deploy]
---
{% include JB/setup %}

# Get hudson from <http://hudson-ci.org/>

# Start hudson ci service

{% highlight bash %}
java -jar /home/jesse/sources/hudson-2.0.0.war --httpPort=8090 &
{% endhighlight %}

# Install "Hudson GIT plugin" in Manage Hudson -> Manage Plugins

# New CI Job
    Source Code Management
    Git and input URL of Repository

    Build Triggers
    Poll SCM Schedule 
    # Poll every 5 minutes
    */5 * * * *

    Add Build step -> Execute shell

{% highlight bash %}
#!/bin/bash -e

# Use the correct ruby
# rvm use 1.9.3
# Do any setup
# e.g. possibly do 'rake db:migrate db:test:prepare' here
cd $WORKSPACE
bundle install
# Finally, run your tests

# set env vars
export CI_REPORTS=results
export RAILS_ENV=test
export PATH=$PATH:$HOME/bin:/var/lib/gems/1.8/bin

# Prepare for rcov
[ -d "coverage" ] && rm -rf coverage
mkdir coverage

# invoke rake
rake spec spec:rcov

# deploy after build
cap deploy
{% endhighlight %}


    Check Publish Rails stats report

    Check Publish Rcov report

    Rcov report directory: coverage

    Check E-mail Notification

# Done, the CI will build when code push to repository