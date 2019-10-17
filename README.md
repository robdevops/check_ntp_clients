# check_ntp_clients
`check_ntp_clients` plugin for Nagios / Icinga. Warns about stale clients known to `ntpd` or `chronyd` NTP servers.

## Description
This test is designed to run on an NTP server, and alerts about known NTP clients we haven't heard from in a while. Supports chronyd and ntpd. 

## Limitations
This test uses runtime network statistics counters to discover clients. This means the client list is cleared when ntpd or chronyd are restarted, making it possible to miss a stale client if the server is restarted at an unfortunate time. It is recommended to compliment this test with `check_ntp_time` (from the `nagios-plugins-ntp` or `monitoring-plugins-basic` packages) on each client.

## Usage
```
Usage: check_ntp_clients [-c] [-i ipv4_address] [-t seconds] [-s]

Options:
-c, --chronyd           Fetch data from chronyd instead of ntpd.
-i, --ignore            Client to ignore. Can be specified multiple times.
-t, --threshold         Warning threshold. Minimum 60. Defaults to two days.
-s, --sudo              Invoke sudo for chronyd method. Needs sudo config for '/usr/bin/chronyc -c clients'.
```

## Requirements
* php 5.4
* ntpd or chronyd
    * For ntpd, `ntpdc -nc monlist` must return results. This requires "monitor" not disabled in ntpd config.
    * For chronyd, `chronyc clients` must return results. This requires running it as root, or with the `--sudo` option.
        * The `--sudo` option requires a sudoers entry for the test user to run `/usr/bin/chronyc -c clients`.

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

