snooper-trooper
==

| who       | what |
|-----------|------|
| dockerhub | https://hub.docker.com/r/jspc/snooper-trooper/   |
| circleci  | https://circleci.com/gh/jspc/snooper-trooper   |
| licence   | MIT   |

snooper-trooper is a project containing coreos cloud-config and help to run a dockerised openvpn/ tor box In The Cloud(tm).

Usage
--

Firstly, create a new ssh key. This is important for two reasons: digital ocean will barf if you're already using a particular key in your account, where this tool expects to upload the key to get key IDs, and should your instance be compromised while you're logged in you may suffer a ssh agent leak attack.

Thus:

```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (~/.ssh/id_rsa): ~/.ssh/snooper-trooper
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ~/.ssh/snooper-trooper.
Your public key has been saved in ~/.ssh/snooper-trooper.pub.
```

With this in mind:

```bash
$ ansible-playbook ansible/gateway.yml --extra-vars 'ssh_pub_key_path=~/.ssh/snooper-trooper.pub ssh_prv_key_path=~/.ssh/snooper-trooper'
```

Any `var` in `ansible/gateway.yml` can be overwritten on the cli with `--extra-vars`. Thus to spin up in the nyc3 region, the above command becomes:

```bash
$ ansible-playbook ansible/gateway.yml --extra-vars 'ssh_pub_key_path=~/.ssh/snooper-trooper.pub ssh_prv_key_path=~/.ssh/snooper-trooper region_id=nyc3'
```

Post Install
--

There are a number of manual steps. This was to avoid having to bootstrap the coreos hosts with python to have the full run of features it expects.

1.  Login via ssh and run `docker exec -ti openvpn-as passwd admin` to change the default password (Holy Fuck please don't forget to do this)
1.  Delete the firewall rules stopping access to the UI: `/sbin/iptables -D INPUT -p tcp --destination-port 943 -j DROP`
1.  Login to: `https://<server ip here>:943/admin` to add users/ fiddle with stuff (You don't necessarily need to do this)
1.  Login to: `https://<server ip here>:943` to get your vpn bundle (Same address as above, minus the `/admin` route)


A note on tor
--

This cloud-config comes with tor installed and configured. This affects **only the server** - your  traffic will not be routed via tor. This is for a couple of reasons:

1.  A lot of traffic is really inappropriate for tor; streaming/ downloads for example
1.  There is a performance hit with tor that may not always be appropriate

Thus; to use tor you'll need to either run it locally and connect via SOCKS or use the Tor Browser Bundle.


Licence
--

MIT License

Copyright (c) 2016 jspc

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
