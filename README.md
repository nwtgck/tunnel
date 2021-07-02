# Tunnel

Secure, multiplexed, TCP port forwarder using [piping-server](https://github.com/nwtgck/piping-server) by [@nwtgck](https://github.com/nwtgck) as relay. Designed mainly for p2p connections between peers behind (multiple) NAT/firewalls.

# Features

1. TCP tunnel between peers, each of which may be behind (multiple) NAT(s), i.e. unreachable from the public internet.
2. Firewalls don't cause problems as only outgoing http(s) connections are used.
3. Security: To connect, peers must know the unique ID of the serving peer and a shared secret key. Traffic between peer and relay is encrypted (TLS). [Relay doesn't store anything](https://github.com/nwtgck/piping-server#ideas).
4. Multiplexing: Each tunnel supports multiple concurrent connections. Connections are full-duplex.
5. Resilience: Peers auto-reconnect in the face of intermittent connectivity.
6. No superuser privilege required.
7. [Option to host your own relay server (easily and for free)](https://github.com/nwtgck/piping-server#self-host-on-free-services).
8. KISS: Just a single, small, portable, shell-script.

# Command-line

```bash
tunnel [-ivuh] [-a <access-key>] [-p <piping-server>] [<local-port> [<peer-ID:peer-port>]]
```

**Options:**
    **-i**  ID, bound to hardware (MAC), USER, HOME and HOSTNAME
    **-v**  Version
    **-u**  Update to latest version
    **-h**  Help
    **-a**  Pass the shared secret. Can use environment variable TUNNEL_KEY instead.
    **-p**  Pass the piping-server URL. Can use environment variable TUNNEL_RELAY instead. Default: https://ppng.io

**Example:**
    Generate ID to be announced to peers
      `tunnel -i`
    Expose local port 4001 (default IPFS port) for peers to connect to
      `TUNNEL_KEY='shared secret' tunnel 4001`
    Forward local port 9090 to port 4001 of peer
      `tunnel -a 'shared secret' 9090 'peerID:4001'`

# Installation

Download with:

```bash
curl -LO https://raw.githubusercontent.com/SomajitDey/tunnel/main/tunnel
```

Make it executable:

```bash
chmod +x ./tunnel
```

Then install globally with:

```bash
./tunnel -c install
```

If you don't have `sudo` privilege, you can install locally instead:

```bash
./tunnel -c install -l
```

# Dependency/Portability

This program is simply an executable `bash` script depending on standard GNU tools including `socat`, `openssl`, `curl`, `mktemp`, `cut`, `awk`,  `sed` , `flock`, `pkill`, `dd`, `xxd` etc. that are readily available on standard Linux distros.

# Applications

1. `ssh`: Forward local-port `PORT` to `peerID:22`. Then `ssh localhost -p "${PORT}"`.
2. `redis`: Forward local-port `PORT` to `peerID:6379`. Then `redis-cli -p "${PORT}"`.
3. `ipfs`: Forward local-port `PORT` to `peerID:4001`. Then `ipfs swarm connect /ip4/127.0.0.1/tcp/${PORT}/p2p/${IPFS_ID_of_peer}` 
4. RDP or VNC

# See also

- [gsocket](https://github.com/hackerschoice/gsocket)
- [ipfs p2p](https://github.com/ipfs/go-ipfs/blob/master/docs/experimental-features.md#ipfs-p2p) with [circuit-relay enabled](https://gist.github.com/SomajitDey/7c17998825bb105466ef2f9cefdc6d43)
- [go-piping-duplex](https://github.com/nwtgck/go-piping-duplex)
- [pipeto.me](https://pipeto.me)
- [uplink](https://getuplink.de)
- [localhost.run](https://localhost.run/)
- [ngrok](https://ngrok.io)
- [sshreach.me](https://sshreach.me) (free trial for limited period only)
- [more](https://gist.github.com/SomajitDey/efd8f449a349bcd918c120f37e67ac00)

------

###### [Copyright](https://github.com/SomajitDey/tunnel/blob/main/LICENSE) &copy; 2021 [Somajit Dey](https://github.com/SomajitDey)



