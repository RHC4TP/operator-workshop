# Basic Overview of Ansible

## Goals

By the end of this section, you will understand:
- What is an Ansible Playbook
- What is an Ansible Role
- How to run Ansible
- How to use variables in an Ansible Role


## Prepping Your Environment

First, ensure that you have Ansible installed on your system:

```bash
ansible --version
```
If you do not get any output, use the [Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

Next, create a directory for this exercise and move into it:

```bash
mkdir ~/Ansible-Exercises
cd ~/Ansible-Exercises
```

## First Commands

Create an ansible inventory file. Ansible works against multiple systems in your infrastructure at the same time. It does this by selecting portions of systems listed in Ansible’s inventory. We will use `localhost` as a starting point:

```bash
cat <<EOF > inventory
[local]
localhost ansible_connection=local
EOF
```

Now test that Ansible is properly configured by running an ad-hoc command:

```bash
ansible -i inventory all -m ping

# Output
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Now that we have confirmed Ansible is properly configured, let's look at Ansible Playbooks.

## Working with Ansible Playbooks

Playbooks are Ansible’s configuration, deployment, and orchestration language. Inside of playbooks, a developer can declare a set of tasks or roles to be executed upon launching the playbook.

For simplicity, let's start with declaring the tasks directly in the playbook. Start by creating and editing `playbook.yml` with a debug statement to output information from our host:

```yaml
- hosts: localhost
  tasks:
    - debug:
        msg: "System {{ inventory_hostname }}"
```

Run the playbook:
```bash
$ ansible-playbook -i inventory playbook.yaml
---- (omitted output) ----
TASK [debug] ****************************
ok: [localhost] => {
    "msg": "System localhost"
}
```

At a basic level, Playbooks can be used to manage configurations and deployments to remote machines. Putting all of our orchestration logic inside of a single playbook file can be cumbersome, which is why it is recommended to separate our logic into multiple Ansible Roles. Let's move our tasks inside of a role.

## Creating an Ansible Role

Ansible Roles are a way of grouping related tasks and other data together in a
known file structure. Grouping content by roles allows it to easily be shared
with others. Roles are easier to reuse than playbooks.

Now that we have seen how tasks are executed inside of playbooks, let's move the same logic inside of an Ansible Role. Begin by creating a new role using `ansible-galaxy`:
```
$ ansible-galaxy init example-role
- example-role was created successfully
```

This generates a skeleton Role which can be easily modified by editing the tasks inside of `example-role/tasks/main.yml`. For a simple example, let's modify the tasks file to print out information about our host as we did in the previous example:
```yaml
- debug:
    msg: "System {{ inventory_hostname }}"
```

## Consuming Ansible Roles from within Ansible Playbook

We can now consume this role inside of `playbook.yml` instead of declaring the tasks inside of our playbook. Modify `playbook.yml` to match the following:
```yaml
- hosts: localhost
  roles:
    - example-role
```

Since `example-role` exists as a directory in the same path as `playbook.yaml`, we can execute the role from our playbook. If we were calling this playbook from a different directory we would have to ensure that the role exists in the default Ansible Roles path or provide the fully qualified path inside of our playbook.

Run the playbook:
```bash
$ ansible-playbook -i inventory playbook.yaml
---- (omitted output) ----
TASK [example-role : debug] *************
ok: [localhost] => {
    "msg": "System localhost has uuid NA"
}
```

You should observe that our `example-role` is referenced in our debug task.

## Using variables inside of Ansible Roles

Ansible Roles are great because they allow a developer to declare all variables
along with defaults ahead of time to ensure idempotence regardless of how the
role is executed. As a simple example, let's declare a variable `message` which
we will print out during execution of our Role. Let's declare a default message
by editing `example-role/defaults/main.yml`:
```yaml
---
message: "Greetings Earthlings."
```

Now modify `example-role/tasks/main.yml` to print out our message using a `debug` task:
```yaml
- debug:
    msg: "System {{ inventory_hostname }}"
- debug:
    msg: "{{ message }}"
```

Run the playbook:
```bash
$ ansible-playbook -i inventory playbook.yaml
---- (omitted output) ----
TASK [example-role : debug] *************
ok: [localhost] => {
    "msg": "System localhost"
}

TASK [example-role : debug] *************
ok: [localhost] => {
    "msg": "Greetings Earthlings."
}
```

We can overwrite the default message by passing along `message` as an extra var when launching the playbook:
```bash
$ ansible-playbook -i inventory playbook.yaml -e 'message="This is an altered message"'
---- (omitted output) ----
TASK [example-role : debug] *************
ok: [localhost] => {
    "msg": "System localhost has uuid NA"
}

TASK [example-role : debug] *************
ok: [localhost] => {
    "msg": "This is an altered message"
}
```
