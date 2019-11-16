---
layout: post
title:  "Compiling emacs from source code on fedora"
date:   2019-11-16
tags: ['emacs', 'emacs27', 'linux', 'fedora']
---

Lately I have been using emacs quite heavily, I started using org mode after a friend insistently 
telling me to try, got hooked and now I'm addicted on spacemacs+evil mode, very useful, I recommend it!
I'm compiling emacs because emacs 27, which it is not available on fedora repos yet, has some serious start up 
performance improvement which I more then welcome when using spacemacs.

But enough talking lets down to the business.

First install the following packages:

{% highlight bash %}
sudo dnf install git autoconf make gcc texinfo gnutls-devel giflib-devel ncurses-devel libjpeg-turbo-devel giflib-devel gtk3-devel libXpm-devel
{% endhighlight %}

Then we need to clone the repo from [savannah.gnu.org](http://savannah.gnu.org/projects/emacs/) where is hosted 
the source code of emacs:
{% highlight bash %}
 git clone -b master git://git.sv.gnu.org/emacs.git
{% endhighlight %}

Navigate to emacs folder that we've just cloned and execute the following steps

{% highlight bash %}
./autogen.sh
./configure
make -j$(nproc)
sudo make install
{% endhighlight %}

After that you will have emacs 27 or further running on your machine. To verify the version just run `emacs --version`.

### Bonus content
For maximum  awesomeness I would suggest using [spacemacs](https://www.spacemacs.org/), 
tt has a lot of features out of the box. To install:

{% highlight bash %}
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
{% endhighlight %}


