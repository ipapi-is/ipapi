# ipapi.is

This repository contains the free databases that are used by the commercial IP API <https://ipapi.is>

## ASN Database

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

**How to download & parse the ASN database?**

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

**How to download & parse the Datacenter database?**

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
