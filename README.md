# Pi ISP

Run a Raspberry Pi as an old-school dial-up ISP for retro computers.

The goal of this repository is to create the simplest possible dial-up emulation setup, not to create a full ISP-like setup with a PBX and multiple phone lines. Just one retro computer 'dialing up' to the Internet through a Raspberry Pi.

For a more elaborate setup, check out [Doge Microsystems Dial-up pool](https://dogemicrosystems.ca/wiki/Dial-up_pool) instructions.

## Hardware Setup

I'm using the following hardware:

  - Raspberry Pi 3, 4, or 5 (or equivalent) ($35)
  - [StarTech.com 56K USB Dial-up Modem](https://amzn.to/3NJMeTZ) ($45)
  - [Viking DLE-200B Two-Way Line Simulator](https://amzn.to/3NnJETN) ($120)

TODO: Add an image of the setup here.

There are other hardware configurations you can use for a Pi-based dial-up ISP, and other ways to emulate phone lines for a little less money, but these devices can be purchased new and work reliably.

On the DLE-200B, to increase the data transfer speed to the maximum of 28.8K, switch the 3rd dip switch position to 'ON' (up). The other three can remain 'OFF' (down). This switch reduces audio attenuation and can give more stable data connections.

Alternatively, the [DLE-300 Advanced Line Simulator](https://amzn.to/4dielnX) may handle higher data rates, up to 33.6. To get 56K speeds, you need a more elaborate setup.

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
