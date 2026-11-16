# IP address databases by ipapi.is

[![GitHub stars](https://img.shields.io/github/stars/ipapi-is/ipapi?style=flat)](https://github.com/ipapi-is/ipapi)
[![Last update](https://img.shields.io/github/last-commit/ipapi-is/ipapi?label=last%20update)](https://github.com/ipapi-is/ipapi/commits/main)
[![Geolocation database: CC BY-SA 4.0](https://img.shields.io/badge/geolocation%20database-CC%20BY--SA%204.0-blue)](#license)
[![API: 1,000 free requests a day](https://img.shields.io/badge/API-1%2C000%20free%20requests%20a%20day-brightgreen)](https://ipapi.is/pricing.html?ref=github)

[ipapi.is](https://ipapi.is/?ref=github) answers three questions about any IPv4 or IPv6 address: who owns it, where it is, and what kind of address it is (datacenter, VPN, proxy, Tor node, known abuser). This repository holds what you can have without paying:

- **A free IP geolocation database.** 4.8 million IPv4 and 1.4 million IPv6 ranges with country, state, city, postal code, coordinates and timezone, as CSV. Commercial use is allowed, and there is no account and no license key.<br>
  **[Download IPv4](https://github.com/ipapi-is/ipapi/raw/main/databases/geolocationDatabaseIPv4.csv.zip)** (67 MB) · **[Download IPv6](https://github.com/ipapi-is/ipapi/raw/main/databases/geolocationDatabaseIPv6.csv.zip)** (12 MB) · [How to use it](#free-ip-geolocation-database)
- **Samples of the seven commercial databases**: [hosting ranges](#ip-to-hosting), [VPN exit nodes](#ip-to-vpn), [ASNs](#ip-to-asn), [companies](#ip-to-company), [abusive addresses](#ip-to-abuser), [full geolocation](#ip-to-geolocation) and [reverse DNS](#reverse-dns).
- **A quick start for the API** these databases power. It is free for 1,000 requests a day: [The API](#the-api).

## Free IP geolocation database

```bash
curl -LO https://github.com/ipapi-is/ipapi/raw/main/databases/geolocationDatabaseIPv4.csv.zip
curl -LO https://github.com/ipapi-is/ipapi/raw/main/databases/geolocationDatabaseIPv6.csv.zip
```

The files are replaced every few weeks; the same ones are on [ipapi.is](https://ipapi.is/geolocation.html?ref=github#free-geolocation-db). Each row is a range of addresses:

```csv
ip_version,start_ip,end_ip,continent,country_code,country,state,city,zip,timezone,latitude,longitude,accuracy,source
4,1.0.0.0,1.0.0.255,OC,AU,Australia,Queensland,South Brisbane,4101,Australia/Brisbane,-27.48034,153.02049,5,whoisDescrAttr
4,1.0.1.0,1.0.1.255,AS,CN,China,Beijing,Beijing,100000,Asia/Shanghai,39.9075,116.39723,5,whoisDescrAttr
4,1.0.2.0,1.0.3.255,AS,CN,China,Beijing,Beijing,100000,Asia/Shanghai,39.9075,116.39723,5,whoisDescrAttr
```

| Column | Meaning |
|---|---|
| `ip_version` | `4` or `6` |
| `start_ip`, `end_ip` | First and last address of the range. IPv6 addresses are written out in full |
| `continent` | Two-letter continent code, such as `OC` |
| `country_code`, `country` | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) code and English name |
| `state`, `city`, `zip` | State or region, city and postal code |
| `timezone` | IANA time zone, such as `Australia/Brisbane` |
| `latitude`, `longitude` | Coordinates |
| `accuracy` | The rating of the row, from `1` (very high) to `5` (very low) |
| `source` | Where the location comes from: `whoisArin` or `whoisLacnic` (the registrant's address in an ARIN or LACNIC record), `whoisGeolocAttr` (the `geoloc` coordinates a network operator put into its WHOIS record) or `whoisDescrAttr` (a place named in the record's description) |

### What it is good for

The locations come from the WHOIS records of the regional internet registries, so the database says where a network is registered. For an office, a university or a regional ISP that is where its addresses are used, and the country is right for the vast majority of addresses. For a hosting provider or a global carrier the city often is not: `85.10.199.76` is a server in Nuremberg, and the free database places it in Gunzenhausen, where its provider Hetzner has its seat. It also does not cover every address.

The [full database](#ip-to-geolocation) covers the entire IPv4 and IPv6 address space and adds what network operators publish about their own ranges ([RFC 8805 geofeeds](https://datatracker.ietf.org/doc/rfc8805/)), reverse DNS and latency measurements. It is the data behind the API, which puts the same address in Nuremberg with accuracy `VERY_HIGH`.

### Ranges are nested

The file is sorted by `start_ip` and keeps WHOIS networks at every level: a provider's allocation and, inside it, the blocks it assigned to its customers. Nine out of ten rows lie inside a wider one, so an address can match several rows. **The smallest range that contains the address is the most specific record, and the one to use.**

```csv
4,93.179.92.0,93.179.92.255,EU,RU,Russia,Lipetsk Oblast,Syrskoye,398533,Europe/Moscow,52.586659,39.486826,2,whoisGeolocAttr
4,93.179.92.0,93.179.95.255,EU,RU,Russia,Moscow,Moscow,101000,Europe/Moscow,55.755773,37.617761,2,whoisGeolocAttr
4,93.179.93.0,93.179.93.255,EU,RU,Russia,Moscow Oblast,Kolomna,140400,Europe/Moscow,55.07108,38.78399,1,whoisDescrAttr
4,93.179.94.0,93.179.94.255,EU,RU,Russia,Irkutsk Oblast,Irkutsk,664000,Asia/Irkutsk,52.29566,104.29076,1,whoisDescrAttr
```

`93.179.94.10` is in the second row and in the fourth. The fourth is the smaller range, so the address is in Irkutsk, not in Moscow.

### Convert it to MMDB

A MaxMind DB file gives you fast lookups from every programming language and takes care of the nesting. Build one with [mmdbctl](https://github.com/ipinfo/mmdbctl). Sort the widest ranges first, so that every more specific range overwrites the part of the wider one it covers:

```bash
unzip geolocationDatabaseIPv4.csv.zip

{ head -1 geolocationDatabaseIPv4.csv
  tail -n +2 geolocationDatabaseIPv4.csv | LC_ALL=C sort -t, -k2,2V -k3,3Vr
} | cut -d, -f2- > ipv4.csv

mmdbctl import --range-multicol --in ipv4.csv --out geolocationDatabaseIPv4.mmdb
```

For the IPv6 file, use `LC_ALL=C sort -t, -k2,2 -k3,3r` instead: its addresses are written out in full, so they sort as text. Then look an address up:

```bash
mmdbctl read -f json-pretty 128.146.65.112 geolocationDatabaseIPv4.mmdb
```

```json
{
  "accuracy": "3",
  "city": "Columbus",
  "continent": "NA",
  "country": "United States",
  "country_code": "US",
  "ip": "128.146.65.112",
  "latitude": "39.96118",
  "longitude": "-82.99879",
  "network": "128.146.0.0-128.146.255.255",
  "source": "whoisArin",
  "state": "Ohio",
  "timezone": "America/New_York",
  "zip": "43215"
}
```

The file works with every MaxMind DB reader, for example [maxminddb](https://pypi.org/project/maxminddb/) for Python and [mmdb-lib](https://www.npmjs.com/package/mmdb-lib) for Node.js:

```python
import maxminddb

with maxminddb.open_database('geolocationDatabaseIPv4.mmdb') as reader:
    print(reader.get('128.146.65.112'))
```

```js
const fs = require('fs');
const mmdb = require('mmdb-lib');

const reader = new mmdb.Reader(fs.readFileSync('geolocationDatabaseIPv4.mmdb'));
console.log(reader.get('128.146.65.112'));
```

### License

The free geolocation database is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). You may use it for any purpose, including commercial ones, if you credit ipapi.is with a link wherever you show data from it:

```html
IP geolocation data by <a href="https://ipapi.is/">ipapi.is</a>
```

If you publish a modified version of the database, or another database built from it, license it under CC BY-SA 4.0 as well. The notice is in [databases/LICENSE-free-geolocation-database.txt](databases/LICENSE-free-geolocation-database.txt) and inside both zip files. It covers the free geolocation database only: the sample files show the format of the commercial databases, which are licensed under the [ipapi.is terms](https://ipapi.is/terms.html?ref=github).

## The API

One request returns everything ipapi.is knows about an address. A [free account](https://ipapi.is/app/signup?ref=github) includes 1,000 requests a day with the complete response, commercial use included:

```bash
curl "https://api.ipapi.is/?q=187.13.197.43&key=your_api_key"
```

```json
{
  "ip": "187.13.197.43",
  "rir": "RIPE",
  "is_bogon": false,
  "is_mobile": false,
  "is_satellite": false,
  "is_anycast": false,
  "is_crawler": false,
  "is_datacenter": true,
  "is_tor": false,
  "is_proxy": false,
  "is_vpn": true,
  "is_abuser": false,
  "vpn": {
    "service": "NordVPN",
    "type": "exit_node",
    "last_seen_str": "2026-11-13T14:55:05.677Z",
    "exit_node_region": "France #1168"
  },
  "datacenter": {
    "datacenter": "PacketHub S.A.",
    "network": "187.13.197.0 - 187.13.197.255"
  },
  "company": {
    "name": "PacketHub S.A.",
    "abuser_score": "0.0117 (Elevated)",
    "type": "hosting"
  },
  "abuse": {
    "email": "abuse@packethub.tech"
  },
  "asn": {
    "asn": 212238,
    "org": "Datacamp Limited",
    "type": "hosting"
  },
  "location": {
    "country": "France",
    "city": "Marseille 01",
    "timezone": "Europe/Paris",
    "accuracy": "MEDIUM"
  }
}
```

The response is shortened here; the [documentation](https://ipapi.is/developers.html?ref=github) describes every field. Bulk lookups of up to 100 addresses, lookups of an AS number with all its prefixes, and HTML, CSV, text and TOON output are part of the same endpoint.

Without a key the API answers 30 requests a day per client address, with the owner and the location only: no detection flags, no bulk lookups, no AS numbers. Code examples in several languages are in [NikolaiT/IP-Address-API](https://github.com/NikolaiT/IP-Address-API), a Node.js client and Express middleware in [NikolaiT/ipapi.is-node](https://github.com/NikolaiT/ipapi.is-node).

## The commercial databases

The data behind the API is also sold as downloads, for lookups without a network call or for loading into your own systems. The databases come in CSV and MMDB and are rebuilt about twice a week; reverse DNS has a format and a schedule of its own. The samples below are a few hundred rows of each. Figures as of November 2026.

| Database | What it contains | Samples |
|---|---|---|
| [IP to Hosting](#ip-to-hosting) | 564,000 IPv4 and 769,000 IPv6 ranges of more than 29,000 hosting, cloud and CDN providers | [CSV](databases/HostingRangesIPv4-Sample.csv), [MMDB](databases/HostingRangesIPv4-Sample.mmdb), IPv6: [CSV](databases/HostingRangesIPv6-Sample.csv), [MMDB](databases/HostingRangesIPv6-Sample.mmdb) |
| [IP to VPN](#ip-to-vpn) | 393,000 exit node addresses of 73 VPN services, VPN ranges inferred from them, open proxies | [CSV](databases/Enumerated-VPN-Database-Sample.csv), [MMDB](databases/Enumerated-VPN-Database-Sample.mmdb), inferred: [CSV](databases/Interpolated-VPN-Database-Sample.csv), [MMDB](databases/Interpolated-VPN-Database-Sample.mmdb) |
| [IP to ASN](#ip-to-asn) | 82,000 active autonomous systems with 1.3 million routes, organization, type and abuse contact | [JSON](databases/ASN-Database-Sample.json), [CSV](databases/asnSample.csv), [MMDB](databases/asnSample.mmdb) |
| [IP to Company](#ip-to-company) | The owner, domain, type and abuse contact of nearly 8 million networks | [CSV](databases/companySample.csv), [MMDB](databases/companySample.mmdb) |
| [IP to Abuser](#ip-to-abuser) | 17 million addresses reported for attacks, spam and scanning by 54 vetted sources; the nodes of the Tor network | [CSV](databases/Abuser-Database-Sample.csv), [MMDB](databases/Abuser-Database-Sample.mmdb) |
| [IP to Geolocation](#ip-to-geolocation) | The location of the entire IPv4 and IPv6 address space | [CSV](databases/locationSample.csv), [MMDB](databases/locationSample.mmdb), IPv6: [CSV](databases/locationSample6.csv), [MMDB](databases/locationSample6.mmdb) |
| [Reverse DNS](#reverse-dns) | The PTR record of every IPv4 address: 1.16 billion hostnames in 1.08 GB | [RDNSZ](https://ipapi.is/data/samples/Reverse-DNS-Database-Sample.rdnsz), [TSV](https://ipapi.is/data/samples/Reverse-DNS-Database-Sample.tsv) |

Each database is a monthly subscription, and the [Business plan](https://ipapi.is/business-subscription.html?ref=github) includes all seven together with a server that runs the whole API in your own infrastructure. [Pricing](https://ipapi.is/pricing.html?ref=github).

In every CSV file a row is a range from `startIp` to `endIp`, both included, and `ipVersion` is `4` or `6`. In the MMDB files all values are strings, and a lookup also returns the matched `network`. Read them like the geolocation file above:

```bash
mmdbctl read -f json-pretty 178.63.179.241 databases/HostingRangesIPv4-Sample.mmdb
```

### IP to Hosting

Which addresses belong to a hosting provider, and to which one. People browse from home, office and mobile networks; a request from a datacenter is a server talking: a bot, a scraper, a proxy or a VPN. Hosting and cloud providers, CDNs, DDoS protection services, IP leasing companies and app platforms all count. Thousands of the providers are small hosts that publish no range list; they are found by [classifying the organizations behind WHOIS records](https://ipapi.is/blog/detecting-hosting-providers.html?ref=github). [Product page](https://ipapi.is/hosting-detection.html?ref=github).

```csv
startIp,endIp,ipVersion,datacenter,domain
178.63.179.240,178.63.179.247,4,Hetzner Online GmbH,hetzner.com
51.178.189.32,51.178.189.47,4,OVH SAS,ovhcloud.com
```

`datacenter` is the provider's name and `domain` its website. The package also has `HostingProviders.csv`, an index of all providers with the number of addresses each one holds.

### IP to VPN

Which addresses belong to a VPN service, and to which one. The core is found by [connecting to the VPN services](https://ipapi.is/blog/systematic-vpn-detection.html?ref=github) around the clock and recording the addresses their traffic leaves from. [Product page](https://ipapi.is/vpn-detection.html?ref=github).

| File | Contains | False positives |
|---|---|---|
| `enumerated-vpn` | Exit nodes we observed, and servers the services list themselves. Removed after 45 days without a new sighting | None |
| `interpolated-vpn` | Ranges in which exit nodes are so dense that the whole range is a VPN service's | Very few |
| `assumed-vpn` | Ranges that WHOIS describes as VPN infrastructure, or that hold exit nodes too sparsely for the tier above | Possible |
| `proxy` | Open HTTP and SOCKS proxies | |

```csv
startIp,endIp,ipVersion,serviceName,serviceUrl,exitNodeType,lastSeen,lastSeenStr,exitNodeRegion,hostname,countryCode,cityName,latitude,longitude,claimedCountryCode,claimedCityName,claimedLatitude,claimedLongitude
89.37.63.229,89.37.63.229,4,MullvadVPN,https://mullvad.net,exit_node,1794338034465,2026-11-10T19:13:54.465Z,se-sto-wg-204,,SE,Stockholm,59.32938,18.06871,,,,
```

| Column | Meaning |
|---|---|
| `serviceName`, `serviceUrl` | The VPN service and its website |
| `exitNodeType` | `exit_node`: we saw traffic leave the VPN from this address. `vpn_server`: the service lists it as one of its servers |
| `lastSeen`, `lastSeenStr` | The last sighting, as Unix time in milliseconds and as ISO 8601 |
| `exitNodeRegion`, `hostname` | The service's own name for the location, and the server's hostname |
| `countryCode`, `cityName`, `latitude`, `longitude` | Where the exit node is |
| `claimedCountryCode`, `claimedCityName`, `claimedLatitude`, `claimedLongitude` | Where the service says it is, when that is another country (a virtual location) |

`interpolated-vpn` has `serviceName` and `serviceUrl` only; a range shared by several services names all of them.

### IP to ASN

Every autonomous system of the internet, from BGP routing tables and the WHOIS records of all registries. The CSV and MMDB files map each announced range to its AS, for lookups by address. The JSON file is keyed by AS number, lists all prefixes of each AS, and also has the 376,000 inactive ones. [Product page](https://ipapi.is/asn.html?ref=github).

```csv
startIp,endIp,ipVersion,asn,abuser_score,route,descr,country,active,org,domain,abuse,type,created,updated,rir
1.0.0.0,1.0.0.255,4,13335,0.001 (Low),1.0.0.0/24,"CLOUDFLARENET - Cloudflare, Inc., US",us,true,"Cloudflare, Inc.",cloudflare.com,abuse@cloudflare.com,hosting,2010-07-14,2017-02-17,ARIN
```

| Column | Meaning |
|---|---|
| `asn`, `descr` | The AS number and its description |
| `route` | The announced prefix that covers the range |
| `org`, `domain` | The organization that holds the AS and its website. Addresses announced by an AS can belong to other organizations; see [IP to Company](#ip-to-company) |
| `type` | `hosting` ([Amazon](https://ipapi.is/asn/16509.html?ref=github)), `isp` ([Deutsche Telekom](https://ipapi.is/asn/3320.html?ref=github)), `business` ([Apple](https://ipapi.is/asn/714.html?ref=github)), `education` ([MIT](https://ipapi.is/asn/3.html?ref=github)), `government` ([NASA](https://ipapi.is/asn/1701.html?ref=github)) or `banking` ([JPMorgan Chase](https://ipapi.is/asn/7743.html?ref=github)) |
| `abuser_score` | The share of the AS's addresses seen in abuse, with a label: see [IP to Company](#ip-to-company) |
| `country`, `rir` | The country the AS is registered in, and its registry |
| `active` | Whether the AS announces any route |
| `abuse` | The address for abuse reports |
| `created`, `updated` | Registration and last change in WHOIS |

### IP to Company

The organization each network is registered to, from the WHOIS records of the five regional registries and several national ones, down to the customer where a provider has assigned a block. [Product page](https://ipapi.is/ip-to-company.html?ref=github).

```csv
startIp,endIp,ipVersion,company,abuser_score,domain,type,rir,netname,abuseName,abuseAddress,abuseEmail,abusePhone
18.0.0.0,18.31.255.255,4,Massachusetts Institute of Technology,0 (Very Low),mit.edu,education,ARIN,MIT,Massachusetts Institute of Technology,"Cambridge, MA, 02139-4307, US",arin-mit-security@mit.edu,+1-617-324-1782
```

| Column | Meaning |
|---|---|
| `company`, `domain` | The organization and its website |
| `type` | `hosting`, `isp`, `business`, `education`, `government` or `banking` |
| `abuser_score` | The share of the network's addresses seen in abuse, with a label: `Very Low` (under 0.05%), `Low` (up to 0.85%), `Elevated` (up to 3%), `High` (up to 20%) or `Very High` |
| `rir`, `netname` | The registry, and the name of the network in its WHOIS record |
| `abuseName`, `abuseAddress`, `abuseEmail`, `abusePhone` | Where to report abuse |

### IP to Abuser

Addresses that are attacking the internet now: guessing passwords, scanning for vulnerable services, sending spam. They come from sources that observe abuse themselves and let their entries expire, among them our own honeypots. The package also has `tor`, the current nodes of the Tor network: exit nodes and other relays. [Product page](https://ipapi.is/ip-to-abuser.html?ref=github).

```csv
startIp,endIp,ipVersion,isAbuser,sourceCount,totalSources
193.32.162.15,193.32.162.15,4,true,24,54
```

`sourceCount` is the number of sources that report the address, out of `totalSources`. The file is sorted by it, so the addresses most sources agree on come first.

### IP to Geolocation

The full edition of the free database above, and the data behind the `location` object of the API. IPv4 and IPv6 are separate files. [Product page](https://ipapi.is/geolocation.html?ref=github).

```csv
startIp,endIp,continent,countryCode,country,state,city,zip,timezone,isDst,latitude,longitude,accuracy
20.34.76.0,20.34.76.255,NA,US,United States,Arizona,Phoenix,85001,America/Phoenix,0,33.44838,-112.07404,1
```

`accuracy` runs from `1` (very high) to `5` (very low) and is what the API reports as `VERY_HIGH` to `VERY_LOW`. `isDst` says whether daylight saving time was in effect when the database was built. How accurate the locations are is measured in a [live benchmark of 18 providers against GPS positions](https://ipapi.is/blog/ip-geolocation-accuracy-benchmark.html?ref=github).

### Reverse DNS

The result of [our own sweep of the whole IPv4 address space](https://ipapi.is/blog/crawling-the-complete-ipv4-reverse-dns-space.html?ref=github): 3.7 billion addresses queried, 1.16 billion PTR records found, and the DNS status of every address without one. It also has 7.4 million IPv6 PTR records from a walk of the `ip6.arpa` tree. A new crawl pass is published every one to two months. [Product page](https://ipapi.is/reverse-dns.html?ref=github).

```
1.0.16.108	st2-smtp.kakeibo.tepco.co.jp
1.0.64.20	20.64.0.1.megaegg.ne.jp
```

The database ships in `.rdnsz`, a binary format that holds the whole sweep in 1.08 GB, with a reader that needs nothing but Node.js 22.15 or newer:

```bash
curl -O https://ipapi.is/data/samples/Reverse-DNS-Database-Sample.rdnsz
curl -O https://ipapi.is/src/rdnsz_query.js
node rdnsz_query.js lookup 1.0.64.20 Reverse-DNS-Database-Sample.rdnsz
```

## Wrong data, questions

Report wrong data on the [corrections page](https://ipapi.is/corrections.html?ref=github); network operators can submit their geofeed there too. For everything else there is [info@ipapi.is](mailto:info@ipapi.is), or an issue in this repository.
