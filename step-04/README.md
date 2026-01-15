# Ansible: Grouping hosts

Hosts in inventory can be grouped arbitrarily. For instance, you could have a
`debian` group, a `web-servers` group, a `production` group, etc...

```ini
[debian]
host0
host1
```

This can even be expressed shorter:

```ini
[debian]
host[0:1]
```

If you wish to use child groups, just define a `[groupname:children]` and add
child groups in it. For instance, let's say we have various flavors of linux
running, we could organize our inventory like this:

```ini
[ubuntu]
host0

[debian]
host1

[linux:children]
ubuntu
debian
```

Grouping, of course, leverages configuration mutualization.

![alt](../images/hosts04.png)

![alt](../images/ping04.png)