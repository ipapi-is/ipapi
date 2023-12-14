# IP Address Databases (ipapi.is)

This repository contains the databases that are used by the commercial IP API <https://ipapi.is>

Please consider subscribing to a paid plan at [https://ipapi.is/pricing.html](https://ipapi.is/pricing.html) to help out the project. The API runs on several servers across the globe and currently handles millions of daily requests.

+ **Commercial** - [IP to Hosting Database](https://ipapi.is/hosting-detection.html) - Contains IP addresses that belong to hosting providers or cloud services such as Amazon AWS or Microsoft Azure. The database contains very small and niche hosting providers.
+ **Free** - [IP to Geolocation Database](https://ipapi.is/geolocation.html) - Contains the geographical location (Including coordinates, city name and country) of millions of unique IPv4 and IPv6 networks.
+ **Free** - [IP to ASN Database](https://ipapi.is/asn.html) - This database includes rich meta data for all active and inactive ASNs of the Internet. Currently, there are around 85.000 active ASNs and hundreds of thousands inactive/unassigned ASNs.

## IP to Hosting Database

The **IP to Hosting Database** is a commercial database that [can be purchased here](https://ipapi.is/hosting-detection.html). The IP to Hosting database contains all known hosting IP ranges of the Internet. The database is continuously updated and new hosting / cloud providers are added as soon as they emerge.

The database considers all of the following services as hosting providers:

+ Normal hosting providers such as [Hetzner.de](https://www.hetzner.de/) or [Leaseweb.com](https://www.leaseweb.com/)
+ Large cloud providers such as [Amazon AWS](https://aws.amazon.com/) or [Microsoft Azure](https://azure.microsoft.com/)
+ Content Delivery Networks such as [Cloudflare](https://www.cloudflare.com/), [Fastly](https://www.fastly.com/) or [edg.io](https://edg.io/)
+ Anti-DDOS services such as [qrator.net](https://qrator.net/) or [ddos-guard.net](https://ddos-guard.net/)
+ IP leasing organizations such as [ipxo.com](https://ipxo.com/) or [interlir.com](https://interlir.com/)
+ Other SaaS, IaaS, or PaaS organizations such as [fly.io](https://fly.io/) or [Heroku](https://www.heroku.com/)

A [proprietary algorithm](https://ipapi.is/blog/detecting-hosting-providers.html) was developed to determine if a network belongs to a hosting provider or not. The database contains more than 470k IPv4 networks and more than 360k IPv6 networks.

The file format of the database is CSV, where each line of the file contains the following fields:

+ `ipVersion` - Determines the IP type. Either 4 or 6.
+ `startIp` - The start IP address of the network range.
+ `endIp` - The end IP address of the network range.
+ `datacenter` - The name of the hosting / datacenter provider
+ `domain` - The domain name of the hosting provider's website

Example excerpt of the database (CSV):

```csv
ipVersion,startIp,endIp,datacenter,domain
4,144.76.148.248,144.76.148.255,Hetzner Online GmbH,www.hetzner.com
4,185.60.30.116,185.60.30.117,"Masergy Communications, Inc",masergy.com
4,165.225.38.0,165.225.39.255,zscaler.com,
4,20.117.192.254,20.117.192.255,Microsoft Azure,
4,193.19.205.0,193.19.205.255,Surfshark Ltd.,surfshark.com
4,104.28.85.186,104.28.85.186,iCloud Private Relay,
4,78.47.80.144,78.47.80.151,Hetzner Online GmbH,www.hetzner.com
4,138.201.240.56,138.201.240.63,Hetzner Online GmbH,www.hetzner.com
4,172.226.201.114,172.226.201.115,iCloud Private Relay,
4,37.140.139.160,37.140.139.161,Teletech d.o.o. Beograd,tele.tech
4,51.91.12.0,51.91.12.255,OVH,www.ovh.com
4,51.38.154.192,51.38.154.207,OVH Sp. z o. o.,www.ovh.com
4,88.198.107.112,88.198.107.127,Hetzner Online GmbH,www.hetzner.com
4,163.44.245.0,163.44.245.7,"GMO Internet Group, Inc.",www.gmo.jp
4,185.112.18.0,185.112.19.255,HISI MUTU,www.groupehisi.fr
4,78.47.155.240,78.47.155.247,Hetzner Online GmbH,www.hetzner.com
4,20.44.73.253,20.44.73.253,Microsoft Azure,
```

+ [IPv4 Sample (CSV)](https://ipapi.is/data/HostingRangesIPv4-Sample.csv)
+ [IPv6 Sample (CSV)](https://ipapi.is/data/HostingRangesIPv6-Sample.csv)

## Geolocation Database

The database includes geolocation information for a large part of the IPv4 address space and a many IPv6 networks. The database is updated several times per week. The accuracy of the data is very good on the country level. For critical applications, it is not recommended to rely on the geolocation database to be accurate to the city level. The database (CSV file) has a `accuracy` column that specifies how accurate the geolocation for that particular network is.

+ [Download the IPv4 Geolocation Database](databases/geolocationDatabaseIPv4.csv.zip)
+ [Download the IPv6 Geolocation Database](databases/geolocationDatabaseIPv6.csv.zip)

The geolocation database is provided as large CSV file with the following header fields:

+ `ip_version` - Either `4` or `6`. Determines the IP type of the network. Example: `"4"`
+ `start_ip` - The first IP address of the network range. Example: `"44.31.140.0"`
+ `end_ip` - The last IP address of the network range. Example: `"44.31.140.255"`
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

## ASN Database

An autonomous system (AS) is a large network or group of networks with a single routing policy. Each AS is assigned a unique ASN, which is a number that identifies the autonomous system (AS).

Most IP addresses belong to an AS. There are many different reasons why ASN metadata can be useful.

For that reason, the [ASN Database](https://ipapi.is/asn.html) is provided for free. The ASN database includes all assigned and allocated AS numbers by IANA and respective meta information. The database furthermore contains unassigned and inactive ASNs.

The ASN database is updated several times per week. For active ASNs (at least one route/prefix assigned to the AS), the database includes rich meta information. For example, the provided information for the ASN `50673` would be:

```JavaScript
"50673": {
  "asn": "50673",
  "org": "Serverius Holding B.V.",
  "domain": "serverius.net",
  "abuse": "abuse@serverius.net",
  "type": "hosting",
  "created": "2010-09-07",
  "updated": "2022-11-15",
  "rir": "ripe",
  "descr": "SERVERIUS-AS, NL",
  "country": "NL",
  "active": true,
  "prefixes": [
    "2.59.183.0/24",
    "5.56.133.0/24",
    // many more IPv4 prefixes ...
  ],
  "prefixesIPv6": [
    "2001:67c:b0::/48",
    "2a00:1ca8::/32",
    // many more IPv6 prefixes ...
  ]
},
```

The database is in JSON format. The key is the ASN as `int` and the value is an object with AS meta information such as the one above.

[Download the ASN Database](https://ipapi.is/asn.html)

### How to download & parse the ASN database?

Download and unzip the ASN database:

```bash
cd /tmp
curl -O https://ipapi.is/data/fullASN.json.zip
unzip fullASN.json.zip
```

And parse with `node`:

```JavaScript
let asnDatabase = require('./fullASN.json');
for (let asn in asnDatabase) {
  console.log(asn, asnDatabase[asn]);
}
```
