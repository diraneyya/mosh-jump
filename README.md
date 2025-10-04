# mossh

**The easiest way to get started with the mobile shell (Mosh).**

If you use SSH daily but struggle with dropped connections on trains, in cafes, or on mobile networks, `mossh` could for you. It's similar to interactive SSH, but with resiliance that survives network changes, roaming, and sleep.

## Why mossh?

Getting started with Mosh can be confusing, especially when the issues you are dealing with leave no time to tinker with the Vanilla `mosh` command, or take the time to understand how it actually works.

This is where `mossh` comes in, as an easy and a quick way to evaluate the mobile shell (Mosh).

## How convenient?

Using `mossh` is dead simple:

If you are a developer who has an SSH configuration that allows you to type `ssh myserver` and get to work, then you most probaly can `mossh myserver` and get to work, just as easily.

If this does not work or if it does not connect, then you would know that Mosh is not supported in your envirnoment, and simply move on 😃

Big time saver!

## How does it work?

`mossh` simplifies getting start with Mosh by:

- Automatically installing the Mosh binaries on both the local and the remote machines
- Handling locale issues
- Handling situations where the target may or may not support Mosh (due to different modes of connectivity and occasionally being behind a carrier-grade NAT, e.g. cellular)
- Using SSH to jump to a virtualised resource after connecting to the root node using Mosh (e.g. when using Proxmox).

### The tradeoffs

Mosh isn't perfect, but for unreliable networks, it's worth trying. `mossh` aims to helping you do just that: **testing Mosh with the least amount of friction**.

As you start using Mosh you will notice that:

- ⚠️ **There is no scrolling** - terminal scrolling does not work in Mosh, due to the way it works (use tmux/screen instead, or switch back to ssh for this convenience)
- ⚠️ **UDP ports are required** - in some situations, you will need to mess with your firewall and/or home router settings to be able to use Mosh

## Install mossh

```bash
mkdir -p $HOME/.local/bin
curl -o $HOME/.local/bin/mossh \
    https://raw.githubusercontent.com/diraneyya/mossh/refs/heads/master/mossh
chmod +x $HOME/.local/bin/mossh
# Add to .bashrc if not already added, alternatively, choose
# an local bin folder other than "$HOME/.local/bin"
export PATH="$PATH:$HOME/.local/bin"
```

## Setup mossh

Add your remote server to `$HOME/.ssh/config`:

```sshconfig
# change myserver to any name you would like to use when
# connecting to the server using ssh or mossh
Host myserver
    # change this to the public IP address your target
    # can be accessed on
    HostName 1.2.3.4
    # the username
    User root
    # the SSH port
    Port 22
```

Open UDP ports 60001-60008 on your remote server:

```bash
sudo ufw allow 60001:60008/udp
```

This port range allows up to 8 concurrent Mosh sessions using `mossh` (which are selected automatically based on availability by the underlying command, `mosh`). Each active connection uses a dedicated UDP port from the range.

For VPS providers (DigitalOcean, Ionos, Hetzner, etc.), these ports need to be opened from the firewall settings in your admin dashboard. For remotes living inside of a home network or behind a NAT, you must setup port forwarding on your router to allow Mosh to work on these remote hosts.

## Use mossh

```bash
# Connect to a server via Mosh
mossh myserver

# Connect and immediately SSH to another host
# requires 'internal' to be configured in ~/.ssh/config on myserver
mossh myserver internal

# Use custom Mosh UDP port range
# (ports must be open or otherwise connectivity will fail)
mossh -p 60010:60020 myserver

# Show help
mossh --help
```

## Requirements

- SSH access to your server (configured in `~/.ssh/config`)
- One or more UDP ports open on the server (60001-60008 are tried by default)
- Mosh will be auto-installed on both local and remote if missing

## Learn More

- [Official Mosh website](https://mosh.org)
- [The tutorial that led to this convenience/evaluation script for Mosh](https://forum.proxmox.com/threads/use-mobile-shell-mosh-with-proxmox.173209/)
