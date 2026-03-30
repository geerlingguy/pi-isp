# Pi ISP

Run a Raspberry Pi as an old-school dial-up ISP for retro computers.

<p align="center"><img alt="Raspberry Pi ISP hardware" src="/resources/pi-isp-setup.jpeg" height="auto" width="600"></p>

The goal of this repository is to create the simplest possible dial-up emulation setup, not to create a full ISP-like setup with a PBX and multiple phone lines. Just one retro computer 'dialing up' to the Internet through a Raspberry Pi.

For a more elaborate setup, check out [Doge Microsystems Dial-up pool](https://dogemicrosystems.ca/wiki/Dial-up_pool) instructions.

## Hardware Setup

I'm using the following hardware (pictured above):

  - Raspberry Pi 3, 4, or 5 (or equivalent) ($35)
  - [StarTech.com 56K USB Dial-up Modem](https://amzn.to/3NJMeTZ) ($45)
  - [Viking DLE-200B Two-Way Line Simulator](https://amzn.to/3NnJETN) ($120)

There are other hardware configurations you can use for a Pi-based dial-up ISP, and other ways to emulate phone lines for a little less money, but these devices can be purchased new and work reliably.

On the DLE-200B, to increase the data transfer speed to the maximum of 28.8K, switch the 3rd dip switch position to 'ON' (up). The other three can remain 'OFF' (down). This switch reduces audio attenuation and can give more stable data connections.

Alternatively, the [DLE-300 Advanced Line Simulator](https://amzn.to/4dielnX) may handle higher data rates, up to 33.6. To get 56K speeds, you need a more elaborate setup.

Plug the 56K USB Dial-up modem into the Pi, and plug a phone line from it into either of the DLE-200B's phone jacks. Then plug a phone line from the other DLE-200B phone jack into your old computer's modem.

> _As an Amazon Associate, I earn from qualified purchases._

## Software Setup

This project uses Ansible (`pip install ansible`).

To get started:

  1. Copy `example.hosts.yml` to `hosts.yml` (change the hostname or IP to match your Pi).
  1. Copy `example.config.yml` to `config.yml` and modify any settings to your liking.
  1. Install Ansible dependencies: `ansible-galaxy install geerlingguy.security`.

### Run the Ansible Playbook

To set up the Pi as a local ISP, run the Ansible playbook.

```
ansible-playbook main.yml
```

If you have the modem hooked up, the Pi should immediately be ready to answer a call (by default, after one ring).

## Connecting from another computer

> These instructions were written for an iBook running Mac OS 9. Dialup instructions vary widely depending on OS and era!

Create a new Remote Access / Modem Dial-up profile, and put in any phone number you like (e.g. `1` or `314`, or even a full phone number).

The default username and password are `dial` and `dialpi`.

<p align="center"><img alt="iBook G3 Remote Access modem connection" src="/resources/mac-os-9-ibook-dialup-remote-access.png" height="auto" width="300"></p>

Make sure your TCP/IP settings are set to PPP (so it uses the modem connection), and Connect using the Remote Access application.

You should hear the modem dialing the number, and then a modem handshake. After a little more time while the modem quietly authenticates, your old computer _should_ be connected.

> Note: Internet security is _your_ responsibility. I wouldn't leave an old computer connected to the Internet for long periods of time, especially if you go browsing random websites!

Some setups will not run stably at higher speeds, even 33.6 kbps; if you are having connection stability issues, try forcing a lower max modem speed by adjusting the `mgetty_modem_max_speed` variable.

## Proxying Web Traffic

By default, this project also installs [Macproxy Classic](https://github.com/rdmark/macproxy_classic), an HTTP proxy which allows many modern websites to be browsed on vintage browsers, by stripping elements that fail to load on old browsers.

You can disable this with the configuration setting `macproxy_classic_enable`, but if it's enabled, all you need to do to use it—assuming you've dialed into the Pi already—is enter the correct proxy settings on your old browser.

Here's the correct setting for Internet Explorer 5.0 on an old Mac:

<p align="center"><img alt="iBook G3 Remote Access modem connection" src="/resources/internet-explorer-proxy-preferences.png" height="auto" width="400"></p>

By default, the Pi ISP's services are accessible at the IP address `192.168.32.1` (this, as well as the remote device's assigned IP, is configured in the `ppp-options` file, and is currently not configurable through this project's `config.yml`).

Set the HTTP proxy to `192.168.32.1`, and update the `port` to `5001`, and web traffic should go through the proxy now.

There are some websites that run fine _without_ the proxy (e.g. [FrogFind](https://frogfind.com) and [Macintosh Garden](https://macintoshgarden.org)), you can add them to the bypass list.

### Browse the web like it's 1999

Macproxy Classic also includes a Wayback Machine extension, that lets you set a date, and browse the internet as if it were that day, courtesy of Internet Archive's [Wayback Machine](https://web.archive.org).

To enable it, browse to web.archive.org, and click 'Enable'. Then set a date, and start browsing the web. To get back to 'the real Internet', go to web.archive.org again, and click 'disable'.

## Monitoring

When 'dialing' the Pi, observe the mgetty logs:

```
sudo journalctl -fu mgetty
```

If you're browsing on a retro computer, a few websites you can access to see if your connection is working include:

  - http://68k.news/ - "Headlines from the Future"
  - http://frogfind.com/ - Web search for ancient browsers

To see live traffic on the PPP connection, install nload and watch:

```
sudo apt install nload
nload ppp0
```

## Hanging Up

Assuming, for some reason, you can't hang up the modem connection from your old computer, here's how to forcibly 'hang up' the modem:

```
sudo kill `cat /var/run/ppp0.pid`
sudo systemctl restart mgetty
```

## See also

This project relies heavily on the research and guidance of other projects:

  - [Doge Microsystems Dial up server](https://dogemicrosystems.ca/wiki/Dial_up_server)
  - [PPP Dialin Server](https://webmin.com/docs/modules/ppp-dialin-server/)

## License

GPLv3

## Author Information

This project was created in 2026 by [Jeff Geerling](https://www.jeffgeerling.com/).
