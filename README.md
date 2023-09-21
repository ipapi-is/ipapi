# Free IP Address Databases (ipapi.is)

This repository contains the free databases that are used by the commercial IP API <https://ipapi.is>

Please consider subscribing to a paid plan at [https://ipapi.is/pricing.html](https://ipapi.is/pricing.html) to help out the project. The API runs on several servers across the globe and currently handles millions of daily requests.

+ [Geolocation Database](https://ipapi.is/geolocation.html) - Contains millions of unique IPv4 and IPv6 networks with the corresponding geolocation metadata.
+ [ASN Database](https://ipapi.is/asn.html) - This database includes rich meta data for all active and inactive ASN's of the Internet. Currently, there are around 85.000 active ASN's.
+ [Hosting IP Ranges Database](https://ipapi.is/hosting-detection.html) - Contains IP addresses that belong to hosting providers or cloud services such as Amazon AWS or Microsoft Azure. Contains very small and niche hosting providers.

## Geolocation Database

The database includes geolocation information for a large part of the IPv4 address space and a many IPv6 networks. The database is updated several times per week. The accuracy of the data is very good on the country level. It is not recommended to rely on geolocation intelligence to be accurate to the city level for critical applications.

+ [Download the IPv4 Geolocation Database](databases/geolocationDatabaseIPv4.csv.zip)
+ [Download the IPv6 Geolocation Database](databases/geolocationDatabaseIPv6.csv.zip)

The geolocation database is provided as large CSV file with the following header fields:

+ `ipVersion` - Either `ipv4` or `ipv6`. Determines the IP type of the network. Example: `"ipv4"`
+ `network` - The IP network to which the geolocation information applies. The format for the network is in free form and can be any network format (CIDR or `inetnum` / `NetRange`). Example: `"44.31.140.0/24"`
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

For offline ASN data access, the [**ASN Database**](https://ipapi.is/asn.html) is provided. The ASN database includes all assigned and allocated AS numbers by IANA and respective meta information.

The database is updated several times per week. For active ASN's (at least one route/prefix assigned to the AS), the database includes rich meta information. For example, the provided information for the ASN `50673` would be:

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

And parse with nodejs:

```JavaScript
let asnDatabase = require('./fullASN.json');
for (let asn in asnDatabase) {
  console.log(asn, asnDatabase[asn]);
}
```

### Hosting IP Ranges Database

Furthermore, the **Hosting IP ranges Database** is provided for offline and scalable access. This database contains all known datacenter IP ranges of the Internet. A [proprietary algorithm](https://ipapi.is/blog/detecting-hosting-providers.html) was developed to determine if a network belongs to a hosting provider.

The file format of the database is tab separated text file (.tsv), where each line of the file contains the `company`, `network` and `domain` of the hosting provider.

Example excerpt of the database:

```text
Linode, LLC 178.79.160.0 - 178.79.167.255 www.linode.com
OVH Sp. z o. o. 178.32.191.0 - 178.32.191.127 www.ovh.com
myLoc managed IT AG 46.245.176.0 - 46.245.183.255 www.myloc.de
```

[Download the Hosting IP Ranges Database](https://ipapi.is/hosting-detection.html)

### How to download & parse the Hosting IP Ranges database?

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
