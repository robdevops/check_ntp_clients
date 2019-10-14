# check_ntp_clients
`check_ntp_clients` plugin for Nagios / Icinga. Warns about stale clients.

## Description
Warns about known clients we haven't heard from in a while.

Uses `ntpdc -nc monlist` to query a list of clients and the time since their last check-in.

Requires "monitor" enabled in `/etc/ntp.conf`


## Dependencies
* php 5.4
* ntpd

## Usage
```
Usage: check_ntp_clients [ OPTIONS ]
	OPTIONS:
	--ignore <ip address>
		Client to ignore. Can be specified multiple times.
	--threshold <seconds>
		Warning threshold. Defaults to two days.
```

### Example output
```
OK: 149 known clients.

echo $?
0
```

```
1 stale clients:

host.example.com stale for 9 days 22 hours 29 minutes

If this is ok, add the client to the ignore list (recommended), or restart ntpd to clear all known clients.

echo $?
1
```

