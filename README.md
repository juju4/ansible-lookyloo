[![Actions Status - Master](https://github.com/juju4/ansible-lookyloo/workflows/AnsibleCI/badge.svg)](https://github.com/juju4/ansible-lookyloo/actions?query=branch%3Amaster)
[![Actions Status - Devel](https://github.com/juju4/ansible-lookyloo/workflows/AnsibleCI/badge.svg?branch=devel)](https://github.com/juju4/ansible-lookyloo/actions?query=branch%3Adevel)

# lookyloo ansible role

Setup lookyloo server, a web interface that allows you to capture and map the journey of a website page
* https://github.com/Lookyloo/lookyloo
* https://www.lookyloo.eu/docs/main/index.html
* https://lookyloo.circl.lu/


## Requirements & Dependencies

### Ansible
It was tested on the following versions:
 * 2.12

### Operating systems

Tested on Ubuntu 20.04, 22.04.

## Example Playbook

Just include this role in your list.
For example

```
- host: myhost
  roles:
    - juju4.lookyloo
```

## Variables

TBD

## Continuous integration

```
$ pip install molecule docker
$ molecule test
$ MOLECULE_DISTRO=ubuntu:20.04 molecule test --destroy=never
```

## Troubleshooting & Known issues

* Log files
/var/log/lookyloo_error.log
/var/log/lookyloo_message.log

* Tree page returns blank content and error log has `WARNING:Lookyloo:Tree too deep, probably a recursive refresh: maximum recursion depth exceeded while pickling an object.`
Verify that you don't miss javascript/css files (tools/3rdparty.py)

* Folder /var/_lookyloo/.cache/ms-playwright contains browser executables. Adapt if /var is mounted with noexec flag.

* No capture happening?
Issue seems to be with splash web capture.
Rebuilding docker locally does not seem to help.

```
$  curl -v 'http://localhost:8050/render.html?url=http://www.google.com&timeout=10'
*   Trying 127.0.0.1:8050...
* Connected to localhost (127.0.0.1) port 8050 (#0)
> GET /render.html?url=http://www.google.com&timeout=10 HTTP/1.1
> Host: localhost:8050
> User-Agent: curl/7.81.0
> Accept: */*
> 
* Recv failure: Connection reset by peer
* Closing connection 0
curl: (56) Recv failure: Connection reset by peer
```
But manual capture in WebUI works.
Anyway, Lookyloo has switch to its own capture tool [playwright](https://github.com/Lookyloo/PlaywrightCapture) since [Apr 2022](https://github.com/Lookyloo/lookyloo/commit/8d159ffba098c624b7e6216fed54b2c6638d442a)
scrapinghub/splash [latest upstream docker](https://hub.docker.com/r/scrapinghub/splash/tags) is from Aug 2020 and based on Ubuntu 20.04 (bionic) which will be [EOL in Apr 2023](https://ubuntu.com/about/release-cycle). Also, considering issue and pull requests, it is uncertain if [splash repository](https://github.com/scrapinghub/splash/) is still maintained.

## License

BSD 2-clause
