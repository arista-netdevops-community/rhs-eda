# RedHat Summit Event-Driven Ansible with Arista

Repository to demonstrate Ansible EDA at RedHat Summit.

This demo is to show the ability to set a device into BGP Maintenance mode within CloudVision portal.  Once the device is within Maintenance mode CloudVision will send a webhook to the EDA server.  The EDA server will key off of specific json keys to then turn around and alert both service now and slack.  Once the event has been cleared the service now ticket issue will be closed as well as slack alerted.

![Topology](diagrams/topo.svg)

## Links

- [CVP Tasks](https://www.arista.com/en/cg-cv/cv-basic-options-for-handling-tasks#topic_pbf_sst_hlb)
- [EOS Maintenance](https://www.arista.com/en/um-eos/eos-maintenance-mode)

## How to run the demo

#### Install EDA 

##### Installation

### ubuntu

```shell
apt-get --assume-yes install build-essential maven openjdk-17-jdk python3-dev python3-pip
export JDK_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export JAVA_HOME=$JDK_HOME
export PIP_NO_BINARY=jpy
export PATH=$PATH:~/.local/bin
pip3 install -U Jinja2
pip3 install ansible ansible-rulebook ansible-runner wheel
```

### Fedora

```shell
dnf --assumeyes install gcc java-17-openjdk maven python3-devel python3-pip
export JDK_HOME=/usr/lib/jvm/java-17-openjdk
export JAVA_HOME=$JDK_HOME
export PIP_NO_BINARY=jpy
pip3 install -U Jinja2
pip3 install ansible ansible-rulebook ansible-runner wheel
```

All other information such as conditions, rules and alternative installations features can be found within the [readthedocs of EDA]("https://ansible-rulebook.readthedocs.io/en/latest/index.html").

### Set your environmental variables
export SN_HOST=https://devinstance.service-now.com
<br>
export SN_USERNAME=admin
<br>
export SN_PASSWORD=password1234
<br>
export SN_TIMEOUT=60
<br>
export SLACK_TOKEN=T0256789M/B03ffffffff/aaabbbccccccc


From the root of this directory.

```bash
ansible-rulebook --rulebook rulebooks/cvp-alerts.yaml -S $PWD -i rulebooks/inventory --verbose
```