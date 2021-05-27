# Changelog

## OPNsense Add-on for Splunk

### v1.4.0 <small>Dec 14, 2020</small>

* Added modular input to pull system information (Available Updates, Versions, Installed Packages/Plugins).

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