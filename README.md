# IP Address Databases (ipapi.is)

This repository contains the databases that are used by the commercial IP API <https://ipapi.is> and the documentation for the API and all databases.

Please consider subscribing to a paid plan at [https://ipapi.is/pricing.html](https://ipapi.is/pricing.html) to help out the project. The API runs on several servers across the globe and currently handles millions of daily requests.

The following databases are available:

+ [IP to Hosting Database](https://ipapi.is/hosting-detection.html) - **Commercial** - Contains IP addresses that belong to hosting providers or cloud services such as Amazon AWS or Microsoft Azure. The database contains very small and niche hosting providers.
+ [IP to Geolocation Database](https://ipapi.is/geolocation.html) - **Free** - Contains the geographical location (Including coordinates, city name and country) of millions of unique IPv4 and IPv6 networks. The full database can be downloaded directly from this repository.
+ [IP to ASN Database](https://ipapi.is/asn.html) - **Commercial** - This database includes rich meta data for all active and inactive ASNs of the Internet. Currently, there are around 85,000 active ASNs and hundreds of thousands inactive/unassigned ASNs.
+ [IP to VPN Database](https://ipapi.is/vpn-detection.html) - **Commercial** - The IP to VPN Database is a database that contains VPN IP addresses from well-known providers like ExpressVPN and NordVPN. Additionally, the database contains IP ranges from other VPN providers from which the provider's name is not known.
+ [Reverse DNS Database](https://ipapi.is/reverse-dns.html) - **Commercial** - Contains the PTR record (reverse DNS hostname) of every IPv4 address on the Internet. The result of a complete monthly sweep of the routable IPv4 address space: 3,702,258,432 IP addresses queried, yielding more than one billion resolved PTR records.
+ [IP to Abuser Database](https://ipapi.is/ip-to-abuser.html) - **Commercial** - Contains IP addresses that are known to be involved in malicious activities such as hacking attempts, spam, and DDoS attacks.
+ [IP to Company Database](https://ipapi.is/ip-to-company.html) - **Commercial** - Maps IP addresses to the companies that own them, providing detailed information about the organization behind each IP.

## The IP API

The databases in this repository power the IP API at <https://api.ipapi.is>. The API returns geolocation, ASN, company/organization, abuse contact and hosting information for any IPv4 or IPv6 address, as well as the security flags `is_bogon`, `is_mobile`, `is_satellite`, `is_crawler`, `is_datacenter`, `is_tor`, `is_proxy`, `is_vpn` and `is_abuser`. The API can be used anonymously for up to 1,000 requests per day.

+ Look up any IP address: [https://api.ipapi.is/?q=3.5.140.2](https://api.ipapi.is/?q=3.5.140.2)
+ Look up any ASN: [https://api.ipapi.is/?q=AS42831](https://api.ipapi.is/?q=AS42831)
+ Look up your own IP address: [https://api.ipapi.is/](https://api.ipapi.is/)

Usage with `curl`:

```bash
curl 'https://api.ipapi.is/?q=32.5.140.2'
```

Bulk lookups of up to 100 IP addresses in one API call are supported via POST:

```bash
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"ips": ["162.158.0.0", "2406:dafe:e0ff:ffff:ffff:ffff:dead:beef"], "key": "your_api_key"}' \
  https://api.ipapi.is
```

Besides the default JSON output, the API supports several other output formats (all billed the same as JSON):

+ HTML: [https://api.ipapi.is/html?q=172.83.212.160](https://api.ipapi.is/html?q=172.83.212.160) - human-readable HTML page
+ TOON: [https://api.ipapi.is/toon?q=172.83.212.160](https://api.ipapi.is/toon?q=172.83.212.160) - token-efficient format for LLM prompts
+ CSV: [https://api.ipapi.is/csv?q=172.83.212.160](https://api.ipapi.is/csv?q=172.83.212.160) - tabular format for spreadsheets
+ TEXT: [https://api.ipapi.is/text?q=172.83.212.160](https://api.ipapi.is/text?q=172.83.212.160) - plain text key-value pairs

The full API documentation can be found at [https://ipapi.is/developers.html](https://ipapi.is/developers.html).

## Free Database Downloads and Samples

The following files can be downloaded directly from this repository (or from [ipapi.is](https://ipapi.is)):

| Database | Free Download / Sample |
| --- | --- |
| IP to Geolocation (IPv4) | [Full Database (CSV)](databases/geolocationDatabaseIPv4.csv.zip) |
| IP to Geolocation (IPv6) | [Full Database (CSV)](databases/geolocationDatabaseIPv6.csv.zip) |
| IP to Geolocation (Commercial Version) | [CSV IPv4](databases/locationSample.csv), [MMDB IPv4](databases/locationSample.mmdb), [CSV IPv6](databases/locationSample6.csv), [MMDB IPv6](databases/locationSample6.mmdb) |
| IP to Hosting (IPv4) | [CSV](databases/HostingRangesIPv4-Sample.csv), [MMDB](databases/HostingRangesIPv4-Sample.mmdb) |
| IP to Hosting (IPv6) | [CSV](databases/HostingRangesIPv6-Sample.csv), [MMDB](databases/HostingRangesIPv6-Sample.mmdb) |
| ASN Database (keyed by ASN) | [JSON](databases/ASN-Database-Sample.json) |
| IP to ASN | [CSV](databases/asnSample.csv), [MMDB](databases/asnSample.mmdb) |
| IP to VPN (`enumerated-vpn`) | [CSV](databases/Enumerated-VPN-Database-Sample.csv), [MMDB](databases/Enumerated-VPN-Database-Sample.mmdb) |
| IP to VPN (`interpolated-vpn`) | [CSV](databases/Interpolated-VPN-Database-Sample.csv), [MMDB](databases/Interpolated-VPN-Database-Sample.mmdb) |
| Reverse DNS | [RDNSZ](https://ipapi.is/data/samples/Reverse-DNS-Database-Sample.rdnsz), [TSV](https://ipapi.is/data/samples/Reverse-DNS-Database-Sample.tsv) |
| IP to Abuser | [CSV](databases/Abuser-Database-Sample.csv), [MMDB](databases/Abuser-Database-Sample.mmdb) |
| IP to Company | [CSV](databases/companySample.csv), [MMDB](databases/companySample.mmdb) |

## IP to Hosting Database

The **IP to Hosting Database** is a commercial database that [can be purchased here](https://ipapi.is/hosting-detection.html). The **IP to Hosting Database** contains all known hosting IP ranges of the Internet. The database is continuously updated and new hosting / cloud providers are added as soon as they emerge.

The database considers all of the following services as hosting providers:

+ Normal hosting providers such as [Hetzner.de](https://www.hetzner.de/) or [Leaseweb.com](https://www.leaseweb.com/)
+ Large cloud providers such as [Amazon AWS](https://aws.amazon.com/) or [Microsoft Azure](https://azure.microsoft.com/)
+ Content Delivery Networks such as [Cloudflare](https://www.cloudflare.com/), [Fastly](https://www.fastly.com/) or [edg.io](https://edg.io/)
+ Anti-DDOS services such as [qrator.net](https://qrator.net/) or [ddos-guard.net](https://ddos-guard.net/)
+ IP leasing organizations such as [ipxo.com](https://ipxo.com/) or [interlir.com](https://interlir.com/)
+ Other SaaS, IaaS, or PaaS organizations such as [fly.io](https://fly.io/) or [Heroku](https://www.heroku.com/)

A [proprietary algorithm](https://ipapi.is/blog/detecting-hosting-providers.html) was developed to determine if a network belongs to a hosting provider or not. The database contains more than 565,000 IPv4 networks and more than 629,000 IPv6 networks and is constantly growing.

The file format of the database is CSV (or MMDB), where each line of the file contains the following fields:

+ `startIp` - The start IP address of the network range.
+ `endIp` - The end IP address of the network range.
+ `ipVersion` - Determines the IP type of the network. Either `4` or `6`.
+ `datacenter` - The name of the hosting / datacenter provider.
+ `domain` - The domain name of the hosting provider's website.

Example excerpt of the database (CSV):

```csv
startIp,endIp,ipVersion,datacenter,domain
92.222.187.0,92.222.187.255,4,OVH VPS ES,ovh.net
153.120.81.0,153.120.81.255,4,SAKURA Internet Inc.,sakura.ad.jp
185.39.138.85,185.39.138.85,4,MissDomain Group AB,missdomain.com
86.66.23.232,86.66.23.239,4,Internet Services,isi.ch
51.77.100.128,51.77.100.143,4,OVH Ltd,ovh.co.uk
38.142.65.248,38.142.65.255,4,"Imperva, Inc",www.imperva.com
185.103.97.252,185.103.97.255,4,UK Dedicated Servers Ltd,uksrv.co.uk
103.64.80.0,103.64.83.255,4,"Guangdong Aofei Data Technology Co., Ltd.",ofidc.com
194.218.29.104,194.218.29.111,4,Iver Sverige AB,iver.com
135.125.25.152,135.125.25.159,4,OVH SAS,ovhcloud.com
46.165.192.0,46.165.255.255,4,Leaseweb Deutschland GmbH,leaseweb.com
5.198.151.0,5.198.151.127,4,Xidras GmbH,xidras.com
78.41.202.254,78.41.202.254,4,Snel.com B.V.,snel.com
133.32.45.0,133.32.45.255,4,"GMO Internet,Inc.",gmo.jp
5.9.232.240,5.9.232.255,4,Hetzner Online GmbH,hetzner.com
176.9.127.64,176.9.127.95,4,Hetzner Online GmbH,hetzner.com
159.148.115.56,159.148.115.63,4,SIA Latnet,bite.lv
51.91.67.0,51.91.67.255,4,OVH SAS,ovhcloud.com
89.151.66.192,89.151.66.255,4,Pulsant Limited,pulsant.com
156.240.103.0,156.240.103.255,4,Bunny Technology LLC,bunny.net
149.6.42.4,149.6.42.7,4,Netwise Hosting Ltd,netwise.co.uk
```

Database samples:

+ [IPv4 Sample (CSV)](databases/HostingRangesIPv4-Sample.csv) / [IPv4 Sample (MMDB)](databases/HostingRangesIPv4-Sample.mmdb)
+ [IPv6 Sample (CSV)](databases/HostingRangesIPv6-Sample.csv) / [IPv6 Sample (MMDB)](databases/HostingRangesIPv6-Sample.mmdb)

## ASN Database

An autonomous system (AS) is a large network or group of networks with a single routing policy. Each AS is assigned a unique ASN, which is a number that identifies the autonomous system (AS). Most IP addresses belong to an AS. There are many different reasons why ASN metadata can be useful.

[You can purchase the full ASN database here](https://ipapi.is/asn.html) - Sample: [ASN Database Sample (JSON)](databases/ASN-Database-Sample.json)

The ASN database includes all assigned and allocated AS numbers by IANA and respective meta information. The database furthermore contains unassigned and inactive ASNs (currently around 85,000 active ASNs and more than 370,000 inactive/unassigned ASNs).

The ASN database is updated several times per week. For active ASNs (at least one route/prefix assigned to the AS), the database includes rich meta information. For example, the provided information for the ASN `50673` would be:

```JavaScript
{
  "asn": 50673,
  "abuser_score": "0.0013 (Low)",
  "descr": "SERVERIUS-AS, NL",
  "country": "nl",
  "active": true,
  "org": "Serverius Holding B.V.",
  "domain": "serverius.net",
  "abuse": "abuse@serverius.net",
  "type": "hosting",
  "created": "2010-09-07",
  "updated": "2022-11-15",
  "rir": "RIPE",
  "whois": "https://api.ipapi.is/?whois=AS50673",
  "prefixes": [
    "5.56.133.0/24",
    "5.178.64.0/21",
    "5.178.64.0/24",
    "5.188.12.0/22",
    "5.188.12.0/24",
    "5.188.13.0/24",
    // many more routes
  ],
  "prefixesIPv6": [
    "2001:67c:b0::/48",
    "2a00:1ca8::/32",
    "2a00:1ca8:77::/48",
    "2a00:1caa::/32",
    "2a02:1680::/32",
    "2a03:3f40::/32",
    "2a06:8000::/29",
    "2a09:e40::/32",
    "2a09:4d41::/32",
    "2a09:aa80::/32",
    "2a0a:3f40::/32",
    "2a0c:480::/32",
    "2a0e:c9c0::/29",
    "2a0f:4a80::/48"
  ],
  "elapsed_ms": 0.6
}
```

The database is in JSON format. The key is the ASN as `int` and the value is an object with AS meta information such as the one above.

The ASN data is also available as an **IP to ASN** database in CSV or MMDB format, where each row maps an IP range to its ASN metadata ([CSV Sample](databases/asnSample.csv), [MMDB Sample](databases/asnSample.mmdb)):

```csv
startIp,endIp,ipVersion,asn,abuser_score,route,descr,country,active,org,domain,abuse,type,created,updated,rir
1.0.0.0,1.0.0.255,4,13335,0.0154 (Elevated),1.0.0.0/24,"CLOUDFLARENET - Cloudflare, Inc., US",us,true,"Cloudflare, Inc.",cloudflare.com,abuse@cloudflare.com,hosting,2010-07-14,2017-02-17,ARIN
1.0.4.0,1.0.4.255,4,38803,0.0006 (Low),1.0.4.0/24,"GTELECOM-AS-AP - Gtelecom Pty Ltd, AU",au,true,Gtelecom Pty Ltd,gtelecom.com.au,support@gtelecom.com.au,isp,,2024-09-18,APNIC
```

### ASN Database Format

Each entry of the ASN database has the meta data listed below. If the ASN is inactive, there is less metadata for the ASN. For example, inactive ASNs have no `prefixes` or `prefixesIPv6` fields.

+ `asn` - The field `asn` is a unique identifier assigned to each autonomous system (AS) on the Internet. Currently, there are more than 85,000 active Autonomous System Numbers (ASNs) registered ([ASN Statistics](https://ipapi.is/data/asn_meta.json)).
+ `abuser_score` - The field `abuser_score` represents the quota of abusive IP addresses in all routes of the ASN. The higher this number is, the more abusive the whole ASN is. Example: `0.0013 (Low)`
+ `descr` - The field `descr` is a short informal descriptive name of the autonomous system (AS) in free format.
+ `country` - The [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1) to which the AS belongs administratively. This is the country specific location of the autonomous system (AS). This does not necessarily mean that all routes (IP addresses) assigned to this AS must originate from the same country.
+ `active` - The field `active` determines if the ASN is active or inactive. Active means that at least one route is assigned to the ASN. Put differently, this means that there is at least one entry in the `prefixes` or `prefixesIPv6` array.
+ `org` - The field `org` contains the organization that owns the ASN. It is crucial to understand that not every IP address assigned to an AS must belong to the organization contained in `org`. Put differently: The IP addresses included in the `prefixes` or `prefixesIPv6` array might be owned by a different organizations than the one from the autonomous system (AS) itself.
+ `domain` - The field `domain` is the domain of the organization's website that owns the ASN.
+ `abuse` - The field `abuse` contains the abuse email address that can be used to direct abuse complains to the owner of the ASN.
+ `type` - The field `type` contains the type of the ASN. The following ASN types exist:
  + `hosting` - The AS is owned by a hosting provider (Example: [as396982](https://api.ipapi.is?q=as396982))
  + `education` - The AS belongs to a university or other kind of educational institution (Example: [AS1111](https://api.ipapi.is?q=AS1111))
  + `government` - The AS belongs to a governmental institution (Example: [AS1701](https://api.ipapi.is?q=AS1701))
  + `banking` - The AS belongs to a banking / financial institution (Example: [AS2134](https://api.ipapi.is?q=AS2134))
  + `isp` - The AS belongs to a Internet Service Provider (ISP) (Example: [AS3222](https://api.ipapi.is?q=AS3222))
  + `business` - If the type is not one of the above, the type is the generic `business` type (Example: [AS3271](https://api.ipapi.is?q=AS3271))
+ `created` - The field `created` specifies the date when this ASN was first registered.
+ `updated` - The field `updated` specifies the date when this ASN was last updated.
+ `rir` - The field `rir` contains the Regional Internet Registry (RIR) responsible for this ASN.
+ `whois` - The field `whois` contains an url pointing to the raw WHOIS record for this ASN.
+ `prefixes` - The field `prefixes` contains all the IPv4 networks assigned to this ASN. This is a list of IPv4 networks in CIDR format.
+ `prefixesIPv6` - The field `prefixesIPv6` contains all the IPv6 networks assigned to this ASN. This is a list of IPv6 networks in CIDR format.

## Geolocation Database

The **IP to Geolocation Database** is free and can be downloaded directly from this repository. The database includes geolocation information for a large part of the IPv4 address space and many IPv6 networks. The database is updated several times per week. The accuracy of the data is very good on the country level. For critical applications, it is not recommended to rely on the geolocation database to be accurate to the city level. The database (CSV file) has an `accuracy` column that specifies how accurate the geolocation for that particular network is.

+ [Download the IPv4 Geolocation Database](databases/geolocationDatabaseIPv4.csv.zip) (more than 3.1 million networks)
+ [Download the IPv6 Geolocation Database](databases/geolocationDatabaseIPv6.csv.zip) (more than 1.1 million networks)

The geolocation database is provided as large CSV file with the following header fields:

+ `ip_version` - Either `4` or `6`. Determines the IP type of the network. Example: `4`
+ `start_ip` - The first IP address of the network range. Example: `44.31.140.0`
+ `end_ip` - The last IP address of the network range. Example: `44.31.140.255`
+ `continent` - The continent as two letter code. Example: `"NA"`
+ `country_code` - The [ISO 3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO_3166-1) to which the IP address belongs. This is the country specific geolocation of the IP address. Example: `"US"`
+ `country` - The full name of the country. Example: `"United States"`
+ `state` - The state / administrative area for the queried IP address. Example: `"California"`
+ `city` - The city to which the IP address belongs. Example: `"Fremont"`
+ `zip` - The postal code for this IP. Example: `"94720"`
+ `timezone` - The timezone for this IP. Example: `"America/Los_Angeles"`
+ `latitude` - The geographical latitude for the IP address. Example: `"37.54827"`
+ `longitude` - The geographical longitude for the IP address. Example: `"-121.98857"`
+ `accuracy` - Geolocation information is not always accurate. For that reason, there is an `accuracy` number that gives an estimate of how accurate the geolocation information is. Entries with `accuracy = 1` have the highest accuracy, rows with `accuracy = 5` have the least accuracy.
  + `accuracy = 1` - Very high geolocation accuracy. You can rely the data to be accurate to the city level.
  + `accuracy = 2` - High geolocation accuracy. You can often rely the data to be accurate to the city level.
  + `accuracy = 3` - Medium geolocation accuracy. You can not rely the data to be accurate to the city level.
  + `accuracy = 4` - Low geolocation accuracy. Usually the data is accurate to the country level.
  + `accuracy = 5` - Very low geolocation accuracy. The data should be accurate to the country level.
+ `source` - The source from which the geolocation information was derived. Examples: `whoisDescrAttr` (WHOIS `descr` attribute), `whoisGeolocAttr` (WHOIS `geoloc` attribute), `ownGeofeedData` (RFC 8805 geofeeds), `whoisArin`, `whoisLacnic`, `whoisOnlyCountryAvailable`

Example excerpt of the database (CSV):

```csv
ip_version,start_ip,end_ip,continent,country_code,country,state,city,zip,timezone,latitude,longitude,accuracy,source
4,1.0.0.0,1.0.0.255,OC,AU,Australia,Victoria,Research,3760,Australia/Melbourne,-37.7,145.18333,3,whoisDescrAttr
4,1.0.1.0,1.0.1.255,AS,CN,China,Beijing,Beijing,100000,Asia/Shanghai,39.9075,116.39723,3,whoisDescrAttr
4,1.0.2.0,1.0.3.255,AS,CN,China,Beijing,Beijing,100000,Asia/Shanghai,39.9075,116.39723,3,whoisDescrAttr
4,1.0.4.0,1.0.4.255,OC,AU,Australia,New South Wales,Warren,2824,Australia/Sydney,-31.70224,147.83392,3,whoisDescrAttr
```

There is also a commercial version of the geolocation database with higher accuracy, more meta data (such as `isDst` and geofeed sources) and MMDB support: [CSV IPv4 Sample](databases/locationSample.csv), [MMDB IPv4 Sample](databases/locationSample.mmdb), [CSV IPv6 Sample](databases/locationSample6.csv), [MMDB IPv6 Sample](databases/locationSample6.mmdb). Learn more on the [IP to Geolocation page](https://ipapi.is/geolocation.html).

### Convert Geolocation Database to MMDB Format

Remove the first column from [geolocationDatabaseIPv4.csv](databases/geolocationDatabaseIPv4.csv.zip):

```bash
cut -d, -f2- geolocationDatabaseIPv4.csv > dataIPv4.csv
```

Then convert using [mmdbctl](https://github.com/ipinfo/mmdbctl):

```bash
mmdbctl import --in dataIPv4.csv --out dataIPv4.mmdb
```

That works like a charm and yields a ready-to-use `dataIPv4.mmdb` file with all networks.

## IP to VPN Database

The [IP to VPN Database](https://ipapi.is/vpn-detection.html) is a database that contains VPN IP addresses from well-known providers like ExpressVPN and NordVPN. Additionally, the database contains IP ranges from other VPN providers from which the provider's name is not known. The database is updated on a regular basis and is available for purchase in CSV or MMDB format.

Despite the fact that VPN services bring a lot of benefits to its users, they also have a dark side. VPN services are often used by cybercriminals to hide their real IP address and to bypass geo-restrictions and commit fraud. The IP to VPN Database is a valuable tool for businesses that want to detect VPN usage on their website or app and to take appropriate measures to protect their services.

The IP to VPN Database contains three different datasets with different detection accuracy and different false positive rates:

+ `enumerated-vpn` - VPN IPs detected by **Exit Node Enumeration**. IP addresses in this dataset are obtained by automatically logging in to different VPN regions and grabbing the public IP address. This dataset is free from false positives. Exit nodes are removed after a maximum staleness of 45 days. ([CSV Sample](databases/Enumerated-VPN-Database-Sample.csv), [MMDB Sample](databases/Enumerated-VPN-Database-Sample.mmdb))
+ `interpolated-vpn` - VPN IPs detected by **Interpolation**, using the `enumerated-vpn` dataset as ground truth: If more than a threshold share of all IP addresses of a small network are known to belong to a certain VPN service, all IP addresses of this network are attributed to this VPN service. Very small false positive rate. ([CSV Sample](databases/Interpolated-VPN-Database-Sample.csv), [MMDB Sample](databases/Interpolated-VPN-Database-Sample.mmdb))
+ `assumed-vpn` - Assumed VPN IP addresses based on the IP ranges of VPN services. Contains false positives and must be used with caution.

The `enumerated-vpn` dataset contains the following fields (the mandatory fields are `ipVersion`, `startIp`, `endIp`, `serviceName`, `serviceUrl`, `exitNodeType`, `lastSeen` and `lastSeenStr`, the other fields are optional):

+ `ipVersion` - Either `4` (IPv4) or `6` (IPv6). Determines the IP type of the network. Example: `4`
+ `startIp` - The first IP address of the network range. Example: `146.70.185.68`
+ `endIp` - The last IP address of the network range. Example: `146.70.185.68`
+ `serviceName` - The name of the VPN provider. Example: `MullvadVPN`
+ `serviceUrl` - The url of the VPN provider. Example: `https://mullvad.net`
+ `exitNodeType` - Either `"exit_node"` (the IP address is a VPN exit node detected by the VPN enumeration system) or `"vpn_server"` (the IP address is a VPN server known to belong to the VPN service).
+ `lastSeen` - The unix timestamp in milliseconds of the last time the VPN exit node was enumerated. Example: `1738732422048`
+ `lastSeenStr` - The last seen date in human readable format. Example: `2025-02-05T05:13:42.048Z`
+ `exitNodeRegion` - The region of the VPN exit node in free format. Example: `Singapore - CBD`
+ `hostname` - The hostname associated with the IP address. Example: `us-nyc-ovpn-603`
+ `countryCode` - The ISO 3166-1 alpha-2 two-letter country code where the VPN exit node is located. Example: `us`
+ `cityName` - The name of the city where the VPN exit node is located. Example: `New York`
+ `latitude` - The latitude of the VPN exit node's location. Example: `40.71427`
+ `longitude` - The longitude of the VPN exit node's location. Example: `-74.00597`

Example excerpt of the `enumerated-vpn` database (CSV):

```csv
startIp,endIp,ipVersion,serviceName,serviceUrl,exitNodeType,lastSeen,lastSeenStr,exitNodeRegion,hostname,countryCode,cityName,latitude,longitude
45.14.193.5,45.14.193.5,4,NordVPN,https://nordvpn.com,exit_node,1783270858483,2026-07-05T17:00:58.483Z,no226.nordvpn.com,,NO,,,
107.170.235.165,107.170.235.165,4,TunnelBear,https://tunnelbear.com,exit_node,1783386256931,2026-07-07T01:04:16.931Z,,,,,,
83.219.96.34,83.219.96.34,4,ExpressVPN,https://expressvpn.com,exit_node,1779087431055,2026-05-18T06:57:11.055Z,jamaica,,JM,,,
45.134.20.109,45.134.20.109,4,ExpressVPN,https://expressvpn.com,exit_node,1779098436659,2026-05-18T10:00:36.659Z,australia-woolloomooloo,,AU,Woolloomooloo,-33.87042,151.21968
151.240.44.38,151.240.44.38,4,Private Internet Access,https://privateinternetaccess.com,exit_node,1781718537869,2026-06-17T17:48:57.869Z,us-wisconsin,,US,Wisconsin,,
```

### How to use the IP to VPN Database?

This example shows how to work with the IP to VPN Database in MMDB format. First, you have to download the database sample:

```bash
curl -O https://ipapi.is/data/samples/Enumerated-VPN-Database-Sample.mmdb
```

And then you can read the database with [mmdbctl](https://github.com/ipinfo/mmdbctl):

```bash
mmdbctl read -f json-pretty 2.26.12.14 Enumerated-VPN-Database-Sample.mmdb
```

which outputs:

```json
{
  "exitNodeType": "vpn_server",
  "ip": "2.26.12.14",
  "ipVersion": "4",
  "lastSeen": "1783942716787",
  "lastSeenStr": "2026-07-13T11:38:36.787Z",
  "network": "2.26.12.14-2.26.12.14",
  "serviceName": "PublicVpnConfigs"
}
```

## Reverse DNS Database

The [Reverse DNS Database](https://ipapi.is/reverse-dns.html) contains the PTR record (reverse DNS hostname) of every IPv4 address on the Internet. It is the result of a complete sweep of the routable IPv4 address space: **3,702,258,432 IP addresses queried, yielding 1,039,377,899 resolved PTR records**. The database is re-crawled from scratch and re-published every month.

Reverse DNS hostnames are one of the most information-dense signals that exist for an IP address. A single PTR record such as `ec2-3-137-190-186.us-east-2.compute.amazonaws.com` reveals the hosting provider, the service type and the region of an IP address - while `dynamic-077-183-045-122.77.183.pool.telefonica.de` immediately identifies a residential dial-up customer. The ipapi.is API derives several of its signals - such as `is_datacenter`, `is_vpn` and the detection of new hosting providers - in part from this reverse DNS data.

Unlike most reverse DNS datasets, this database stores the DNS outcome for **every single queried IPv4 address**, not only the IP addresses that have a PTR record. For each IP, one of the following statuses is recorded: `has_ptr`, `noerror_empty`, `nxdomain`, `servfail`, `refused`, `timeout`, `net_error` or `lame_delegation`. Where a PTR hostname could be verified with a forward lookup, the database additionally stores the Forward-Confirmed reverse DNS (FCrDNS) result.

The database ships in `.rdnsz`, a purpose-built binary format that stores the complete sweep - all 3.7 billion query results including all hostnames - in roughly **0.99 GB** (less than a byte per queried IP address). The format achieves this with IP delta encoding, hostname templating and Zstandard block compression. The binary format specification is included in the package, so you can implement a reader in any language - the reference implementations in JavaScript and Go are less than 300 lines each.

A converted excerpt of the database looks like this (IP address, tab, PTR hostnames):

```
1.0.16.108	st2-smtp.kakeibo.tepco.co.jp
1.0.64.20	20.64.0.1.megaegg.ne.jp
1.0.64.40	40.64.0.1.megaegg.ne.jp
1.0.64.60	60.64.0.1.megaegg.ne.jp
```

### How to use the Reverse DNS Database?

Every database package includes `rdnsz_query.js`, a self-contained reader tool with zero npm dependencies - it only requires Node.js >= 22.15. To try it out, download the sample database and the tool:

```bash
curl -O https://ipapi.is/data/samples/Reverse-DNS-Database-Sample.rdnsz
curl -O https://ipapi.is/src/rdnsz_query.js
```

Look up a single IP address:

```bash
node rdnsz_query.js lookup 1.0.64.20 Reverse-DNS-Database-Sample.rdnsz
```

which outputs:

```json
{
  "ip": "1.0.64.20",
  "status": "has_ptr",
  "ptr": [
    "20.64.0.1.megaegg.ne.jp"
  ],
  "fcrdns_match": true
}
```

Show the crawl statistics of a database file or convert it to plain text (TSV) / JSONL:

```bash
node rdnsz_query.js stats Reverse-DNS-Database-Sample.rdnsz
node rdnsz_query.js dump Reverse-DNS-Database-Sample.rdnsz --limit 5
```

Typical use cases for the Reverse DNS Database are IP classification and enrichment, threat intelligence and abuse analysis, email deliverability checks, asset discovery / attack surface management, Internet research and offline reverse DNS lookups with zero latency. Because each monthly release is a complete, self-contained snapshot, you can also diff two releases to detect infrastructure changes. Read more on the [Reverse DNS Database page](https://ipapi.is/reverse-dns.html).

## IP to Abuser Database

The [IP to Abuser Database](https://ipapi.is/ip-to-abuser.html) is a database that contains IP addresses known for abusive behavior. The database is aggregated from more than 150 independent blocklist sources, updated on a regular basis and available for purchase in CSV or MMDB format. Currently, the database contains more than 5 million abusive IP addresses.

Abusive IP addresses can be used for various malicious activities, including spam, hacking attempts, and DDoS attacks. The IP to Abuser Database is a valuable tool for businesses that want to detect and prevent abuse on their website or app and to take appropriate measures to protect their services.

The IP to Abuser Database is provided as a CSV or MMDB file with the following fields:

+ `ipVersion` - Either `4` (IPv4) or `6` (IPv6), determining the IP type of the network.
+ `startIp` - The start IP address of the range in string format. Example: `45.86.210.31`
+ `endIp` - The end IP address of the range in string format. Example: `45.86.210.31`
+ `isAbuser` - A boolean value indicating whether the IP is known for abusive behavior. Example: `true`
+ `sourceCount` - The number of independent blocklist sources where this IP address was found. A higher number indicates the IP is more widely recognized as abusive. Example: `7`
+ `totalSources` - The total number of active blocklist sources used by the database. This allows you to calculate the percentage of sources where an IP appears. Example: `154`

The database is sorted by `sourceCount` in descending order. IPs with higher `sourceCount` values are found on more independent blocklists and are generally considered more reliably abusive.

Example excerpt of the database ([CSV Sample](databases/Abuser-Database-Sample.csv), [MMDB Sample](databases/Abuser-Database-Sample.mmdb)):

```csv
startIp,endIp,ipVersion,isAbuser,sourceCount,totalSources
93.174.95.106,93.174.95.106,4,true,34,154
86.54.31.38,86.54.31.38,4,true,33,154
118.26.111.107,118.26.111.107,4,true,32,154
80.82.77.33,80.82.77.33,4,true,31,154
```

## IP to Company Database

The [IP to Company Database](https://ipapi.is/ip-to-company.html) contains the name, domain and type for every company / organization in the Internet that owns an IP address or IP network. IP ownership of companies is obtained by querying the five major WHOIS registries responsible for number resources: ARIN, RIPE, APNIC, LACNIC, and AFRINIC. The IP to Company Database is updated in regular intervals to ensure the correctness of the firmographic data.

The IP to Company Database allows you to accurately classify traffic according to different criteria, such by company name or type. For example, traffic from an organization classified as hosting tends to have a lower reputation compared to traffic from a government or education network.

The file format of the IP to Company Database is either in CSV or MMDB format ([CSV Sample](databases/companySample.csv), [MMDB Sample](databases/companySample.mmdb)) and contains the following fields:

+ `startIp` - The start IP address of the range in string format. Example: `117.200.64.0`
+ `endIp` - The end IP address of the range in string format. Example: `117.200.239.255`
+ `ipVersion` - Either `4` (IPv4) or `6` (IPv6), determining the IP type of the network.
+ `company` - The name of the company. Example: `Broadband Networks`
+ `abuser_score` - The field `abuser_score` represents the quota of abusive IP addresses of the network belonging to the organization (company). The higher this number is, the more abusive the whole network is. `abuser_score = Number of Abusive IPs / Number of Total IPs in the Network`
  - Example: `0.0183 (Elevated)`
  - `Very High` - More than 20% of all IPs of the network are abusive
  - `High` - Between 3% to 20% of all IPs of the network are abusive
  - `Elevated` - Between 0.85% to 3% of all IPs of the network are abusive
  - `Low` - Between 0.85% to 0.05% of all IPs of the network are abusive
  - `Very Low` - Less than 0.05% of all IPs of the network are abusive
+ `domain` - The domain of the Company. Example: `bsnl.co.in`
+ `type` - The field `type` contains the type of the company. The following company types exist:
  - `hosting` - The company is a hosting provider (Example: `5.161.56.240`)
  - `education` - The company is a university or other kind of educational institution (Example: `129.114.137.255`)
  - `government` - The company is a governmental institution (Example: `137.75.167.171`)
  - `banking` - The company is a banking/financial institution (Example: `199.67.175.0`)
  - `isp` - The company is an Internet Service Provider (ISP) (Example: `117.200.226.44`)
  - `business` - If the type is not one of the above, the type is the generic business type (Example: `17.133.85.230`)
+ `rir` - The Regional Internet Registry responsible for the IP range. Example: `APNIC`
+ `netname` - The network name as registered in the WHOIS record (the `NetName` field in ARIN records and the `netname` field in RIPE/APNIC/AFRINIC records). This is a per-network identifier, not a per-org one, so two networks belonging to the same organization can have different netnames. Empty for LACNIC-administered space (which has no equivalent field). Example: `CHINANET-FJ`
+ `abuseName` - The abuse contact name, the name of the identity that receives abuse complaints about the network. The abuse contact name is mostly the same name as in `company`. Example: `ABUSE BSNLIN`
+ `abuseAddress` - The abuse contact address. This is the physical address of abuse contact of the entity that owns the network. Example: `Internet Cell, Bharat Sanchar Nigam Limited., 8th Floor,148-B Statesman House, Barakhamba Road, New Delhi - 110 001`
+ `abuseEmail` - The abuse contact email. This email address takes abuse complaints about incidents associated with the network. Example: `abuse1@bsnl.co.in`
+ `abusePhone` - The abuse contact phone number. This phone number takes abuse complaints about incidents associated with the network. Example: `+000000000`

## Contact

If you have any questions, suggestions or if you found an error in the data, please [contact us](https://ipapi.is/contact.html). Errors in the data can also be reported directly on [ipapi.is](https://ipapi.is/).
