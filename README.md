# mosh-jump

Establish durable terminal sessions using Mosh, with an optional SSH jump action to other hosts.

## Install

```bash
mkdir -p $HOME/.local/bin 
curl -o $HOME/.local/bin/mosh-jump \
    https://raw.githubusercontent.com/diraneyya/mosh-jump/refs/heads/master/mosh-jump
chmod +x $HOME/.local/bin/mosh-jump
# Add to .bashrc if not there
export PATH="$PATH:$HOME/.local/bin"
```

## Setup

Add your jump server to `$HOME/.ssh/config`:

```sshd
Host myserver
    HostName 1.2.3.4
    User root
    Port 22
```

If you want to jump through to other hosts, add them to `$HOME/.ssh/config` on the jump server.

Open UDP ports 60001-60008 on your jump server:

```bash
sudo ufw allow 60001:60008/udp
```

In case of many VPS providers, this can be done from your admin dashboard.

If hosting behind a NAT or a router, port-forwarding must be configured on the router to allow this to work.

## Usage

```bash
# Connect to jump server
HOST=myserver mosh-jump

# Jump through to another server
HOST=myserver mosh-jump internal-host

# Custom Mosh port range
PORTS=60010:60020 HOST=myserver mosh-jump

# Show help
mosh-jump --help
```

## Requirements

- SSH access to jump server
- UDP ports 60001-60008 open on jump server
- Mosh will be auto-installed if missing
