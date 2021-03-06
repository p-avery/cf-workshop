# Exercise 1.1 - Creating Tags and Categories

For our first exercise, we are going to setup some categories and tags, these will be used throughout the workshop and will help you understand not only how they help within CloudForms but also with other tools like Ansible Tower.

Tags are the foundation when it comes to automation, you will see by assigning tags to instances we can then consistently ensure the state of an instance.

Tags and Categories examples

Categories are the parent and the tag is the child, so we will create them as the following

            
- environment:
  - prod
  - qa
  - dev
- os:
  - linux
  - windows
- linux_app:
  - apache
  - nginx
  - mysql
- windows_app:
  - iis
  - mssql
    

## Table of Contents
 - [Step 1: Tags setup](#step-1-tags_setup)
 - [Step 2: Create Categories](#step-2-create_categories)
 - [Step 3: ios_facts](#step-3-ios_facts)
 - [Step 4: ios_command](#step-4-ios_command)
 - [Step 5: ios_banner](#step-5-ios_banner)
 - [Step 6: ios_banner removal](#step-6-ios_banner-removal)

### Step 1: Tags_setup

After logging into CloudForms up in the top right corner is the name of the user logged in click on that and select Configuration.
You will be in the settings section and it will display the settings for the appliance you are logged into, at the top of the accordian on the left under settings is **Settings Region: Region 0 [0]** click on this, You will now see the Tags Section, click into this.

### Step 2: Create_Categories

You will be under My Company Categories and the pre created categories will be present, to add new ones click under Name where is says <Click on this row to create a new category>



Let’s start with something really basic - pinging a linux host. Note that this is not an ICMP ping but rather a python script being executed on the host.

```bash
ansible control -m ping
```

To figure out all the options associated with an Ansible module each module has a documentation page.  Check out the [Ansible ping module here](http://docs.ansible.com/ansible/latest/ping_module.html)

### Step 2: Command
Now let’s see how we can run a good ol' fashioned Linux command and format the output using the command module.
```bash
ansible control -m command -a "uptime" -o
```

Ansible documentation page for the [command module](http://docs.ansible.com/ansible/latest/command_module.html)

### Step 3: ios_facts

Let’s switch gears and take a look at our routers. The ios_facts module displays ansible facts (and a lot of them) about an ios device.

```bash
ansible routers -m ios_facts -c local
```

Ansible documentation page for the [ios_facts module](http://docs.ansible.com/ansible/latest/ios_facts_module.html)

### Step 4: ios_command

Now, let’s get an interface summary using the ios_command module

```bash
ansible routers -m ios_command -a 'commands="show ip int br"' -c local
```
Ansible documentation for the [ios_command module](http://docs.ansible.com/ansible/latest/ios_command_module.html)
### Step 5: ios_banner
Let's check the banner on the routers before changing them
```bash
ansible routers -m ios_command -a 'commands="show banner motd"' -c local
```
You'll see that we currently have no motd banners

Let's go ahead and add the motd banner using the ios_banner module!

```bash
ansible routers -m ios_banner -a 'banner=motd text="Ansible is awesome!" state=present' -c local
```
Now, let's run our test again to see the change!
```bash
ansible routers -m ios_command -a 'commands="show banner motd"' -c local
```
Ansible documentation on the [ios_banner module](http://docs.ansible.com/ansible/latest/ios_banner_module.html)

### Step 6: ios_banner removal

Finally, let’s revert back and remove the banner.

```bash
ansible routers -m ios_banner -a 'banner=motd state=absent' -c local
```
Feel free to check again using the ios_command

# Complete
You have completed lab exercise 1.1

 ---
[Click Here to return to the Ansible Lightbulb - Networking Workshop](../README.md)
