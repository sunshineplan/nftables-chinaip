# Nftables chinaip script

Python script that generates .nft files with mappings between IP addresses
and its geolocation, so you can include them inside your rules.

# Requirements

Package used that are not present in the Python Standard Library

- `requests`

# Usage example

To generate ipv4 and ipv6 mappings, download geoip data from db-ip.com
saving the output in the current folder

```
   ./nft_geoip.py --download
```

Giving the following output

```
  drwxr-xr-x 2 foobar foobar 4,0K ene  4 19:38 .
  drwxr-xr-x 5 foobar foobar 4,0K ene  4 19:38 ..
  -rw-r--r-- 1 foobar foobar  22M ene  4 19:38 dbip.csv
  -rw-r--r-- 1 foobar foobar 203K ene  4 19:38 chinaip-ipv4.nft
```

* chinaip-ipv4.nft defines chinaip4 map (@chinaip4)
* China iso numeric value is 156

## Marking packets to its corresponding country

```
  meta mark set ip saddr map @chinaip4
```

## Example: Counting incoming China traffic

```
  table filter {
    include "./chinaip-ipv4.nft"

    chain input {
                  type filter hook input priority 0;
                  meta mark set ip saddr map @chinaip4
                  meta mark 156 counter
          }
  }
```
