# Configure Modular Input

The TA-opnsense modular input will interact with the OPNsense API using GET requests. This allows for system information to be returned to enrich the already ingested Splunk data. For more information on what data is collected by this modular input see [Reference: Modular Input](../../../reference/reference-mod-input/) in this documentation.

## Add-on Prerequisites

The OPNsense Add-on for Splunk must be set to visible in order to configure the modular input.

1. In Splunk web, navigate to the "Manage Apps" view by clicking the gear icon on the Launcher page or click "Manage Apps" from the "Apps" dropdown next to the Splunk logo on the top left of the screen.
1. In the app list search for "OPNsense Add-on" and click "Edit properties" on the right side.
1. Ensure "Visible" is set to Yes and save.

## Setup Modular Input

???+ info "Tested Versions"
    OPNsense **v21.1**
    
    Add-on Version **1.4.0**


### Modular Input Prerequisites

* Obtain [API Credentials](#obtain-api-credentials).
* FQDN/IP of the OPNsense instance (multiple instances may be setup through the interface).
* (Recommended) [CA Certificate for OPNsense instance](#obtain-ca-certificate).
* Splunk must be able to communicate to the firewall directly via port 443/tcp or through a proxy.

#### Obtain API Credentials

1. Log in to the OPNsense web interface. 
1. Navigate to System > Access > Users
1. create a new user or edit an existing user. At the `API keys` section, click the "+" to create new API credentials. This downloads an "apikey.txt" file containing the credentials for the API. These will be needed for later steps.

#### Obtain CA Certificate

1. Log in to the OPNsense web interface.
1. Navigate to System > Trust > Authorities. 
1. Export the CA cert of the Authority being used for the web interface.
1. Place the CA Certificate into `$SPLUNK_HOME/etc/auth`. It may be easier to create a new directory to keep this certificate separate <small>(i.e. `$SPLUNK_HOME/etc/auth/opnsense_certs`)</small>.

### Create Account

At least one account is needed for the modular input to work.

1. Verify Prerequisites have been completed before proceeding. 
1. Log in to the Splunk web interface.
1. Navigate to the OPNsense Add-on for Splunk > Configuration (Tab).

    ??? question "Not seeing the OPNsense Add-on?" 
        Verify the [Add-on Prerequistes](#add-on-prerequisites) have been completed.

1. Add a new Account.
1. Enter a name for the account.
1. Enter the [API credentials](#obtain-api-credentials) previously created.
1. Enter the IP/FQDN of the OPNsense instance.
1. <small>(Optional)</small> Enter the certificate path relative to `$SPLUNK_HOME/etc/auth`. Example value: (`opnsense_certs/OPNsense.crt`).
1. Uncheck the box if you are not using a certificate to verify.
1. Click add.
1. <small>_(optional)_</small> Configure proxy
1. <small>_(optional)_</small> Set logging level

### Create Input

1. Navigate to the Input tab.
1. Click "Create New Input"
1. For the selected input, enter a unique name, index, and an interval to run in seconds. <small>_The Default is 43200 seconds (12 hours)_</small>.
1. Select the correct account credentials for the input.
6. Click add.

### Verify 

Once completed the modular input will immediately run. To verify open up a search and run a similar query:

```shell
index=<your index> sourcetype=opnsense:system
```

If data does not appear within a few minutes, see [Troubleshooting Modular Inputs](../troubleshooting/troubleshoot-modinputs.md).

--8<-- "includes/abbreviations.md"