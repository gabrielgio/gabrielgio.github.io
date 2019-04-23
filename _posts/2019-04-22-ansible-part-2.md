---
layout: post
title:  "Automating desktop setup with ansible-pull part-2"
date:   2019-03-07
tags: ['ansible', 'ansible-pull', 'linux', 'fedora']
---

[See part 1]({% post_url 2019-03-07-ansible-part-1 %})

Now we're gonna setup ansible to work with a git repository. The process is quite similar with `ansible-playbook` the only difference is that command will get a repository instead of a folder. Following the previews example we'll get vim setup automated.

Do create a git repository wherever you see fit ([gitlab](https://about.gitlab.com/) and [github](https://github.com/) offer free repositories). For this task we're gonna need to add only two file: one for the `yml` file describing the task and the `.vimrc` file.

In the `.vimrc` add your own configuration, you can see mine [over here](https://github.com/gabrielgio/homestation/blob/241b27285d8cba8548277f3508e097439831a6eb/config/.vimrc), it is pretty simple as I don't use it but for simple text editing (like this post) so you can start with it if you don't have one.

The `yml` file will have two tasks, one is to install vim itself, it identical as it in the part 1.

{% highlight yml %}
# main.yml
---
- name: install vim
  dnf:
    name: vim
    state: latest
{% endhighlight %}

Then we add the task to copy `.vimrc` file to your `$HOME`, for it we shall use [copy module](https://docs.ansible.com/ansible/latest/modules/copy_module.html):

{% highlight yml %}
---
- name: copy vimrc file
  copy:
    src: config/.vimrc
    dest: ~/
    mode: 0644
{% endhighlight %}

After adding those two files your repository will be something [like this](https://github.com/gabrielgio/homestation/tree/debcf3458df511aef9f7dca0cb73f6cf6baddd5d).

And now we just need to run `ansible-pull` command

{% highlight bash %}
ansible-pull -U <YOUR_REPO> -i all main.yml #you may need run it as a sudo
{% endhighlight %} 

The `-i` option it is a list of hosts. Remember `man` is your best friend take a look at `man ansible-pull` to know more about the params.

The best part if you want to test quickly you can just run my sample and see the result:
{% highlight bash %}
ansible-pull -U https://github.com/gabrielgio/homestation.git -C debcf3458df511aef9f7dca0cb73f6cf6baddd5d -i all main.yml
{% endhighlight %}

The idea here is to keep your repository as a source of truth when comes to configuration, you can add this task to your cron tab,thus you just push something to your repository and after a few minutes no only your machine but all machine that have it setup will receive an update, you can use it as a simple way to install software, update machines or even distribute tools company-wise.
