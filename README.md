# Ansible Role: Jenkins Swarm Agent

Installs Jenkins Agent on servers via the swarm jar. On windows it runs as a service using NSSM.

## Requirements

* Requires Java 8+ be installed on the server
* Jenkins master with the [Swarm client](https://wiki.jenkins-ci.org/display/JENKINS/Swarm+Plugin)
* Ansible 2.2 due to use of some new Windows modules

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    jenkins_agent_master: 'localhost'
    jenkins_agent_master_port: 80
    jenkins_agent_master_protocal: 'http'

The system hostname; usually `localhost` works fine. This will be used during setup to communicate with the running Jenkins instance via HTTP requests.


    jenkins_admin_username: admin
    jenkins_admin_password: admin

Credentials for Jenkins master user.

    name: 'Jenkins_Agent'

Name of the slave.

    version: "3.6"

Then Jenkins agent swarm client version can be pinned to any version available on https://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/

    client_jar: "swarm-client-{{version}}.jar"

    client_source: "https://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/{{version}}/{{client_jar}}" 

The url for the client source jar file.

    swarm_mode: 'exclusive'

The mode controlling how Jenkins allocates jobs to slaves. Can be either 'normal' (utilize this slave as much as possible) or 'exclusive'(leave this machine for tied jobs only). Default is exclusive.

    swarm_executors: '4'

Number of executors.

    swarm_labels: 'windows'

The labels to associate this agent with. Defaults to 'windows'

    disableSslVerification

Swarm option to disable ssl verification in the HttpClient.

    service_user: "LocalSystem"
    service_pass: undefined

This system account and password he service will run on. This is set it to run as the `LocalSystem` account by default with no password.

    java: 'C:\ProgramData\Oracle\Java\javapath\java.exe'

The path to the java program on the system.

    agent_drive: 'C:'

    agent_home: "{{agent_drive}}\\j"

The Jenkins agent home directory which, amongst others, is being used for storing artifacts, workspaces and plugins. This variable allows you to override the default C:\j location.

## Dependencies

  - Java installed on the system. Used `geerlingguy.java` role.


Example Playbook
----------------

```yaml
- hosts: jenkins_agents
  vars:
    jenkins_agent_master: "{{ hostvars.example_master.ansible_host }}",
    jenkins_agent_num_executors: 8,
    jenkins_agent_labels: "Windows dotnet swarm msbuild"

  roles:
    - mmagonde.jenkins-swarm-agent

## License

BSD

## Author Information

This role was created in 2021 by [Mutsa Magonde](https://github.com/mmagonde).
