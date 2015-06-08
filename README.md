Ansible-role-pattern
====================

Due to the fact there are no global ansible conventions, it is really difficult
to reuse the code of roles/playbooks and people use different strategies to 
create roles. 

It is not about good practises, because those things depends on the organization,
on the context, on the people ...  it is about documenting my way of working.
I have defined some conventions and patterns in my roles with the aim of having 
something to clone and start working on a new role, helping to get always the 
same structure and making easy to understand. This is a kind of document that 
you have to read by opening the different files to understand the reasons and 
patterns used.

Moreover of this example/pattern-collection role, there are other documents 
that help me to define it, some of then are:

 * 6 practices for super smooth Ansible experience: http://hakunin.com/six-ansible-practices
 * Multistage environments with Ansible: http://rosstuck.com/multistage-environments-with-ansible/


**To sum up: make everything reusable and interchangeable.**

Structure and Ansible Conventions
---------------------------------

Please, go over all the files of this role and read the comments. Good starting
point is `defaults/main.yml`.


 * Each role should be independent and reusable -without external dependencies- 
   except, of course, if you need something like java to run a service, in that
   case it should be a dependency, but never things like a database, because the
   database server can be running on a different server, or you could use things 
   like SQLite. The dependencies should be defined at playbook level, because 
   Ansible is kind of stateless orchestrating tool, so it is difficult to 
   control when a role was called from others with different
   parameters. To sum up, keep the dependencies at the top level.

 * A role/playbook should be idempotent, that means, for example, if you 
   re-provision a server and there were no changes, it has no to restart the 
   associated service. If no changes on the input (variables), the result is 
   the same as previous time, without changing the status of the services.

 * Each task needs a name and it should be a block of code, do not put lines 
   from diferent tasks together please, it makes it difficult to read.
   Divide them in groups and use tags with the same name as the tasks file:
   * `install`: in charge of installing the distribution packages and after 
   running the service should be stopped (unless no configuration is needed).
   * `configure`: in charge of creating the configuration settings and
   make sure that the service is enabled and started.
   * `plugins`, `vhost` ... : in charge of additional setups.

 * Use `vars` for specific distribution parameters of the service, think about 
   something like constants. Usually those parameters they do not affect the 
   functionality of the service you try to install, it is just about package 
   names, location, folder names, ... nothing to do with the real settings in 
   the configuration files of the service. There usually are Debian and RedHat 
   files with specific parameters which are going to be loaded in those cases. 
   Do not work against distribution philosophy, try to keep the files in the same
   place where your distribution usually has put those files.

 * If you want to maintain your code, keep the name consistency between your 
   playbooks, inventories, roles and group variables. 

 * Use `defaults` as service parameters in order to get parameterized roles. 
   There you should define your service defaults settings (to use them in 
   the template), and the recommended defaults (from the owner/programmer) 
   should be defined in the template with `parameter | default()`. By doing in
   that way, we could reuse those templates in other provisioning tools 
   different than Ansible.

 * Make it at least available for RedHat/Centos and Debian/Ubuntu distributions.
   To help you on testing, just create a `Vagrantfile` ready to run the role with
   some defaults or tipical usage, like a demo. Document at least the tipical 
   usage of the role on a Readme file.

License
-------

GPLv3

Author Information
------------------

José Riguera López <jriguera@gmail.com>

