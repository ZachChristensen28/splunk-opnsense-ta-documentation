# Syslog Guide

Sending data to a syslog server where a Splunk forwarder is monitoring the files can improve the data collection to Splunk. A Splunk forwarder automatically load-balances data to all indexers in a distributed environment and can be setup with TLS and other setting to ensure the data transmission is secure and reliable. Utilizing a syslog server will also help prevent gaps in data when Splunk needs to restart for maintenance or unplanned shutdowns. For more information see [Splunk answers](https://community.splunk.com/t5/Getting-Data-In/universal-forwarder-vs-dedicated-rsyslog-syslog-ng-servers-to/td-p/54911) and [Splunk Docs: Forwarder Manual](https://docs.splunk.com/Documentation/Forwarder/latest/Forwarder/Abouttheuniversalforwarder). More information on syslog and Splunk, see the (SYSLOG) Syslog Data Collection section of the [Splunk Validated Architectures](https://www.splunk.com/pdfs/technical-briefs/splunk-validated-architectures.pdf) white paper.

The following will walk through an example of setting up syslog using Rsyslog. Syslog-ng example can be found at the below blog posts:

* [High Performance Syslog - Part 1](https://www.splunk.com/en_us/blog/tips-and-tricks/high-performance-syslogging-for-splunk-using-syslog-ng-part-1.html)
* [High Performance Syslog - Part 2](https://www.splunk.com/en_us/blog/tips-and-tricks/high-performance-syslogging-for-splunk-using-syslog-ng-part-2.html)
* [Using Syslong-ng with Splunk](https://www.splunk.com/en_us/blog/tips-and-tricks/using-syslog-ng-with-splunk.html)

## Data on-boarding using Rsyslog

[Rsyslog](https://www.rsyslog.com) is a default package on most linux distros. The OPNsense firewall can be setup to send logs via syslog to a configured Rsyslog server for a Splunk Forwarder to monitor. Below is a basic configuration to get started with data on-boarding. 

!!! note
    The following does not reflect rsyslog best practices but could be used as a starting point.

### Rsyslog Basic Configuration

!!! note ""
    Tested with Rsyslog version: rsyslogd 8.32.0 on RHEL/CentOS/Ubuntu

The default rsyslog configuration file is located in `/etc/rsyslog.conf`.

Open the rsyslog configuration file with your favorite text editor. Place the following in the configuration file or uncomment if already present:

```shell
# /etc/rsyslog.conf

# Load Modules
module(load="imudp") # provides UDP syslog reception

# OMFILE Global Permissions
module(load="builtin:omfile" dirCreateMode="0750" dirOwner="root" dirGroup="splunk" fileCreateMode="0640" fileOwner="root" fileGroup="splunk")

# DynaFile Template
template(name="t_opnsense" type="string" string="/var/log/remote/opnsense/%hostname%/opnsense.log")

# ruleset
ruleset(name="r_opnsense" queue.type="linkedlist"){
  action(type="omfile" dynaFile="t_opnsense")
}

# Use port 514 for OPNsense data
input(type="imudp" port="514" ruleset="r_opnsense")
```

Once the above has been configured, Save & Close the file. Then restart the rsyslog process:

`systemctl restart rsyslog`

Verify rsyslog is running with: `service rsyslog status`

The above will create a new file in `/var/log/remote` which a Splunk Forwarder can monitor and forward to the Splunk Indexers.

### Troubleshooting

- Ensure firewall rules are configured to allow the rsyslog listening port (514/udp)
- Ensure SELinux is not blocking rsyslog from writing to the file. This may occur if you write data outside of `/var/log`.

## Basic Log Rotation Strategy for syslog server

It is necessary to rotate the logs to ensure the disk space does not fill up. Below is a basic example to get started with log rotation.

The following uses the builtin package `logrotate`. The configuration file can be found at `/etc/logrotate.conf`

Start by creating a new file in `/etc/logrotate.d`. For this example, we will use a created file in `/etc/logrotate.d/opnsense`

Add the following to the file.

```SHELL
/var/log/remote/opnsense/*/*.log
{
        rotate 7
        daily
        missingok
        create 0640 root splunk
        notifempty
        compress
        dateext
        dateformat -%Y-%m-%d
        dateyesterday
        sharedscripts
        postrotate
                /bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
        endscript
}
```

The above may not work for every situation. Please check the man pages for logroate if experiencing issues. It may also be helpful to look at existing configuration files located in `/etc/logrotate.d` for reference.

## OPNsense Syslog Configuration

The following is necessary to now send syslog data from the OPNsense firewall to the newly configured syslog server.

Administrator access to the OPNsense Web GUI will be required to perform the following steps.

1. Log into the OPNsense firewall.
1. Navigate to: System > Settings > Logging/targets.
1. Click the `+` (plus sign) to add a new syslog destination.
  - Ensure the `Enabled` checkbox is checked.
  - Transport = UDP(4)
  - Applications = (Leave Blank) to select everything
  - Levels = (Leave default setting)
  - Facilities = (Leave Blank) to select everything
  - Hostname = FQDN or IP of the syslog server configured in previous steps.
  - Port = 514
  - Description = <small>(Optional)</small>
1. Click Save
1. Click Apply

--8<-- "includes/abbreviations.md"