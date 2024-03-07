# IP Address Databases (ipapi.is)

This repository contains the databases that are used by the commercial IP API <https://ipapi.is>

Please consider subscribing to a paid plan at [https://ipapi.is/pricing.html](https://ipapi.is/pricing.html) to help out the project. The API runs on several servers across the globe and currently handles millions of daily requests.

+ [IP to Hosting Database](https://ipapi.is/hosting-detection.html) - **Commercial** -  Contains IP addresses that belong to hosting providers or cloud services such as Amazon AWS or Microsoft Azure. The database contains very small and niche hosting providers.
+ [IP to Geolocation Database](https://ipapi.is/geolocation.html) - **Free** - Contains the geographical location (Including coordinates, city name and country) of millions of unique IPv4 and IPv6 networks.
+ [IP to ASN Database](https://ipapi.is/asn.html) - **Commercial** - This database includes rich meta data for all active and inactive ASNs of the Internet. Currently, there are around 85.000 active ASNs and hundreds of thousands inactive/unassigned ASNs.

## IP to Hosting Database

The **IP to Hosting Database** is a commercial database that [can be purchased here](https://ipapi.is/hosting-detection.html). The **IP to Hosting Database** contains all known hosting IP ranges of the Internet. The database is continuously updated and new hosting / cloud providers are added as soon as they emerge.

The database considers all of the following services as hosting providers:

+ Normal hosting providers such as [Hetzner.de](https://www.hetzner.de/) or [Leaseweb.com](https://www.leaseweb.com/)
+ Large cloud providers such as [Amazon AWS](https://aws.amazon.com/) or [Microsoft Azure](https://azure.microsoft.com/)
+ Content Delivery Networks such as [Cloudflare](https://www.cloudflare.com/), [Fastly](https://www.fastly.com/) or [edg.io](https://edg.io/)
+ Anti-DDOS services such as [qrator.net](https://qrator.net/) or [ddos-guard.net](https://ddos-guard.net/)
+ IP leasing organizations such as [ipxo.com](https://ipxo.com/) or [interlir.com](https://interlir.com/)
+ Other SaaS, IaaS, or PaaS organizations such as [fly.io](https://fly.io/) or [Heroku](https://www.heroku.com/)

A [proprietary algorithm](https://ipapi.is/blog/detecting-hosting-providers.html) was developed to determine if a network belongs to a hosting provider or not. The database contains more than 470k IPv4 networks and more than 360k IPv6 networks and is constantly growing.

The file format of the database is CSV, where each line of the file contains the following fields:

+ `ipVersion` - Determines the IP type of the network. Either `4` or `6`.
+ `startIp` - The start IP address of the network range.
+ `endIp` - The end IP address of the network range.
+ `datacenter` - The name of the hosting / datacenter provider.
+ `domain` - The domain name of the hosting provider's website.

Example excerpt of the database (CSV):

```csv
ipVersion,startIp,endIp,datacenter,domain
4,37.252.231.48,37.252.231.55,powered by ANX,anx.io
4,188.213.5.210,188.213.5.210,GINERNET S.L.,ginernet.com
4,78.46.131.96,78.46.131.127,Hetzner Online GmbH,www.hetzner.com
4,208.52.169.0,208.52.169.255,"MacStadium, Inc.",macstadium.com
4,194.145.196.0,194.145.197.255,Prager Connect GmbH,www.prager-it.com
4,69.172.96.12,69.172.96.12,Cloudfire s.r.l.,www.cloudfire.it
4,83.138.157.160,83.138.157.175,Centric Telecom Limited,rackspace.com
4,192.84.39.0,192.84.39.255,"Network Solutions, LLC",www.networksolutions.com
4,88.99.132.200,88.99.132.207,Hetzner Online GmbH,www.hetzner.com
4,200.107.232.0,200.107.239.255,Redes y Telecomunicaciones,ifxnetworks.com
4,185.193.80.0,185.193.83.255,TerraTransit AG,terratransit.de
4,185.133.34.0,185.133.34.255,"HugeServer Networks, LLC",www.hugeserver.com
4,91.207.112.0,91.207.113.255,Webland AB,webland.se
4,212.223.29.168,212.223.29.175,Ratiokontakt GmbH,ratiokontakt.de
4,31.6.80.0,31.6.95.255,Grid Telekom,grid.com.tr
4,62.216.229.208,62.216.229.215,Equinix Customer - BEEKS FINANCIAL CLOUD LIMITED,eu.equinix.com
4,80.77.95.0,80.77.95.255,level0 is a webhosting organization,www.webhosting.fm
4,52.245.16.0,52.245.16.63,Microsoft Azure,azure.microsoft.com
4,185.90.131.136,185.90.131.143,Envia Tel GmbH,www.enviatel.de
4,146.185.16.0,146.185.17.255,Hosting Services Inc,www.ingenuitycloudservices.com
4,217.182.245.64,217.182.245.127,OVH BV,www.ovh.com
4,5.104.231.60,5.104.231.60,Snel.com B.V.,www.snel.com
4,149.210.141.0,149.210.141.255,Transip Bv.,www.transip.eu
```

+ [IPv4 Sample (CSV)](https://ipapi.is/data/HostingRangesIPv4-Sample.csv)
+ [IPv6 Sample (CSV)](https://ipapi.is/data/HostingRangesIPv6-Sample.csv)

## ASN Database

An autonomous system (AS) is a large network or group of networks with a single routing policy. Each AS is assigned a unique ASN, which is a number that identifies the autonomous system (AS). Most IP addresses belong to an AS. There are many different reasons why ASN metadata can be useful.

[IP to ASN Database Sample (JSON)](https://ipapi.is/data/HostingRangesIPv4-Sample.csv)

The ASN database includes all assigned and allocated AS numbers by IANA and respective meta information. The database furthermore contains unassigned and inactive ASNs.

The ASN database is updated several times per week. For active ASNs (at least one route/prefix assigned to the AS), the database includes rich meta information. For example, the provided information for the ASN `50673` would be:

```JavaScript
{
  "asn": 50673,
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

[You can purchase the full ASN database here](https://ipapi.is/asn.html)

### ASN Database Format

Each entry of the **IP to ASN Database** has the meta data listed below. If the ASN is inactive, there is less metadata for the ASN. For example, inactive ASNs have no `prefixes` or `prefixesIPv6` fields.

+ `asn` - The field `asn` is a unique identifier assigned to each autonomous system (AS) on the Internet. As of January 2024, there were over 81,000 active Autonomous System Numbers (ASNs) registered ([ASN Statistics](https://ipapi.is/data/asn_meta.json)).
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

### Convert Geolocation Database to MMDB Format

Remove first column from [geolocationDatabaseIPv4.csv](https://github.com/ipapi-is/ipapi/blob/main/databases/geolocationDatabaseIPv4.csv.zip):

```
cut -d, -f2- geolocationDatabaseIPv4.csv > dataIPv4.csv
```

Then convert using [mmdbctl](https://github.com/ipinfo/mmdbctl):

```
mmdbctl import --in dataIPv4.csv --out dataIPv4.mmdb
```

That works like a charm and yields:

```
writing to dataIPv4.mmdb (1347178 entries)
```
