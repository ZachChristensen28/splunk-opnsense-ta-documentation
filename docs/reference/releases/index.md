# Changelog

## OPNsense Add-on for Splunk

### v1.4.2 <small>June 2, 2021</small>

* Adding support for absolute paths in modular input setup for certificates - [#44](https://github.com/ZachChristensen28/TA-opnsense/issues/44)
* Fixed issue with the Verify Certificate checkbox not working properly - [#47](https://github.com/ZachChristensen28/TA-opnsense/issues/47)

### v1.4.1 <small>May 27, 2021</small>

* Fixed incorrect sourcetype transform for modular input - issue [#41](https://github.com/ZachChristensen28/TA-opnsense/issues/41)
* Increased the truncate limit to allow large events.

### v1.4.0 <small>May 27, 2021</small>

* Added modular input to pull system information (Available Updates, Versions, Installed Packages/Plugins).
* Updated the suricata sourcetyper to recognize the json data without the standard syslog message header.
* Fixed ipv6 ICMP events not extracting properly - issue [#37](https://github.com/ZachChristensen28/TA-opnsense/issues/37)

### v1.3.2 <small>Dec 14, 2020</small>

* Added meta field for event length (opnsense_event_length).
* Added sourcetype for Syslog-ng logs (opnsense:syslog).
* Added action for "Redirect" if port forwarding logging rules exist.
* Fixed "unknown" severity for opnsense:suricata:json events - issue [#27](https://github.com/ZachChristensen28/TA-opnsense/issues/27).
* Fixed IGMP events not being extracted - issue [#32](https://github.com/ZachChristensen28/TA-opnsense/issues/32).
* Fixed Access logs not being extracted - issue [#35](https://github.com/ZachChristensen28/TA-opnsense/issues/35).

### v1.3.1 <small>Oct 31, 2020</small>

* fixed KV_MODE for opnsense:unbound sourcetype.

### v1.3.0 <small>Aug 15, 2020</small>

* Added compatibility for eve syslog format for Suricata events.
* Removed incorrect field extraction for DHCP events.

### v1.2.9 <small>Aug 5, 2020</small>

* Added compatibility for new syslog format released in OPNSense v20.7.
* Updated the 'vendor_options' field to be multi-valued.
* Appinspect fixes.

### v1.2.7 <small>Jul 15, 2020</small>

* Removed Dependency for CIM app.
* Fixed multiple regex statements under one stanza.

--8<-- "includes/abbreviations.md"
