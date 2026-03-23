---
date: 2026-03-22T03:59:22-04:00
description: "Automating all the things"
# image: ""
lastmod: 2026-03-23
showTableOfContents: false
tags: ["tech"]
title: "Starting To Learn Ansible"
type: "post"
---

In college I bought my first laptop to write code.
It wasn't the flashiest thing, but I figured I should learn about Linux.
I installed Debian and used it throughout my undergrad.
I'm still not the most knowledgeable Linux person but this period of time definitely helped me in the long run in knowing how computers work and seeing things happen step-by-step.
But my favorite part was learning to use all the small tools that come with Linux and customizing them.

I spent a good deal of time customizing my shell and text editor and tweaking each setting.
But here's the thing, I don't edit my system configs all that often.
What ends up actually happening is,  I hop on a new computer and then I spend too much time trying to remember how to do things: 

- How do I set relative lines in vim again?
- What's the syntax again to set colors on my shell prompt?

And this happens every time I hop on a new computer. It happened when I:

1. Bought my first laptop and installed Linux
2. Installed Linux on my desktop
3. Started work at a place where they gave me a macbook
4. Bought a raspberry pi

## The Solution

I should've done this long ago, but I finally decided to start up a repo that I can use to pull my configs into a new system so that I don't have to deal with these configuration tasks anymore.
I've been interested in [Ansible](https://www.redhat.com/en/ansible-collaborative?intcmp=7015Y000003t7aWQAQ) lately but I'll get to that more below.
I like to use [Pyenv](https://github.com/pyenv/pyenv) to manage python environments and so I decided to write a script that:

1. Install Pyenv and its dependencies
2. Create the virtualenv and install ansible in that virtualenv (I like to have a global environment separate from the system environment)
3. Execute an ansible playbook to handle the rest (install packages and symlink my configs)

It wasn't too hard to write a crude Bash script that "technically worked" but I later decided to follow Ansible guidelines and have the script be **idempotent**, so before we do each step, we have to check whether or not the step is necessary.

```bash {hl_lines=[1]}
if ! command -v pyenv >/dev/null; then
    export PYENV_ROOT="${HOME}/.pyenv"
    export PATH="${PYENV_ROOT}/bin:${PATH}"
    sudo apt-get update -y
    sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \
    libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
    libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
    curl https://pyenv.run | bash
    eval "$(pyenv init -)"
else
    echo "Pyenv is already installed and in the path"
fi

...
...

ansible-playbook ansible/setup.yml -i 'localhost,' -K -c local
```


Above is the logic for step 1 and I've highlighted the conditional check to show that we're not just installing these packages every single time, only if Pyenv isn't installed yet.
The [script](https://github.com/jaydom28/system-setup/blob/main/bootstrap.sh) uses this pattern for the other steps and ended up not being that long.

## Learning Ansible

Ansible was on my radar for a while and I figured this would be an opportunity to learn a little.
With Ansible I can just define what I want my system to look like (this is known as **infrastructure as code**), and it will handle all the weird edge cases and errors that I haven't thought of yet.
Ansible uses [YAML](https://en.wikipedia.org/wiki/YAML) syntax and here is an example of an ansible task to install a system package, and its Bash equivalent.

```bash
# Install tmux in a Bash script
sudo apt install tmux
```

```yaml
# Ansible task to install tmux
- name: "Ensure package is installed"
  ansible.builtin.apt:
    name: "tmux"
    state: present
  become: true
```

You might be thinking that it seems easier to just write a Bash script to install all my packages, and you're probably right.
Ansible is probably overkill for just setting up my personal system, installing packages, and creating symlinks to my config files (which are also going in this repo btw).
**But** there are some advantages:

1. The Ansible task uses a module that handles edge cases I haven't thought of yet.
2. Ansible does not have to be run on the local system, I can use Ansible to configure a remote server without needing to install anything on the remote server.
3. The Ansible task will also check for whether or not the package is installed before attempting to do so.

Anyway, I have pushed all the code [here](https://github.com/jaydom28/system-setup) and there are probably some more things that need to be done but I have the basic steps to download all the packages I use and symlink the correct configs. I still want to refactor my neovim and tmux configs but that will be for another time.
