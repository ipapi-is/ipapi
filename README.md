# Documentation

1. [Quickstart](#quickstart)
2. [Introduction](#introduction)
3. [API Features](#api-features)
    - [ASN Database](#asn-database)
    - [Hosting IP Ranges Database](#hosting-ip-ranges-database)
4. [API Response Format](#api-response-format)
    - [Top Level API Output](#response-format-top-level-api-output)
    - [The `datacenter` object](#response-format-the-datacenter-object)
    - [The `company` object](#response-format-the-company-object)
    - [The `asn` object](#response-format-the-asn-object)
    - [The `location` object](#response-format-the-location-object)
5. [API Endpoints](#api-endpoints)
    - [GET Endpoint](#get-endpoint---lookup-a-single-ip-address-or-asn)
    - [POST Endpoint](#post-endpoint---query-up-to-100-ip-addresses-in-one-api-call)

---

This IP address API returns useful meta-information for IP addresses. For example, the API response includes the organization of the IP address, ASN information and geolocation intelligence.

Furthermore, the API response allows to derive security information for each IP address, for example whether an IP address belongs to a hosting provider (`is_datacenter`), is a TOR exit node (`is_tor`), if an IP address is a proxy (`is_proxy`) or belongs to an abuser (`is_abuser`).

This API strongly emphasises **datacenter/hosting detection**. A complicated hosting detection algorithm was developed to achieve a high detection rate. [Thousands of different hosting providers](https://ipapi.is/hosting-detection.html) are tracked. Whois records, public hosting IP ranges from hosting providers and a proprietary hosting discovery algorithm are used to decide whether an IP address belongs to a datacenter or not.

The API includes accurate and rich ASN meta-data. For instance, the API output contains whois information for each active ASN and the ASN type is derived by analyzing the company that owns the AS.

## Quickstart

Lookup any IP address: [https://ipapi.is/json/?q=3.5.140.2](https://ipapi.is/json/?q=3.5.140.2)

Lookup your own IP address: [https://ipapi.is/json/](https://ipapi.is/json/)

Usage with JavaScript:

```JavaScript
fetch('https://ipapi.is/json/?q=23.236.48.55')
  .then(res => res.json())
  .then(res => console.log(res));
```

Usage with Curl:

```bash
curl 'https://ipapi.is/json/?q=32.5.140.2'
```

## Introduction

The IP adddress API makes use of the following data sources:

1. Public whois records from regional Internet address registries such as [RIPE NCC](https://www.ripe.net/), [APNIC](https://www.apnic.net/), [ARIN](https://www.arin.net/) and so on
2. BGP information in order to find ASN information and their associated routes/prefixes
3. Public blocklists such as [firehol/blocklist-ipset](https://github.com/firehol/blocklist-ipsets)
4. The API uses several proprietary datacenter/hosting detection algorithms
5. Other open source projects that try to find hosting IP addresses such as [github.com/client9/ipcat](https://github.com/client9/ipcat), [github.com/Umkus/ip-index](https://github.com/Umkus/ip-index) or [https://github.com/X4BNet/lists_vpn](github.com/X4BNet/lists_vpn) are also considered
6. The API uses IP threat data from various honeypots
7. IP geolocation information from several different geolocation providers is used

## API Features

- **Ready for production**: This API can be used in production and is stable
- **Many datacenters supported:** [Thousands of different hosting providers and counting](https://ipapi.is/hosting-detection.html) - From Huawei Cloud Service to ServerMania Inc. Find out whether the IP address is hosted by looking at the `is_datacenter` property!
- **Always updated**: The API database is automatically updated several times per week.
- **AS (Autonomous System) support**: The API provides autonomous system information for each looked-up IP address
- **Company Support**: The API provides organisational information for each network of each looked up IP address
- **Bulk IP Lookups**: You can lookup up to 100 IP addresses per API call

### ASN Database

For offline ASN data access, the [**ASN Database**](https://ipapi.is/asn.html) is provided. The ASN database includes all assigned and allocated AS numbers by IANA and respective meta information. The database is updated several times per week. For active ASN's (at least one route/prefix assigned to the AS), the database includes rich meta information. For example, the provided information for the ASN `50673` would be:

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

[Click here to download the ASN Database](https://ipapi.is/asn.html)

**How to download & parse the database?**

Download and unzip the ASN database:

```bash
cd /tmp
curl -O https://ipapi.is/data/fullASN.json.zip
unzip fullASN.json.zip
```

And parse with nodejs:

```JavaScript
let asnDatabase = require('./fullASN.json');
for (let asn in asnDatabase) {
  console.log(asn, asnDatabase[asn]);
}
```

### Hosting IP Ranges Database

Furthermore, the **Hosting IP ranges Database** is provided for offline and scalable access. This database contains all known datacenter IP ranges in the Internet. A proprietary algorithm was developed to determine if a network belongs to a hosting provider.

The file format of the database is tab separated text file (.tsv), where each line of the file contains the `company`, `network` and `domain` of the hosting provider.

Example excerpt of the database:

```text
Linode, LLC 178.79.160.0 - 178.79.167.255 www.linode.com
OVH Sp. z o. o. 178.32.191.0 - 178.32.191.127 www.ovh.com
myLoc managed IT AG 46.245.176.0 - 46.245.183.255 www.myloc.de
```

[Click here to download the Hosting IP Ranges Database](https://ipapi.is/hosting-detection.html)

**How to download & parse the database?**

Download and unzip the Hosting Ranges database:

```bash
cd /tmp
curl -O https://ipapi.is/data/hostingRanges.tsv.zip
unzip hostingRanges.tsv.zip
```

And parse with nodejs:

```JavaScript
const fs = require('fs');

let hostingRanges = fs.readFileSync('hostingRanges.tsv').toString().split('\n');
for (let line of hostingRanges) {
  let [company, network, domain] = line.split('\t');
  console.log(company, network, domain);
}
```

## API Response Format

The API output format is explaind by walking through an example. Most of the returned information is self-explanatory.

This is how a typical API response looks like. The IP `107.174.138.172` was queried with the API call [https://ipapi.is/json/?q=107.174.138.172](https://ipapi.is/json/?q=107.174.138.172):

```json
{
  "ip": "107.174.138.172",
  "rir": "ARIN",
  "is_bogon": false,
  "is_datacenter": true,
  "is_tor": true,
  "is_proxy": false,
  "is_vpn": false,
  "is_abuser": true,
  "company": {
    "name": "ColoCrossing",
    "domain": "colocrossing.com",
    "network": "107.172.0.0 - 107.175.255.255",
    "whois": "https://ipapi.is/json/?whois=107.172.0.0"
  },
  "datacenter": {
    "datacenter": "ColoCrossing",
    "domain": "www.colocrossing.com",
    "network": "107.172.0.0 - 107.175.255.255"
  },
  "asn": {
    "asn": 36352,
    "route": "107.174.138.0/24",
    "descr": "AS-COLOCROSSING, US",
    "country": "us",
    "active": true,
    "org": "ColoCrossing",
    "domain": "www.colocrossing.com",
    "abuse": "abuse@colocrossing.com",
    "type": "hosting",
    "created": "2005-12-12",
    "updated": "2013-01-08",
    "rir": "arin",
    "whois": "https://ipapi.is/json/?whois=AS36352"
  },
  "location": {
    "country": "us",
    "state": "New York",
    "city": "Buffalo",
    "latitude": "42.882500",
    "longitude": "-78.878800",
    "zip": "14202",
    "timezone": "-05:00",
    "local_time": "2022-12-20 07:31:16.256-0500",
    "local_time_unix": 1671521476.256
  },
  "elapsed_ms": 2.4
}
```

In the following section, the different parts of the API response are explained in-depth.

### Response Format: Top Level API Output

The top level API output looks as follows:

```json
{
  "ip": "107.174.138.172",
  "rir": "ARIN",
  "is_bogon": false,
  "is_datacenter": true,
  "is_tor": true,
  "is_proxy": false,
  "is_vpn": false,
  "is_abuser": true,
  "elapsed_ms": 2.4
}
```

The explanation for the top level API fields is as follows:

- `ip` - `string` - the IP address that was looked up, here it was `107.174.138.172`
- `rir` - `string` - to which [Regional Internet Registry](https://en.wikipedia.org/wiki/Regional_Internet_registry) the IP address belongs. Here it belongs to `ARIN`, which is the RIR responsible for North America
- `is_bogon` - `boolean` - Whether the IP address is bogon. [Bogon IP Addresses](https://en.wikipedia.org/wiki/Bogon_filtering) is the set of IP Addresses not assigned/allocated to IANA and any RIR (Regional Internet Resgistry). For example, the loopback IP `127.0.0.1` is a special/bogon IP address. The IP address `107.174.138.172` is not bogon, hence it is set to `false` here.
- `is_datacenter` - `boolean` - whether the IP address belongs to a datacenter. Here, we have the value `true`, since `107.174.138.172` belongs to the hosting provider `ColoCrossing`.
- `is_tor` - `boolean` - is true if the IP address belongs to the TOR network. This is the case here. Tor detection is accurate, so you can rely on the value of `is_tor`. The API detects most TOR exit nodes reliably.
- `is_proxy` - `boolean` - whether the IP address is a proxy. This is not the case here. In general, the flag `is_proxy` only covers a subset of all proxies in the Internet.
- `is_vpn` - `boolean` - whether the IP address is a VPN. This is not the case with the IP `107.174.138.172`. In general, the flag `is_vpn` only covers a subset of all VPN's in the Internet. It is not possible to detect all VPN exit nodes passively.
- `is_abuser` - `boolean` - is true if the IP address committed abusive actions, which was the case with `107.174.138.172`. Various IP blocklists and threat intelligence feeds are used to populate the `is_abuser` flag.
- `elapsed_ms` - `float` - how much internal processing time was spent in milliseconds (ms). This lookup only took `2.4ms`, which is quite fast.

### Response Format: The `datacenter` object

```json
  "datacenter": {
    "datacenter": "ColoCrossing",
    "domain": "www.colocrossing.com",
    "network": "107.172.0.0 - 107.175.255.255"
  },
```

If the IP address belongs to a datacenter/hosting provider, the API response will include a `datacenter` object with the following attributes:

- `datacenter` - `string` - to which datacenter the IP address belongs. For a full list of datacenters, check the [ipapi.is/json/info endpoint](https://ipapi.is/json/info). In this case, the datacenter's name is `ColoCrossing`.
- `domain` - `string` - The domain name of the company
- `network` - `string` - the network this IP address belongs to (In the above case: `107.172.0.0 - 107.175.255.255`)

Most IP's don't belong to a hosting provider. In those cases, the `datacenter` object will not be present in the API output.

For a couple of large cloud providers, such as Google Cloud, Amazon AWS, DigitalOcean or Microsoft Azure (and some others), the `datacenter` object is more detailed.

[Amazon AWS](https://aws.amazon.com/) example:

```json
{
  "ip": "3.5.140.2",
  "datacenter": {
    "datacenter": "Amazon AWS",
    "network": "3.5.140.0/22",
    "region": "ap-northeast-2",
    "service": "EC2",
    "network_border_group": "ap-northeast-2"
  }
}
```

[DigitalOcean](https://www.digitalocean.com/) example:

```json
{
  "ip": "167.99.241.130",
  "datacenter": {
    "datacenter": "DigitalOcean",
    "code": "60341",
    "city": "Frankfurt",
    "state": "DE-HE",
    "country": "DE",
    "network": "167.99.240.0/20"
  },
}
```

[`Linode`](https://www.linode.com/) example:

```json
{
  "ip": "72.14.182.54",
  "datacenter": {
    "datacenter": "Linode",
    "name": "US-TX",
    "city": "Richardson",
    "country": "US",
    "network": "72.14.182.0/24"
  },
}
```

### Response Format: The `company` object

```json
  "company": {
    "name": "ColoCrossing",
    "domain": "colocrossing.com",
    "network": "107.172.0.0 - 107.175.255.255",
    "whois": "https://ipapi.is/json/?whois=107.172.0.0"
  },
```

Most IP addresses can be associated with an organization or company. The API uses whois database information to infer which organization is the owner of a certain IP address. Most API lookups will have an `company` object with the following attributes:

- `name` - `string` - The name of the company
- `domain` - `string` - The domain name of the company
- `network` - `string` - The network for which the company has ownership
- `whois` - `string` - An url to the whois information for the network of this IP address

### Response Format: The `asn` object

```json
  "asn": {
    "asn": 36352,
    "route": "107.174.138.0/24",
    "descr": "AS-COLOCROSSING, US",
    "country": "us",
    "active": true,
    "org": "ColoCrossing",
    "domain": "www.colocrossing.com",
    "abuse": "abuse@colocrossing.com",
    "type": "hosting",
    "created": "2005-12-12",
    "updated": "2013-01-08",
    "rir": "arin",
    "whois": "https://ipapi.is/json/?whois=AS36352"
  },
```

Most IP addresses can be associated with an Autonomeous System (AS). The `asn` object provides the following attributes:

- `asn` - `int` - The AS number
- `route` - `string` - The IP route as CIDR in this AS
- `descr` - `string` - An informational description of the AS
- `country` - `string` - The country where the AS is situated in (administratively)
- `active` - `string` - Whether the AS is active (active = at least one route administred by the AS)
- `domain` - `string` - The domain of the organization to which this AS belongs
- `org` - `string` - The organization responisible for this AS
- `type` - `string` - The type for this ASN, this is either `hosting`, `education`, `goverment`, `banking`, `business` or `isp`
- `created` - `string` - When the ASN was established
- `updated` - `string` - The last time the ASN was updated
- `rir` - `string` - To which Regional Internet Registry the ASN belongs
- `whois` - `string` - An url to the whois information for this ASN

For inactive autonomeous systems, most of the above information is not available.

### Response Format: The `location` object

```json
  "location": {
    "country": "us",
    "state": "New York",
    "city": "Buffalo",
    "latitude": "42.882500",
    "longitude": "-78.878800",
    "zip": "14202",
    "timezone": "-05:00",
    "local_time": "2022-12-02 08:25:17.411-0500",
    "local_time_unix": 1669969517.411
  },
```

The API provides geolocation information for the looked up IP address. The `location` object includes the following attributes:

- `country` - `string` - The ISO 3166-1 alpha-2 country code to which the IP address belongs. This is the country specific geolocation of the IP address.
- `state` - `string` - The state for the IP address
- `city` - `string` - The city to which the IP address belongs
- `latitude` - `string` - The latitude for the IP address
- `longitude` - `string` - The longitude for the IP address
- `zip` - `string` - The zip code for this IP
- `timezone` - `string` - The timezone for this IP
- `local_time` - `string` - The local time for this IP in human readable format
- `local_time_unix` - `string` - The local time for this IP as unix timestamp
- `possible_other_location` - `array` - (Optional) -  If there are multiple possible locations, the attribute `possible_other_location` is included in the API response. It contains an array of ISO 3166-1 alpha-2 country codes which represent the possible other geolocation countries.

Country level geolocation accuracy is quite good, since the API provides information from several different geolocation service providers.

## API Endpoints

The IP API currently has two endpoints.

### GET Endpoint - Lookup a single IP Address or ASN

This GET endpoint allows to lookup a single IPv4 or IPv6 IP address by specifying the query parameter `q`. Example: `q=142.250.186.110`. You can also lookup **ASN** numbers by specifying the query `q=AS209103`.

- **Endpoint** - `/`
- **Method** - `GET`
- **Parameter** - `q` - The IP address or ASN to lookup
- **Example** - [https://ipapi.is/json/?q=3.5.140.2](https://ipapi.is/json/?q=3.5.140.2)
- **ASN Example** - [https://ipapi.is/json/?q=AS42831](https://ipapi.is/json/?q=AS42831)

### POST Endpoint - Query up to 100 IP Addresses in one API call

You can also make a bulk API lookup with up to 100 IP addresses (Either IPv4 or IPv6) in one single request.

- **Endpoint** - `/`
- **Method** - `POST`
- **Content-Type** - `Content-Type: application/json`
- **Parameter** - `ips` - An array of IPv4 and IPv6 addresses to lookup

For example, in order to lookup the IP addresses

- `162.158.0.0`
- `2406:dafe:e0ff:ffff:ffff:ffff:dead:beef`
- `162.88.0.0`
- `20.41.193.225`

you can use the following POST API request with `curl`:

```bash
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"ips": ["162.158.0.0", "2406:dafe:e0ff:ffff:ffff:ffff:dead:beef", "162.88.0.0", "20.41.193.225"]}' \
  https://ipapi.is/json/
```
