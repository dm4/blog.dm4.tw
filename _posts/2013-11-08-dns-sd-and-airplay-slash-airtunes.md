---
layout: post
title: "dns-sd &amp; AirPlay / AirTunes"
tags: [dns-sd, airplay, airtunes]
redirect_from: /2013/11/08/dns-sd-and-airplay-slash-airtunes.html
---

## Usage

    $ dns-sd
    dns-sd -E                  (Enumerate recommended registration domains)
    dns-sd -F                      (Enumerate recommended browsing domains)
    dns-sd -R <Name> <Type> <Domain> <Port> [<TXT>...] (Register a service)
    dns-sd -B        <Type> <Domain>        (Browse for services instances)
    dns-sd -L <Name> <Type> <Domain>           (Look up a service instance)
    dns-sd -P <Name> <Type> <Domain> <Port> <Host> <IP> [<TXT>...]  (Proxy)
    dns-sd -q <name> <rrtype> <rrclass> (Generic query for any record type)
    dns-sd -Z        <Type> <Domain>   (Output results in Zone File format)
    dns-sd -G     v4/v6/v4v6 <name>  (Get address information for hostname)
    dns-sd -V    (Get version of currently running daemon / system service)

## Type

- AirTunes 是 `_raop._tcp.`
- AirPlay 是 `_airplay._tcp.`

## Browse & Look up

    $ dns-sd -B _raop._tcp
    Browsing for _raop._tcp
    Timestamp     A/R Flags if Domain                    Service Type              Instance Name
    17:05:13.971  Add     3  8 local.                    _raop._tcp.               C92A14084F64@Hello

    $ dns-sd -L "C92A14084F64@Hello" _raop._tcp.
    Lookup C92A14084F64@Hello._raop._tcp..local
    17:11:26.993  C92A14084F64@Hello._raop._tcp.local. can be reached at Hello.local.:50391 (interface 8) Flags: 1
     et=0,3,5 cn=1,2,3 da=true sf=0x4 tp=UDP vv=1 pw=false am=AppleTV3,1 txtvers=1 vn=65537 md=0,1,2 vs=150.33 sv=false ch=2 sr=44100 rhd=4.6.5 ss=16

