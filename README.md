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

Spin up a coreos box and pass `router-cloudconfig.yml` in to cloud-init. On digital ocean, for example, this is done on the `Create Droplet` page.

On boot, you'll need to:

1.  Login via ssh and run `docker exec -ti openvpn-as passwd admin` to change the default password (Holy Fuck please don't forget to do this)
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
