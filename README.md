# apache-directory-listing
Lab demonstrating Apache default directory listing and potential exposure of sensitive files
# Apache Directory Listing

## Overview
This lab demonstrates how Apache’s default configuration enables directory listing, and how this can expose sensitive files placed under the web root. While directory listing itself is default behavior, the presence of sensitive files makes this a security concern.

## Environment
- Ubuntu Server
- Apache2
- Publicly accessible HTTP service

## Proof of Concept

### Baseline
The Apache default page was accessible over HTTP, confirming the service was publicly reachable.

![Apache default page](screenshots/01-default-page.png)

### Directory Listing Exposure
Navigating directly to a directory without an index file resulted in a directory listing, exposing sensitive files intentionally placed for this lab.

![Directory listing](screenshots/02-directory-listing.png)

**Note:** Apache’s default configuration enables directory listing. The vulnerability arises from the combination of default listing and sensitive files present in the web root.

### Configuration
Directory indexing is controlled by the `Options Indexes` directive in Apache’s configuration.

![Apache config](screenshots/03-apache-indexes-config.png)

## Impact
An unauthenticated attacker could enumerate and download sensitive files such as database backups or user data, potentially leading to data exposure or further compromise.

---

## Remediation and Verification
To prevent directory listing, remove `Indexes` from Apache configuration for the relevant `<Directory>` block:

```apache
<Directory /var/www/>
    Options FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

