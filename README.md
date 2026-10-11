# GuardWatch-Lab

**Status: planning (2026-10-10).** Nothing is built yet. The direction below is being weighed,
it is not decided.

## The idea

One virtualization host with three layers:

| Layer | What it would be | Running |
|---|---|---|
| Front | A firewall VM, plus the host's own firewall on each VM's network interface | Always on, started first |
| Back | A log server VM (Graylog) that collects the logs of the firewall and of every guest - the guard watch | Always on, started second |
| Middle | Work VMs, for example Windows test installs made from templates | Started when needed, shut down after |

The host being looked at is Proxmox VE.

## Why

- A Windows image can be installed, checked, rolled back to a snapshot and installed again in a
  VM, as often as needed, without re-installing a real PC.
- One place where the logs of the firewall and of the guests come together.

## What the documentation says (looked up 2026-10-10)

**Proxmox VE** (admin guide)

- It is its own operating system (Debian-based) with its own installer, for bare metal. The
  installer removes all existing data on the drives selected for it, so it needs a machine of
  its own.
- It needs a 64-bit CPU with Intel VT or AMD-V. Recommended: 2 GB of memory for Proxmox VE
  itself, plus the memory of every guest.
- A VM can be set to start at boot, with a start order and a delay to the next VM. Shutdown
  runs in the reverse order.
- Templates, full clones, linked clones and snapshots.
- Windows 11 guests: UEFI (OVMF), an EFI disk with Secure Boot keys and a virtual TPM 2.0 are
  supported. Windows needs the VirtIO drivers from an extra ISO during the install.
- A built-in firewall that filters on any VM or container interface.

**Graylog** (documentation of version 7.1)

- Server operating systems in the compatibility matrix: Debian 12, Ubuntu 22.04 / 24.04, the
  RHEL family 8 / 9, SUSE 15, and Docker images. Windows is not in that list.
- Single node install: MongoDB first, then Graylog Data Node, then Graylog server.
- Memory: the heap of the Data Node and the heap of Graylog are each set to half of the system
  memory. The pages read name no minimum.

## Planned steps

1. Install the host on its own machine.
2. Build the firewall VM (front).
3. Build the log server VM (back).
4. Send the first log source into the log server: the firewall.
5. Install a Windows 11 VM from a prepared image and check it.
6. Send Windows logs into the log server.

## Open

- Which machine the host goes on, and how much memory it has.
- How much memory the log server needs on that machine.
- Which firewall the front VM runs.

## Rules of this project

- Own code only. No code is taken from anyone else.
- No APIs in the code, at least for now.
- Related but separate project: WinHardenDebloat,
  https://github.com/Training1990for2026Systems/WinHardenDebloat

## References

- Proxmox VE documentation: https://pve.proxmox.com/pve-docs/
- Graylog documentation: https://go2docs.graylog.org/current/home.htm
- Microsoft Learn: https://learn.microsoft.com/en-us/
- Cloudflare developers: https://developers.cloudflare.com/
- Netgate documentation: https://docs.netgate.com/
- OpenWrt: https://openwrt.org/

---

Built with Claude by Anthropic. Product names belong to their owners; this project is not
affiliated with them and is not endorsed by them.
