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

_As an Amazon Associate, I earn from qualified purchases._

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

## Testing the server

TODO.

## See also

This project relies heavily on the research and guidance of other projects:

  - [Doge Microsystems Dial up server](https://dogemicrosystems.ca/wiki/Dial_up_server)

## License

GPLv3

## Author Information

This project was created in 2026 by [Jeff Geerling](https://www.jeffgeerling.com/).
