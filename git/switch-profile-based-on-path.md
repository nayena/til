# Switch profile based on path

Most of us have a personal git profile and a work profile.

Up until a few weeks ago, I used to not commit code using my personal profile
and in stead manage two different sets of git configs.

The [git documentation has quite the section about how to seperate
configuration files out into their own files](https://git-scm.com/docs/git-config#_includes), and including them based on globs.

In my personal setup, I have a single `.gitconfig` file, and it contains nothing
more than this:

```
[includeIf "gitdir:~/"]
  path = .gitconfig-priv
[includeIf "gitdir:~/src/work/"]
  path = .gitconfig-work
```

This simply includes my personal profile, with all aliases and other goodies.
If the path is `~/src/work`, it also loads my work profile. At work, we have a 
conditional, that doesn't let us commit code, unless the author email matches
a certain regex.

In my `.gitconfig-work`, I simply include this piece:

```
[user]
  email = <work email>
  name = <work name>
  signingkey = <gpgkey>
```

This will override any configuration made in my personal profile, thus using
my work gpg key and my work "credentials".
