## Utils
This is a collection of ansible helper tasks within a role (utils) that are useful in other roles

## Description
This project is an ansible role that provides a few helper tasks that are useful in other roles.
e.g. Waiting for a port to become available, with retries etc.

## Installation

In your role/requirements.yml specify the following (or similar)
```
---
roles:
  - src: http://github.com/gocallag/utils.git
    version: main
    name: utils
    scm: git
```

## Usage

### Waitfor

In your playbooks you can do something like the following :
```
  - include_role:
      name: utils
      tasks_from: waitfor
    vars: 
        ponder: true
        wait_host_ip: "{{ env.qemu_ip }}"
        wait_proto: "{{ (os.type | lower == 'linux' ) | ternary('ssh', 'psrp' ) }}"
        wait_port: "{{ (os.type | lower == 'linux' ) | ternary(env.qemu_ssh_port, env.qemu_psrp_port ) }}"
```
Note: If you specify ponder: true then the waitfor task will check to see if there are patching activities underway and wait for them to complete, this more necessary for windows as you 
often have to wait until a patch event is finished before you try performing other installation tasks.

```
  - include_role:
      name: utils
      tasks_from: download.yml
    vars: 
        download_url: "{{ os.iso }}"
        download_checksum: "{{ os.checksum }}"
        download_dest: "{{ env.build.path }}/{{ id }}/downloads/{{ os.iso | basename }}"
```
Note: This task uses curl amongst other tools to perform the download as curl has proven to be far more robust that ansible module equivalents.
          
## Support
Use the issue tracker

## Authors and acknowledgment
* Geoff O'Callaghan

## License
Apache

## Project status
Active
