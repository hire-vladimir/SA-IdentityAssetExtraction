# SA-IdentityExtraction
Project found at https://github.com/hire-vladimir/SA-IdentityExtraction

# Introduction
The Splunk *SA-IdentityExtraction* add-on is used in conjunction with the *SA-ldapsearch* to populate asset and identity information from Active Directory. The asset and identity information within this app is by default integrated with Enterprise Security to enrich and correlate events with customer-defined information.

## Tested with
* Splunk 6.2.3 and Splunk 6.3.0
* SA-ldapsearch 2.1.0
* Enterprise Security 3.3.0

# Assumptions and pre-requisites
1. **SA-ldapsearch** app is installed. The application can be installed from here: https://splunkbase.splunk.com/app/1151/ Documentation: http://docs.splunk.com/Documentation/SA-LdapSearch/latest/User/AbouttheSplunkSupportingAdd-onforActiveDirectory
2. **SA-ldapsearch** app is configured with *default* domain name configuration. Note, the scheduled searches assume *default* domain is configured, search tuning will be required for different names.


# Installation
To install the *SA-IdentityExtraction* app you can either unpack the package under `$SPLUNK_HOME/etc/apps` or install via Manage Apps -> Install app from file from Splunk. The application will not require Splunk restart, if installed via UI.

By default, scheduled searches that generate asset and identity data are **disabled**, they must be enabled after review to ensure they fit into your environment.

## Customization
Every organization/environment is different, and therefore you will need to adjust the priority, category, and any additional fields to satisfy your requirements. Several `eval` and `case` examples have been included in each of the searches to get you started.

# Components and Usage
The *SA-IdentityExtraction* add-on consists of several settings and knowledge objects.

## Saved Searches
SA leverages scheduled searches to continuously build refresh asset and identity data. Searches output all fields required by Enterprise Security asset and identity lookups. **Note**, search scheduled times can be modified based on the desired frequency.

1. **ldap_assets** - Populates asset information from AD and runs every day at 02:00 AM. Generates `$SPLUNK_HOME/etc/apps/SA-IdentityExtraction/lookups/ldap_assets.csv`
2. **ldap_identities** - Populates identity information from AD and runs every day at 12:00 AM. Generates `$SPLUNK_HOME/etc/apps/SA-IdentityExtraction/lookups/ldap_identities.csv`
3. **splunk_deployment_server_assets** - Populates and merges information from Splunk Deployment Server logs into an asset lookup. This search runs everyday for the last 24 hours at 03:00 AM. Generates `$SPLUNK_HOME/etc/apps/SA-IdentityExtraction/lookups/splunk_deployment_server_assets.csv.csv`

## Inputs
There are three inputs that are used to perform identity and asset merge functionality within Enterprise Security, they are located under `$SPLUNK_HOME/etc/apps/SA-IdentityExtraction/default`
1. [identity_manager://ldap_identities]
2. [identity_manager://ldap_assets]
3. [identity_manager://splunk_deployment_server_assets]

## Transforms
There are three lookup definition stanzas found under `$SPLUNK_HOME/etc/apps/SA-IdentityExtraction/default`
1. [ldap_identities]
2. [ldap_assets]
3. [splunk_deployment_server_assets]

# Troubleshooting
1. I am not using Splunk app for Enterprise Security (ES), and seeing errors related to `identity_manager` on startup, such as listed below. *SA-IdentityExtraction* is developed to work with ES, and as such requires special components. To use this SA without ES, simply rename `$SPLUNK_HOME/etc/apps/SA-IdentityExtraction/default/inputs.conf` to `$SPLUNK_HOME/etc/apps/SA-IdentityExtraction/default/inputs.conf.disabled`

    > Invalid key in stanza [identity_manager://ldap_identities] in /Applications/Splunk/etc/apps/SA-IdentityExtraction/default/inputs.conf, line 5: category  (value:  ldap_identities)
Invalid key in stanza [identity_manager://ldap_identities] in /Applications/Splunk/etc/apps/SA-IdentityExtraction/default/inputs.conf, line 6: description  (value:  List of identities pulled from the SA-ldapsearch)
Invalid key in stanza [identity_manager://ldap_identities] in /Applications/Splunk/etc/apps/SA-IdentityExtraction/default/inputs.conf, line 7: target  (value:  identity)
Invalid key in stanza [identity_manager://ldap_identities] in /Applications/Splunk/etc/apps/SA-IdentityExtraction/default/inputs.conf, line 8: url  (value:  lookup://ldap_identities)


# Additional Resources
Additional documentation discussing ES assets and identities can be found at http://docs.splunk.com/Documentation/ES/latest/Install/IdentityManager

# Credits
Big thanks to the following individuals who helped contribute to this effort:
* Aaron Kornhouser

# Legal
* *Splunk* is a registered trademark of Splunk, Inc.
