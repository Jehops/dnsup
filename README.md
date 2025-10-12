# dnsup – A Simple DNS Updater

## SYNOPSIS

```dnsup [-h|-s]```

## DESCRIPTION
   Update a host's A record when its external IPv4 address changes.  The update
   is performed using a DNS provider's API.  Gandi and Porkbun are supported.
   Support for GoDaddy has been dropped.

```
   -h    Print this help message.
   -s    Be silent except upon error.
```

   A configuration file, typically named <domain>.<record>.conf (e.g.,
   home.example.com.conf), is read from /etc/dnsup/ or /usr/local/etc/dnsup/ for
   each A record to be updated.

   Configuration values

   Common:
```
  provider=<Gandi | Porkbun>
  domain=<domain name>
  name=<Use @ or leave blank for zone root, otherwise subdomain without domain>
  ttl=<TTL in seconds, minimum of 600 for Porkbun>
```

  Gandi:
```
  pat=<Personal Access Token>
```

  Porkbun:
```
  apikey=<API key>
  secretapikey=<secret API key>
```

## API Keys

   To learn about the API keys and configuration values specific to each
   provider, visit these sites.

   * Gandi: https://api.gandi.net/
   * Porkbun: https://porkbun.com/account/api

## COMPATIBILITY
   dnsup is written in POSIX shell.  It requires curl with TLS support and a CA
   certificate store.

## EXAMPLE
```
$ cat /usr/local/etc/dnsup/example.com.conf

provider=porkbun
domain=example.com
apikey=pk1_5c7a9e0f3b1d4862a9c3e5f0d1b476823a5d9c1f0b4e72638d50f91a2c3b4e7d
secretapikey=sk1_f8b3a0e5c7d19246b0e9d5f7c1a83429e6f0d3b5c7a19284f6e3d5c7a0b1d4e9
ttl=600
```

```
$ cat /usr/local/etc/cron.d/dnsup

PATH=/etc:/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin
SHELL=/bin/sh

#minute	hour	mday	month	wday	who	command
*/10     *       *       *       *      dnsup    dnsup >> /var/log/dnsup.log 2>&1
@reboot                                 dnsup    dnsup >> /var/log/dnsup.log 2>&1
```

## AUTHOR
   Joseph Mingrone <jrm@ftfl.ca>

## LICENSE
   dnsup is released under a BSD 2-Clause License.
