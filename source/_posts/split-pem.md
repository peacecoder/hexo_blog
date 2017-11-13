---
title: split_pem
date: 2017-09-15 12:48:20
tags: awk, pem
---


This script use demo how to spilt pem into key and certificate by awk script

```awk
BEGIN {
    split(ARGV[1], a, ".");
    fname=a[1] ".key"
    rm_cmd = "rm -f " a[1] ".key"
    system(rm_cmd)
    rm_cmd="rm -f " a[1] ".crt"
    system(rm_cmd)
}
{
    print $0 >> fname
    if($0=="-----END PRIVATE KEY-----") {
        fname=a[1] ".crt"
    } else if ($0=="-----END CERTIFICATE-----") {
        exit 0
    }
}
```

Usage:

awk -f split_pem.awk $file_name
