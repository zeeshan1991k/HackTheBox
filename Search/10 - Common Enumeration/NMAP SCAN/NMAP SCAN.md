# NMAP SCAN
## FULL TCP NMAP
```bash
# Nmap 7.92 scan initiated Wed Feb 23 18:37:01 2022 as: nmap -vv --reason -Pn -T4 -sV -sC --version-all -A --osscan-guess -p- -oN /home/kali/Dropbox/Documents/htb/boxes/search/results/10.10.11.129/scans/_full_tcp_nmap.txt -oX /home/kali/Dropbox/Documents/htb/boxes/search/results/10.10.11.129/scans/xml/_full_tcp_nmap.xml 10.10.11.129
Nmap scan report for search.htb (10.10.11.129)
Host is up, received user-set (0.11s latency).
Scanned at 2022-02-23 18:37:03 PKT for 269s
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Search &mdash; Just Testing IIS
|_http-server-header: Microsoft-IIS/10.0
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2022-02-23 13:39:42Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: search.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2022-02-23T13:41:23+00:00; -8s from scanner time.
| ssl-cert: Subject: commonName=research
| Issuer: commonName=search-RESEARCH-CA/domainComponent=search
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-08-11T08:13:35
| Not valid after:  2030-08-09T08:13:35
| MD5:   0738 614f 7bc0 29d0 6d1d 9ea6 3cdb d99e
| SHA-1: 10ae 5494 29d6 1e44 276f b8a2 24ca fde9 de93 af78
| -----BEGIN CERTIFICATE-----
| MIIFZzCCBE+gAwIBAgITVAAAABRx/RXdaDt/5wAAAAAAFDANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VhcmNo
| MRswGQYDVQQDExJzZWFyY2gtUkVTRUFSQ0gtQ0EwHhcNMjAwODExMDgxMzM1WhcN
| MzAwODA5MDgxMzM1WjAxMRwwGgYDVQQDExNyZXNlYXJjaC5zZWFyY2guaHRiMREw
| DwYDVQQDEwhyZXNlYXJjaDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
| AJryZQO0w3Fil8haWl73Hh2HNnwxC3RxcPGE3QrXLglc2zwp1AsHLAKhUOuAq/Js
| OMyVBQZo13cmRh8l7XOcSXUI4YV/ezXr7GbznlN9NTGooZkzYuMBa21afqTjBgPk
| VYByyfYcECv8TvKI7uc78TpkwpZfmAKi6ha/7o8A1rCSipDvp5wtChLsDK9bsEfl
| nlQbMR8SBQFrWWjXIvCGH2KNkOI56Xz9HV9F2JGwJZNWrHml7BuK18g9sMs0/p7G
| BZxaQLW18zOQnKt3lNo97ovV7A2JljEkknR4MckN4tAEDmOFLvTcdAQ6Y3THvvcr
| UMg24FrX1i8J5WKfjjRdhvkCAwEAAaOCAl0wggJZMDwGCSsGAQQBgjcVBwQvMC0G
| JSsGAQQBgjcVCIqrSYT8vHWlnxuHg8xchZLMMYFpgcOKV4GUuG0CAWQCAQUwEwYD
| VR0lBAwwCgYIKwYBBQUHAwEwDgYDVR0PAQH/BAQDAgWgMBsGCSsGAQQBgjcVCgQO
| MAwwCgYIKwYBBQUHAwEwHQYDVR0OBBYEFFX1E0g3TlBigM7mdF25TuT8fM/dMB8G
| A1UdIwQYMBaAFGqRrXsob7VIpls4zrxiql/nV+xQMIHQBgNVHR8EgcgwgcUwgcKg
| gb+ggbyGgblsZGFwOi8vL0NOPXNlYXJjaC1SRVNFQVJDSC1DQSxDTj1SZXNlYXJj
| aCxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMs
| Q049Q29uZmlndXJhdGlvbixEQz1zZWFyY2gsREM9aHRiP2NlcnRpZmljYXRlUmV2
| b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0cmlidXRpb25Qb2lu
| dDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAChoGjbGRhcDovLy9DTj1z
| ZWFyY2gtUkVTRUFSQ0gtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VhcmNoLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTANBgkqhkiG9w0BAQsFAAOCAQEAOkRDrr85ypJJcgefRXJMcVduM0xK
| JT1TzlSgPMw6koXP0a8uR+nLM6dUyU8jfwy5nZDz1SGoOo3X42MTAr6gFomNCj3a
| FgVpTZq90yqTNJEJF9KosUDd47hsBPhw2uu0f4k0UQa/b/+C0Zh5PlBWeoYLSru+
| JcPAWC1o0tQ3MKGogFIGuXYcGcdysM1U+Ho5exQDMTKEiMbSvP9WV52tEnjAvmEe
| 7/lPqiPHGIs7mRW/zXRMq7yDulWUdzAcxZxYzqHQ4k5bQnuVkGEw0d1dcFsoGEKj
| 7pdPzYPnCzHLoO/BDAKJvOrYfI4BPNn2JDBs46CkUwygpiJpL7zIYvCUDQ==
|_-----END CERTIFICATE-----
443/tcp   open  ssl/http      syn-ack ttl 127 Microsoft IIS httpd 10.0
| tls-alpn: 
|_  http/1.1
|_ssl-date: 2022-02-23T13:41:23+00:00; -8s from scanner time.
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
| ssl-cert: Subject: commonName=research
| Issuer: commonName=search-RESEARCH-CA/domainComponent=search
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-08-11T08:13:35
| Not valid after:  2030-08-09T08:13:35
| MD5:   0738 614f 7bc0 29d0 6d1d 9ea6 3cdb d99e
| SHA-1: 10ae 5494 29d6 1e44 276f b8a2 24ca fde9 de93 af78
| -----BEGIN CERTIFICATE-----
| MIIFZzCCBE+gAwIBAgITVAAAABRx/RXdaDt/5wAAAAAAFDANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VhcmNo
| MRswGQYDVQQDExJzZWFyY2gtUkVTRUFSQ0gtQ0EwHhcNMjAwODExMDgxMzM1WhcN
| MzAwODA5MDgxMzM1WjAxMRwwGgYDVQQDExNyZXNlYXJjaC5zZWFyY2guaHRiMREw
| DwYDVQQDEwhyZXNlYXJjaDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
| AJryZQO0w3Fil8haWl73Hh2HNnwxC3RxcPGE3QrXLglc2zwp1AsHLAKhUOuAq/Js
| OMyVBQZo13cmRh8l7XOcSXUI4YV/ezXr7GbznlN9NTGooZkzYuMBa21afqTjBgPk
| VYByyfYcECv8TvKI7uc78TpkwpZfmAKi6ha/7o8A1rCSipDvp5wtChLsDK9bsEfl
| nlQbMR8SBQFrWWjXIvCGH2KNkOI56Xz9HV9F2JGwJZNWrHml7BuK18g9sMs0/p7G
| BZxaQLW18zOQnKt3lNo97ovV7A2JljEkknR4MckN4tAEDmOFLvTcdAQ6Y3THvvcr
| UMg24FrX1i8J5WKfjjRdhvkCAwEAAaOCAl0wggJZMDwGCSsGAQQBgjcVBwQvMC0G
| JSsGAQQBgjcVCIqrSYT8vHWlnxuHg8xchZLMMYFpgcOKV4GUuG0CAWQCAQUwEwYD
| VR0lBAwwCgYIKwYBBQUHAwEwDgYDVR0PAQH/BAQDAgWgMBsGCSsGAQQBgjcVCgQO
| MAwwCgYIKwYBBQUHAwEwHQYDVR0OBBYEFFX1E0g3TlBigM7mdF25TuT8fM/dMB8G
| A1UdIwQYMBaAFGqRrXsob7VIpls4zrxiql/nV+xQMIHQBgNVHR8EgcgwgcUwgcKg
| gb+ggbyGgblsZGFwOi8vL0NOPXNlYXJjaC1SRVNFQVJDSC1DQSxDTj1SZXNlYXJj
| aCxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMs
| Q049Q29uZmlndXJhdGlvbixEQz1zZWFyY2gsREM9aHRiP2NlcnRpZmljYXRlUmV2
| b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0cmlidXRpb25Qb2lu
| dDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAChoGjbGRhcDovLy9DTj1z
| ZWFyY2gtUkVTRUFSQ0gtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VhcmNoLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTANBgkqhkiG9w0BAQsFAAOCAQEAOkRDrr85ypJJcgefRXJMcVduM0xK
| JT1TzlSgPMw6koXP0a8uR+nLM6dUyU8jfwy5nZDz1SGoOo3X42MTAr6gFomNCj3a
| FgVpTZq90yqTNJEJF9KosUDd47hsBPhw2uu0f4k0UQa/b/+C0Zh5PlBWeoYLSru+
| JcPAWC1o0tQ3MKGogFIGuXYcGcdysM1U+Ho5exQDMTKEiMbSvP9WV52tEnjAvmEe
| 7/lPqiPHGIs7mRW/zXRMq7yDulWUdzAcxZxYzqHQ4k5bQnuVkGEw0d1dcFsoGEKj
| 7pdPzYPnCzHLoO/BDAKJvOrYfI4BPNn2JDBs46CkUwygpiJpL7zIYvCUDQ==
|_-----END CERTIFICATE-----
|_http-title: Search &mdash; Just Testing IIS
|_http-server-header: Microsoft-IIS/10.0
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: search.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2022-02-23T13:41:23+00:00; -8s from scanner time.
| ssl-cert: Subject: commonName=research
| Issuer: commonName=search-RESEARCH-CA/domainComponent=search
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-08-11T08:13:35
| Not valid after:  2030-08-09T08:13:35
| MD5:   0738 614f 7bc0 29d0 6d1d 9ea6 3cdb d99e
| SHA-1: 10ae 5494 29d6 1e44 276f b8a2 24ca fde9 de93 af78
| -----BEGIN CERTIFICATE-----
| MIIFZzCCBE+gAwIBAgITVAAAABRx/RXdaDt/5wAAAAAAFDANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VhcmNo
| MRswGQYDVQQDExJzZWFyY2gtUkVTRUFSQ0gtQ0EwHhcNMjAwODExMDgxMzM1WhcN
| MzAwODA5MDgxMzM1WjAxMRwwGgYDVQQDExNyZXNlYXJjaC5zZWFyY2guaHRiMREw
| DwYDVQQDEwhyZXNlYXJjaDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
| AJryZQO0w3Fil8haWl73Hh2HNnwxC3RxcPGE3QrXLglc2zwp1AsHLAKhUOuAq/Js
| OMyVBQZo13cmRh8l7XOcSXUI4YV/ezXr7GbznlN9NTGooZkzYuMBa21afqTjBgPk
| VYByyfYcECv8TvKI7uc78TpkwpZfmAKi6ha/7o8A1rCSipDvp5wtChLsDK9bsEfl
| nlQbMR8SBQFrWWjXIvCGH2KNkOI56Xz9HV9F2JGwJZNWrHml7BuK18g9sMs0/p7G
| BZxaQLW18zOQnKt3lNo97ovV7A2JljEkknR4MckN4tAEDmOFLvTcdAQ6Y3THvvcr
| UMg24FrX1i8J5WKfjjRdhvkCAwEAAaOCAl0wggJZMDwGCSsGAQQBgjcVBwQvMC0G
| JSsGAQQBgjcVCIqrSYT8vHWlnxuHg8xchZLMMYFpgcOKV4GUuG0CAWQCAQUwEwYD
| VR0lBAwwCgYIKwYBBQUHAwEwDgYDVR0PAQH/BAQDAgWgMBsGCSsGAQQBgjcVCgQO
| MAwwCgYIKwYBBQUHAwEwHQYDVR0OBBYEFFX1E0g3TlBigM7mdF25TuT8fM/dMB8G
| A1UdIwQYMBaAFGqRrXsob7VIpls4zrxiql/nV+xQMIHQBgNVHR8EgcgwgcUwgcKg
| gb+ggbyGgblsZGFwOi8vL0NOPXNlYXJjaC1SRVNFQVJDSC1DQSxDTj1SZXNlYXJj
| aCxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMs
| Q049Q29uZmlndXJhdGlvbixEQz1zZWFyY2gsREM9aHRiP2NlcnRpZmljYXRlUmV2
| b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0cmlidXRpb25Qb2lu
| dDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAChoGjbGRhcDovLy9DTj1z
| ZWFyY2gtUkVTRUFSQ0gtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VhcmNoLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTANBgkqhkiG9w0BAQsFAAOCAQEAOkRDrr85ypJJcgefRXJMcVduM0xK
| JT1TzlSgPMw6koXP0a8uR+nLM6dUyU8jfwy5nZDz1SGoOo3X42MTAr6gFomNCj3a
| FgVpTZq90yqTNJEJF9KosUDd47hsBPhw2uu0f4k0UQa/b/+C0Zh5PlBWeoYLSru+
| JcPAWC1o0tQ3MKGogFIGuXYcGcdysM1U+Ho5exQDMTKEiMbSvP9WV52tEnjAvmEe
| 7/lPqiPHGIs7mRW/zXRMq7yDulWUdzAcxZxYzqHQ4k5bQnuVkGEw0d1dcFsoGEKj
| 7pdPzYPnCzHLoO/BDAKJvOrYfI4BPNn2JDBs46CkUwygpiJpL7zIYvCUDQ==
|_-----END CERTIFICATE-----
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: search.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2022-02-23T13:41:24+00:00; -7s from scanner time.
| ssl-cert: Subject: commonName=research
| Issuer: commonName=search-RESEARCH-CA/domainComponent=search
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-08-11T08:13:35
| Not valid after:  2030-08-09T08:13:35
| MD5:   0738 614f 7bc0 29d0 6d1d 9ea6 3cdb d99e
| SHA-1: 10ae 5494 29d6 1e44 276f b8a2 24ca fde9 de93 af78
| -----BEGIN CERTIFICATE-----
| MIIFZzCCBE+gAwIBAgITVAAAABRx/RXdaDt/5wAAAAAAFDANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VhcmNo
| MRswGQYDVQQDExJzZWFyY2gtUkVTRUFSQ0gtQ0EwHhcNMjAwODExMDgxMzM1WhcN
| MzAwODA5MDgxMzM1WjAxMRwwGgYDVQQDExNyZXNlYXJjaC5zZWFyY2guaHRiMREw
| DwYDVQQDEwhyZXNlYXJjaDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
| AJryZQO0w3Fil8haWl73Hh2HNnwxC3RxcPGE3QrXLglc2zwp1AsHLAKhUOuAq/Js
| OMyVBQZo13cmRh8l7XOcSXUI4YV/ezXr7GbznlN9NTGooZkzYuMBa21afqTjBgPk
| VYByyfYcECv8TvKI7uc78TpkwpZfmAKi6ha/7o8A1rCSipDvp5wtChLsDK9bsEfl
| nlQbMR8SBQFrWWjXIvCGH2KNkOI56Xz9HV9F2JGwJZNWrHml7BuK18g9sMs0/p7G
| BZxaQLW18zOQnKt3lNo97ovV7A2JljEkknR4MckN4tAEDmOFLvTcdAQ6Y3THvvcr
| UMg24FrX1i8J5WKfjjRdhvkCAwEAAaOCAl0wggJZMDwGCSsGAQQBgjcVBwQvMC0G
| JSsGAQQBgjcVCIqrSYT8vHWlnxuHg8xchZLMMYFpgcOKV4GUuG0CAWQCAQUwEwYD
| VR0lBAwwCgYIKwYBBQUHAwEwDgYDVR0PAQH/BAQDAgWgMBsGCSsGAQQBgjcVCgQO
| MAwwCgYIKwYBBQUHAwEwHQYDVR0OBBYEFFX1E0g3TlBigM7mdF25TuT8fM/dMB8G
| A1UdIwQYMBaAFGqRrXsob7VIpls4zrxiql/nV+xQMIHQBgNVHR8EgcgwgcUwgcKg
| gb+ggbyGgblsZGFwOi8vL0NOPXNlYXJjaC1SRVNFQVJDSC1DQSxDTj1SZXNlYXJj
| aCxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMs
| Q049Q29uZmlndXJhdGlvbixEQz1zZWFyY2gsREM9aHRiP2NlcnRpZmljYXRlUmV2
| b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0cmlidXRpb25Qb2lu
| dDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAChoGjbGRhcDovLy9DTj1z
| ZWFyY2gtUkVTRUFSQ0gtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VhcmNoLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTANBgkqhkiG9w0BAQsFAAOCAQEAOkRDrr85ypJJcgefRXJMcVduM0xK
| JT1TzlSgPMw6koXP0a8uR+nLM6dUyU8jfwy5nZDz1SGoOo3X42MTAr6gFomNCj3a
| FgVpTZq90yqTNJEJF9KosUDd47hsBPhw2uu0f4k0UQa/b/+C0Zh5PlBWeoYLSru+
| JcPAWC1o0tQ3MKGogFIGuXYcGcdysM1U+Ho5exQDMTKEiMbSvP9WV52tEnjAvmEe
| 7/lPqiPHGIs7mRW/zXRMq7yDulWUdzAcxZxYzqHQ4k5bQnuVkGEw0d1dcFsoGEKj
| 7pdPzYPnCzHLoO/BDAKJvOrYfI4BPNn2JDBs46CkUwygpiJpL7zIYvCUDQ==
|_-----END CERTIFICATE-----
3269/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: search.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=research
| Issuer: commonName=search-RESEARCH-CA/domainComponent=search
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-08-11T08:13:35
| Not valid after:  2030-08-09T08:13:35
| MD5:   0738 614f 7bc0 29d0 6d1d 9ea6 3cdb d99e
| SHA-1: 10ae 5494 29d6 1e44 276f b8a2 24ca fde9 de93 af78
| -----BEGIN CERTIFICATE-----
| MIIFZzCCBE+gAwIBAgITVAAAABRx/RXdaDt/5wAAAAAAFDANBgkqhkiG9w0BAQsF
| ADBKMRMwEQYKCZImiZPyLGQBGRYDaHRiMRYwFAYKCZImiZPyLGQBGRYGc2VhcmNo
| MRswGQYDVQQDExJzZWFyY2gtUkVTRUFSQ0gtQ0EwHhcNMjAwODExMDgxMzM1WhcN
| MzAwODA5MDgxMzM1WjAxMRwwGgYDVQQDExNyZXNlYXJjaC5zZWFyY2guaHRiMREw
| DwYDVQQDEwhyZXNlYXJjaDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
| AJryZQO0w3Fil8haWl73Hh2HNnwxC3RxcPGE3QrXLglc2zwp1AsHLAKhUOuAq/Js
| OMyVBQZo13cmRh8l7XOcSXUI4YV/ezXr7GbznlN9NTGooZkzYuMBa21afqTjBgPk
| VYByyfYcECv8TvKI7uc78TpkwpZfmAKi6ha/7o8A1rCSipDvp5wtChLsDK9bsEfl
| nlQbMR8SBQFrWWjXIvCGH2KNkOI56Xz9HV9F2JGwJZNWrHml7BuK18g9sMs0/p7G
| BZxaQLW18zOQnKt3lNo97ovV7A2JljEkknR4MckN4tAEDmOFLvTcdAQ6Y3THvvcr
| UMg24FrX1i8J5WKfjjRdhvkCAwEAAaOCAl0wggJZMDwGCSsGAQQBgjcVBwQvMC0G
| JSsGAQQBgjcVCIqrSYT8vHWlnxuHg8xchZLMMYFpgcOKV4GUuG0CAWQCAQUwEwYD
| VR0lBAwwCgYIKwYBBQUHAwEwDgYDVR0PAQH/BAQDAgWgMBsGCSsGAQQBgjcVCgQO
| MAwwCgYIKwYBBQUHAwEwHQYDVR0OBBYEFFX1E0g3TlBigM7mdF25TuT8fM/dMB8G
| A1UdIwQYMBaAFGqRrXsob7VIpls4zrxiql/nV+xQMIHQBgNVHR8EgcgwgcUwgcKg
| gb+ggbyGgblsZGFwOi8vL0NOPXNlYXJjaC1SRVNFQVJDSC1DQSxDTj1SZXNlYXJj
| aCxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMs
| Q049Q29uZmlndXJhdGlvbixEQz1zZWFyY2gsREM9aHRiP2NlcnRpZmljYXRlUmV2
| b2NhdGlvbkxpc3Q/YmFzZT9vYmplY3RDbGFzcz1jUkxEaXN0cmlidXRpb25Qb2lu
| dDCBwwYIKwYBBQUHAQEEgbYwgbMwgbAGCCsGAQUFBzAChoGjbGRhcDovLy9DTj1z
| ZWFyY2gtUkVTRUFSQ0gtQ0EsQ049QUlBLENOPVB1YmxpYyUyMEtleSUyMFNlcnZp
| Y2VzLENOPVNlcnZpY2VzLENOPUNvbmZpZ3VyYXRpb24sREM9c2VhcmNoLERDPWh0
| Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1
| dGhvcml0eTANBgkqhkiG9w0BAQsFAAOCAQEAOkRDrr85ypJJcgefRXJMcVduM0xK
| JT1TzlSgPMw6koXP0a8uR+nLM6dUyU8jfwy5nZDz1SGoOo3X42MTAr6gFomNCj3a
| FgVpTZq90yqTNJEJF9KosUDd47hsBPhw2uu0f4k0UQa/b/+C0Zh5PlBWeoYLSru+
| JcPAWC1o0tQ3MKGogFIGuXYcGcdysM1U+Ho5exQDMTKEiMbSvP9WV52tEnjAvmEe
| 7/lPqiPHGIs7mRW/zXRMq7yDulWUdzAcxZxYzqHQ4k5bQnuVkGEw0d1dcFsoGEKj
| 7pdPzYPnCzHLoO/BDAKJvOrYfI4BPNn2JDBs46CkUwygpiJpL7zIYvCUDQ==
|_-----END CERTIFICATE-----
|_ssl-date: 2022-02-23T13:41:23+00:00; -8s from scanner time.
8172/tcp  open  ssl/http      syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| ssl-cert: Subject: commonName=WMSvc-SHA2-RESEARCH
| Issuer: commonName=WMSvc-SHA2-RESEARCH
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-04-07T09:05:25
| Not valid after:  2030-04-05T09:05:25
| MD5:   eeb9 303e 6d46 bd8b 34a0 1ed6 0eb8 3287
| SHA-1: 1e06 9fd0 ef45 b051 78b2 c6bf 1bed 975e a87d 0458
| -----BEGIN CERTIFICATE-----
| MIIC7TCCAdWgAwIBAgIQcJlfxrPWrqJOzFjgO04PijANBgkqhkiG9w0BAQsFADAe
| MRwwGgYDVQQDExNXTVN2Yy1TSEEyLVJFU0VBUkNIMB4XDTIwMDQwNzA5MDUyNVoX
| DTMwMDQwNTA5MDUyNVowHjEcMBoGA1UEAxMTV01TdmMtU0hBMi1SRVNFQVJDSDCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALXrSYHlRsq+HX01zrmC6Ddi
| /+vL/iDzS9endY3CRjfTjBL85qwvU5dkS+cxYTIkDaK5M9eoLcaVSARIcyrGIGuq
| DwIFQuYuaoGeQgiaQCqU5vXgsZ/xE8DRmlnZ2DeiAcHhx72TOHoUoUP4q2EqRoVr
| q5RCBGITT7hdRQd0vuTIHoLxdO2U5wZVCoN5vsp0Du43/LCgXExUpcHAHu9aVAzt
| pXWFY8B3XEFZjafffOHXiK6C2UzX4DddYweKR+ItMfQzX8T2MbX1qVm7D526/gU9
| WRGa7F/tj8+qvzZc4SQZ6Td9PWpMKCPGqqYTGmHlEW8ZowGoMSH62QaCilFxckEC
| AwEAAaMnMCUwEwYDVR0lBAwwCgYIKwYBBQUHAwEwDgYDVR0PBAcDBQCwAAAAMA0G
| CSqGSIb3DQEBCwUAA4IBAQAlGUrh9gLK7Er/BzEjyWebPPf18m3XxgZ13iFllhJ0
| 5tBUb3hczHIr3VOj/OWUJygxw8O10OrBZJZf29TPZ2nXKGbJRpYe+baii49LsGjr
| DiOM5XVZv5qiPBNts7fKyhpzTy0DdnIKAXUIYy/7nQ6rHetXApz89ZEzU6vAN0g0
| Zxq/NolqIVnehFn/36tjc65v1wgo6KnHAQUt6zWufueeYS3k2f4JzvFn4aPtUYRi
| nQgTuGbJTlxdVJ5DJjld9pLyJ+OctGeI1jRITiYlu5p3JwhxU0+mQjGT5mQZ+Umu
| abMpffugMOPYnyHu8poRZWjKgNBN0ygmnqGbTjx57No5
|_-----END CERTIFICATE-----
|_ssl-date: 2022-02-23T13:41:23+00:00; -8s from scanner time.
|_http-title: Site doesn't have a title.
| tls-alpn: 
|_  http/1.1
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
49667/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49675/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49698/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49709/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49738/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
TCP/IP fingerprint:
SCAN(V=7.92%E=4%D=2/23%OT=53%CT=%CU=%PV=Y%DS=2%DC=T%G=N%TM=6216398C%P=x86_64-pc-linux-gnu)
SEQ(SP=107%GCD=1%ISR=109%TI=I%II=I%SS=S%TS=U)
OPS(O1=M505NW8NNS%O2=M505NW8NNS%O3=M505NW8%O4=M505NW8NNS%O5=M505NW8NNS%O6=M505NNS)
WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)
ECN(R=Y%DF=Y%TG=80%W=FFFF%O=M505NW8NNS%CC=Y%Q=)
T1(R=Y%DF=Y%TG=80%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=N)
U1(R=N)
IE(R=Y%DFI=N%TG=80%CD=Z)

Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=263 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: Host: RESEARCH; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -7s, deviation: 0s, median: -8s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2022-02-23T13:40:48
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 32134/tcp): CLEAN (Timeout)
|   Check 2 (port 19529/tcp): CLEAN (Timeout)
|   Check 3 (port 18790/udp): CLEAN (Timeout)
|   Check 4 (port 60521/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

TRACEROUTE (using port 445/tcp)
HOP RTT       ADDRESS
1   110.98 ms 10.10.14.1
2   111.05 ms search.htb (10.10.11.129)

Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Feb 23 18:41:32 2022 -- 1 IP address (1 host up) scanned in 272.06 seconds
```
