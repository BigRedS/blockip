# Blockip

Safely blocks IP addresses. 

blockip allows for fast and easy blocking/unblocking of IP addresses, preventing
mistaken or inadvertent blocking of things on a whitelist.

## Installation

By default, config files are looked for in the current directory, so you can just
git clone this and use it as-is. Else, config is expected to be in /etc/blockup.

Get a default config by running

    ./blockip --example-config > ./blockip.conf

Which will produce a complete and commented config file. Or use the file 
`bockip.conf.example` from this repo.


## Configuration

There are two configuration files:

* `blockip.static` is intended to be left to be updated from upstream
* `blockip.conf` is for local config changes. 



Blockip has two whitelists; *whitelist* and *confirm-first*

An address on the *confirm-first* list simply trigger a warning and confirmation
that it ought to be blocked:

    # blockip 216.58.211.164 
    Address '216.58.211.164' is listed as confirm-first in ./blockip.conf
    Reason given:
      Google Bot
    
    Are you sure you want to block this address? [Y/n]  n
    Aborting

But one on the *whitelist* is simply rejected:

    # /blockip 103.21.244.0 
    103.21.244.0 is whitelisted, as 'CloudFlare'



By default, iproute2 is used to block the address, using an invocation like

    ip route add blackhole 216.58.211.164

This can be configured using the `block_command` and `unblock_command` options,
see the example config file for details. While the whitelists are set in either
or both config files, this can only be set in the local one (`blockip.conf`).

## Logging

Each invocation of blockip is logged to syslog using the INFO facility.
