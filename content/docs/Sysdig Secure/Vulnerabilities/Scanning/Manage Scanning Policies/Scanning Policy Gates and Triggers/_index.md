---
linkTitle: "Scanning Policy Gates and Triggers"
title: "Scanning Policy Gates and Triggers"
weight: "11"
aliases:
  - /en/scanning-policy-gates-and-triggers.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

This document describes the gates (and their respective triggers /
parameters) that are supported within Sysdig Secure policy bundles. Use
these policy gates, triggers, and parameters to build in-depth scanning
policies, from whitelisting / blacklisting partial file names to
defining what login shells are approved.

{{% callout type="note" %}}
This information can also be obtained using the CLI:

```yaml
user@host:~$ sdc-cli policy describe (--gate <gatename> (--trigger <triggername))
```

{{% /callout %}}

For more information, see [Manage Scanning
Policies](/en/docs/sysdig-secure/vulnerabilities/scanning/manage-scanning-policies/#manage-scanning-policies) and [Dockerfile
Gate and Triggers Deep
Dive](/en/docs/sysdig-secure/vulnerabilities/scanning/manage-scanning-policies/dockerfile-gate-and-triggers-deep-dive/#dockerfile-gate-and-triggers-deep-dive).

## Always

This gate provides users with a valuable testing resource, as it will be
triggered unconditionally.

### always

The `always` trigger / gate will trip if it is present in the policy.

{{% callout type="note" %}}

The Always gate is useful for testing whether the image
blacklist/whitelist is working as expected.

{{% /callout %}}

<img src="/image/374670949.png" width="600" />

## Dockerfile

The `dockerfile` gate reviews the contents of a Dockerfile, or the
assumed contents of a dockerfile if one is not provided, for exposed
ports and instructions that do not follow best practices.

{{% callout type="note" %}}

The gate can assume what the contents would be based on the Docker layer
history. See also: [Dockerfile Gate and Triggers Deep
Dive](/en/docs/sysdig-secure/vulnerabilities/scanning/manage-scanning-policies/dockerfile-gate-and-triggers-deep-dive/#dockerfile-gate-and-triggers-deep-dive)

{{% /callout %}}

### effective\_user

This trigger reviews whether the effective user matches the user
provided, and will fire based on the configured type.

| Parameter | Description                                                       | Example     |
|-----------|-------------------------------------------------------------------|-------------|
| `type`    | Determines whether the user should be whitelisted or blacklisted. | N/A         |
| `user`    | The name of the user.                                             | root,docker |

<img src="/image/374670942.png" width="600" />

### exposed\_ports

This trigger evaluates the set of exposed ports to determine whether
they should be whitelisted or blacklisted.

| Parameter                | Description                                                                                                                                            | Example      |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| `actual_dockerfile_only` | Defines whether the evaluation should skip inferred or guessed Dockerfiles, and only evaluate user-provided Dockerfiles. The default value is `false`. | true         |
| `ports`                  | A comma-separated list of port numbers.                                                                                                                | 80,8080,8088 |
| `type`                   | Defines whether the ports should be whitelisted or blacklisted.                                                                                        | N/A          |

<img src="/image/374670935.png" width="600" height="250" />

### instruction

This trigger evaluates whether any directives/instructions in the list
match the conditions in the Dockerfile.

| Parameter                | Description                                                                                                                                            | Example |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| `actual_dockerfile_only` | Defines whether the evaluation should skip inferred or guessed dockerfiles, and only evaluate user-provided dockerfiles. The default value is `false`. | true    |
| `check`                  | The type of check to perform.                                                                                                                          | =       |
| `instruction`            | The `dockerfile` instruction to check.                                                                                                                 | FROM    |
| `value`                  | The value to check the `dockerfile` instruction against.                                                                                               | scratch |

<img src="/image/374670928.png" width="600" />

### no\_dockerfile\_provided

This trigger will trip if there is no Dockerfile supplied with the
image. No parameters are required for this trigger.

<img src="/image/374670921.png" width="600" />

## Files

The Files gate reviews files within the analyzed image. This evaluation
covers file content, names, and filesystem attributes.

### content\_regex\_match

This trigger is tripped for each file where a match has been found using
the configured regex in the `analyzer_config.yaml` `content_search`
section.

{{% callout type="note" %}}

For more information regarding the regex values, refer to the
`analyzer_config.yaml` file.

{{% /callout %}}

| Parameter    | Description                                                                       | Example        |
|--------------|-----------------------------------------------------------------------------------|----------------|
| `regex_name` | The regex string that appears in the `FILECHECK_CONTENTMATCH` analyzer parameter. | .\*password.\* |

<img src="/image/374670914.png" width="600" />

### name\_match

This trigger is tripped if the name of a file in the container matches
the provided regex.

{{% callout type="note" %}}

This trigger has a performance impact on policy evaluation.

{{% /callout %}}

| Parameter | Description              | Example   |
|-----------|--------------------------|-----------|
| `regex`   | The regex to search for. | .\*\\.pem |

<img src="/image/374670907.png" width="600" />

### suid\_or\_guid\_set

This trigger is tripped for each file that has a set-user identification
(SUID) or set-group identification (SGID) configured. No parameters are
required.

<img src="/image/374670900.png" width="600" />

## Licenses

This gate is used to review software licenses found in the container
image, to ensure, for example, that packages that violate internal
company policy are not being used.

### blacklist\_exact\_match

This trigger will be tripped if the image contains packages distributed
under the exact license specified.

| Parameter  | Description                                           | Example                    |
|------------|-------------------------------------------------------|----------------------------|
| `licenses` | A comma-separated list of license names to blacklist. | GPLv2+,GPL-3+,BSD-2-clause |

<img src="/image/374670893.png" width="600" />

### blacklist\_partial\_match

This trigger will be tripped if the image contains packages distributed
under a license that includes the partial strings provided.

| Parameter  | Description                                                  | Example  |
|------------|--------------------------------------------------------------|----------|
| `licenses` | A comma-separated list of strings to blacklist for licenses. | LGPL,BSD |

<img src="/image/374670886.png" width="600" />

## Metadata

This gate reviews image metadata, including the size, operating system,
and architecture.

### attribute

The attribute trigger is tripped if a named image metadata value matches
the given condition.

| Parameter   | Description                                  | Example    |
|-------------|----------------------------------------------|------------|
| `attribute` | The attribute name to check.                 | size       |
| `check`     | The operation to perform for the evaluation. | &gt;       |
| `value`     | The value used for the evaluation.           | 1073741824 |

<img src="/image/374670879.png" width="600" />

## Packages

The Packages gate reviews all packages within the image, verifying
names, versions, and whitelisted / blacklisted packages.

### blacklist

This trigger is tripped if the image contains packages that have been
blacklisted by either name, or name and version.

| Parameter | Description                                                  | Example        |
|-----------|--------------------------------------------------------------|----------------|
| `name`    | The name of blacklisted package/s.                           | openssh-server |
| `version` | The exact version of the package that should be blacklisted. | 1.0.1          |

<img src="/image/374670831.png" width="600" />

### required\_package

The `required_package` trigger is tripped if the specified package /
version is not found in the image.

| Parameter            | Description                                                                                                                                                                    | Example   |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|
| `name`               | The name of the required package.                                                                                                                                              | libssl    |
| `version`            | The required package version.                                                                                                                                                  | 1.10.3rc3 |
| `version_match_type` | Defines whether the trigger should require the exact package and version (exact), or just a version of the package (minimum). This is only relevant if the version is defined. | exact     |

<img src="/image/374670824.png" width="600" />

### verify

This trigger reviews the package integrity against the package database
in the image, and is tripped for change or removal of content in either
all or a defined list of directories provided.

| Parameter          | Description                                                                           | Example        |
|--------------------|---------------------------------------------------------------------------------------|----------------|
| `check`            | Defines whether the check should focus on missing packages, changed packages, or all. | changed        |
| `only_directories` | Defines the list of directories the check should be limited to.                       | /usr,/var/lib  |
| `only_packages`    | Defines the list of packages that should be verified.                                 | libssl,openssl |

<img src="/image/374670817.png" width="600" />

## Passwd File

This gate reviews `/etc/passwd` for blacklisted users, groups, and
shells.

### blacklist\_full\_entry

This trigger trips if the whole password is found in the `/etc/password`
file.

| Parameter | Description                               | Example                                     |
|-----------|-------------------------------------------|---------------------------------------------|
| `entry`   | The full entry to match in `/etc/passwd`. | ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin |

<img src="/image/374670991.png" width="600" />

### blacklist\_groupids

This trigger is tripped if the designated group id/s are found in the
`/etc/passwd` file.

| Parameter   | Description                                                                       | Example |
|-------------|-----------------------------------------------------------------------------------|---------|
| `group_ids` | A numeric, comma separated list of group ids that will cause the trigger to trip. | 999,20  |

<img src="/image/374670984.png" width="600" />

### blacklist\_shells

This trigger will trip if a designated login shell is found under any
user in the `/etc/passwd` file.

| Parameter | Description                              | Example            |
|-----------|------------------------------------------|--------------------|
| `shells`  | The list of shell commands to blacklist. | /bin/bash,/bin/zsh |

<img src="/image/374670977.png" width="600" />

### blacklist\_userids

This trigger will be tripped if the specified user ID is present in
/`etc/passwd`.

| Parameter  | Description                                                   | Example |
|------------|---------------------------------------------------------------|---------|
| `user_ids` | The numerical, comma-separated list of user IDs to blacklist. | 0,1     |

<img src="/image/374670970.png" width="600" />

### blacklist\_usernames

The `blacklist_usernames` trigger will trip if the specified username is
found in the `/etc/passwd` file.

| Parameter    | Description                                       | Example    |
|--------------|---------------------------------------------------|------------|
| `user_names` | A comma-separated list of usernames to blacklist. | daemon,ftp |

<img src="/image/374670963.png" width="600" />

### content\_not\_available

The `content_not_available` trigger will trip if the `/etc/passwd` file
is not present in the image. No parameters are required.

<img src="/image/374670956.png" width="600" />

## Secret Scans

Secret scans determine, based on configured regex, whether secrets that
could be available if an image was compromised have been baked into the
image.

### content\_regex\_checks

The `content_regex_checks` trigger trips if the content search analyzer
finds a match with the configured and named regexes. Matches are
filtered by the `content_regex_name`, and the `filename_regex`, if
either are set.

{{% callout type="note" %}}

The `content_regex_name` should be a value from the secret\_search
section of `analyzer_config.yaml`.

{{% /callout %}}

<table>
<thead>
<tr class="header">
<th><p>Parameter</p></th>
<th><p>Description</p></th>
<th><p>Example</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>content_regex_name</code></p></td>
<td><p>The name of the variable / content. If found in the image, this should trip the trigger.</p>
<div class="callout callout--note">
<p>The names available by default are <code>AWS_ACCESS_KEY</code>, <code>AWS_SECRET_KEY</code>, <code>PRIV_KEY</code>, <code>DOCKER_AUTH</code>, and <code>API_KEY</code>.</p>
</div></td>
<td><p>AWS_ACCESS_KEY</p></td>
</tr>
<tr class="even">
<td><p><code>filename_regex</code></p></td>
<td><p>Filters the files that should be analyzed for the presence of the <code>content_regex_name</code>.</p></td>
<td><p>/etc/.*</p></td>
</tr>
</tbody>
</table>

<img src="/image/374671033.png" width="600" />

## Vulnerabilities

CVE / vulnerability checks can be used to ensure the included packages
don't have vulnerabilities above a set level, are older than a
designated period, or if data is unavailable.

### package

The package trigger is tripped if a vulnerability in an image matches
the configured comparison criteria. The table below outlines the
available parameters and criteria:

| Parameter             | Description                                                                                                 | Example |
|-----------------------|-------------------------------------------------------------------------------------------------------------|---------|
| `fix_available`       | If present, the fix availability for the vulnerability record must match the value of the parameter.        | true    |
| `package_type`        | The specific type of package.                                                                               | all     |
| `severity`            | The vulnerability severity.                                                                                 | high    |
| `severity_comparison` | The type of comparison to perform for the security evaluation.                                              | &gt;    |
| `vendor_only`         | If true, an available fix for this CVE must not be explicitly marked as "Won't be addressed by the vendor". | true    |

<img src="/image/374671040.png" width="600" />

### stale\_feed\_data

The `stale_feed_data` trigger will be tripped if the CVE data is older
than the window specified.

| Parameter             | Description                                                                | Example |
|-----------------------|----------------------------------------------------------------------------|---------|
| `max_days_since_sync` | Determines how old in days sync data can be before the trigger is tripped. | 10      |

<img src="/image/374671047.png" width="600" />

### vulnerability\_data\_unavailable

If no vulnerability data is available, the
`vulnerability_data_unavailable` trigger will trip. No parameters are
required for this trigger.

<img src="/image/374671054.png" width="600" height="175" />
