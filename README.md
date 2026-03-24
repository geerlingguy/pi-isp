# Pi ISP

Run a Raspberry Pi as an old-school dial-up ISP for retro computers.

## Hardware Setup

TODO:

  - Raspberry Pi model recommendation
  - USB modem recommendation
  - Phone line simluator recommendation
  - Wiring diagram or picture

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
