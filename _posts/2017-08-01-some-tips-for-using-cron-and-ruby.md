---
layout: post
title: "Some tips for using Cron and Ruby"
description: ""
category: 
tags: [cron,ruby,devops]
---
{% include JB/setup %}

I thought I'd do a quick post on some Ruby/RVM/cron gotchas.

First up, if you're using cron, your north star should be this snippet:

    $ grep CRON /var/log/syslog

This will tell you what cron is up to, and what it has run. You'll often see errors crop up too, but it's essentially your best friend for checking whether or not cron has actually tried to run a task.

Okay, so you're having issues with cron and Ruby. I can almost guarantee that the issue will come down to either:

1. Permissions
2. Gemsets

Permissions are simple - if you're running cron as `root` then change user and try and run the ruby script with the same permissions and setup as cron will. Even though cron doesn't have a shell available to it, nine times out of ten this will reveal that there's a permissions problem that is causing the job to fail.

Gemsets are a bit trickier. Your problem is probably as follows - you've installed ruby via RVM or similar and the gems you have available to `require` when you run `irb` are not the same that cron will see. If you're the only thing using ruby on the box, the issue should be resolvable pretty easily:

    $ rvm gemset create global
    $ rvm gemset use global
    $ rvm @global do gem install gem-you-want-to-install

Then ensure that cron is using the same ruby that the user that installed rvm is using. **Remember that cron needs to have an absolute path to the ruby binary and the script as well** and you should be fine. If you need separate gemsets then you will have to tweak further.

Either way, your script should have:

    #!/path/to/bin/ruby

at the top, and for that pseudo code example, your cron line should look like (for example, where your application or service is called `service-name`):

    0,15,30,45 * * * * /path/to/bin/ruby /opt/service-name/ruby-script.rb

To redirect the output to a log file, you can use a construction like:

    0,15,30,45 * * * * /path/to/bin/ruby /opt/service-name/ruby-script.rb &>> /var/lib/service-name/logs/cron-pipelines.log

Most times, running through this simple checklist will work for me, hopefully it helps you too.