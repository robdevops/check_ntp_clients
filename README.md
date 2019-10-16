# check_ntp_clients
`check_ntp_clients` plugin for Nagios / Icinga. Warns about stale clients known to ntpd or chronyd servers.

## Description
Warns about known ntp clients we haven't heard from in a while.
Supports chronyd and ntpd (ntpd is the default).

## Requirements
* php 5.4
* ntpd or chronyd

For ntpd, `ntpdc -nc monlist` must return results. This requires "monitor" enabled in ntpd config.
For chronyd, `chronyc clients` must return results. This requires running it as root.

## Usage
```
Usage: check_ntp_clients [ OPTIONS ]
        OPTIONS:
        --ignore <ip address>   Client to ignore. Can be specified multiple times.
        --threshold <seconds>   Warning threshold. Minimum 60. Defaults to two days.
        -c --chronyd    Fetch data from chronyd instead of ntpd
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

