---
layout: post
title:  "Automating desktop setup with ansible-pull part 1"
date:   2019-03-07
tags: ['ansible', 'ansible-pull', 'linux', 'fedora']
---

Every time that I do a clean install on my machine it takes a few hours
till I get to point where I was before formatting it, install all
packages, select themes, icons, fonts, install IDEs, extensions and so
on. After doing it a few times I came to the conclusion (
[genius](https://i.imgur.com/BtWuQgT.png)) that It would be nice to
automate this chore. And as a result, I could tinker a little more with
my system and not be afraid of spending a weekend reinstalling
everything (which have happened more time that I'd like)

So after a few attempts using python and bash, I couldn't get something 
that scales and ended with many files and keep the files organized and
concise turned to be more tedious than the setup itself. So it comes
[Ansible](https://www.ansible.com/). It is an enterprise-grade software
used to automate tasks. It has many features I can be really helpful as
a sysadmin but what we gonna focus here is cliente side of thing using
[Ansible Pull](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#ansible-pull)
and
[Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html)
as better describe:

> Ansible-Pull is used to up a remote copy of ansible on each managed 
> node, each set to run via cron and update playbook source via a source
> repository. This inverts the default push architecture of ansible into
> a pull architecture, which has near-limitless scaling potential.

> Playbooks are Ansible’s configuration, deployment, and orchestration 
> language. They can describe a policy you want your remote systems to
> enforce, or a set of steps in a general IT process.

So what we're gonna do is pull a playbook from a git account a run on
the host, that playbook will have the tasks that we need to setup our
machine.

To run it locally first we need localhost to all hosts list, to do so we
only the following text to `/etc/ansible/hosts`:

{% highlight text %} 
[all] 
localhost 
{% endhighlight %}

As an experiment we're gonna make tasks to install vim. Currently, I
using
[Fedora](https://getfedora.org/) thus we going to use
[dnf modeule](https://docs.ansible.com/ansible/latest/modules/dnf_module.html)
to install packages

The playbook to install is quite simple:

{% highlight yml %}
# main.yml
- hosts: all tasks:
     - name: install vim
       dnf:
         name: vim
         state: latest
{% endhighlight %}

Fist `hosts:` it is required and it has to match our hosts so we are
able to run that playbook. Then `tasks:` which is a list of task that
the playbook will perform that in this case will be `dnf install` for
the package vim.

Ansible pull requires a repository but for the first example I want to
keep it simple so we will use `ansible-playbook` commando to run
`main.yml` direct from disk, do to so just run the following command:

{% highlight bash %}
sudo ansible-playbook --connection=local main.yml
{% endhighlight %}

After a few seconds, vim will be installed on your machine.
{% highlight bash %}
PLAY [all] *************************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [install vim] *****************************************************
ok: [localhost]

PLAY RECAP *************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0  
{% endhighlight %}
 
This is the first step, next part we shall create a more complex
playbook and setup repo and actually use `ansible-pull`



 


