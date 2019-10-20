# check_ntp_clients
`check_ntp_clients` is a plugin for Icinga and Nagios. It warns about stale clients known to NTP servers.

## Description
This test is designed to run on a private NTP server, and alerts about known NTP clients we haven't heard from in a while. It supports `chrony`, `ntpsec` and `ntpd classic` time daemons.

## Limitations
* This test uses runtime network statistics counters to discover clients. This means the client list is cleared when ntpd or chronyd are restarted, making it possible to miss a stale client if the server is restarted at an unfortunate time. It is recommended to compliment this test with `check_ntp_time` (from the `nagios-plugins-ntp` or `monitoring-plugins-basic` packages) on each client.

* There is no IPv6 support at this time.

## Requirements
* php 5.4 or greater
* supported time server
    * For ntpd classic, `ntpdc -nc monlist` must return results. This requires "monitor" not set as disabled in ntpd config.
    * For ntpsec, `ntpq -nc mrulist` must return results. This requires "monitor" not set as disabled in ntpd config.
    * For chrony, `chronyc -c clients` must return results. This requires running it as root, or with the `--sudo` option.
        * The `--sudo` option requires sudo config allowing `sudo /usr/bin/chronyc -c clients`.

## Usage
```
Usage: check_ntp_clients -s <chrony|ntpsec|ntpd> [-hpu] [-i IP_ADDRESS] [-w SECONDS]

Options:
-h, --help              This help.
-i, --ignore            Client to ignore. Can be specified multiple times.
-p, --ping              Ping stale clients, ignore any which do not respond.
-s, --source            Source daemon (chrony, ntpsec, or ntpd). REQUIRED.
-S, --sudo              Use sudo for chrony source. Sudoers must allow '/usr/bin/chronyc -c clients'.
-w, --warning           Warning threshold. Minimum 60. Defaults to two days.

Examples:
check_ntp_clients --source source chrony --sudo --ping --warning 86400
check_ntp_clients -s ntpsec -w 43200
```

### Example output

Normal state:
```
OK: 149 known clients.
```

Warning state:
```
WARN: 1 stale clients:

host.example.com stale for 1 day 2 hours 29 minutes

If this is ok, add the client to the ignore list (recommended), or restart ntpd to clear all known clients.
```

