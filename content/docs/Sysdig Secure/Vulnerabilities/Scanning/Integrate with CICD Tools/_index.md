---
linkTitle: "Integrate with CI/CD Tools"
title: "Integrate with CI/CD Tools"
weight: "10"
aliases:
  - /en/integrate-with-ci-cd-tools.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

You have the option to use image scanning as part of your development
pipeline, to check for best practices, vulnerabilities, and sensitive
content.

<div
id="UUID-8945ddee-8c45-58b4-7d85-e06c4235d03c_tip-idm53188840542430"
class="tip">

Review the [Types of Secure
Integrations](/en/docs/sysdig-secure/integrations-for-sysdig-secure/#integrations-for-sysdig-secure) table for more
context. The CI/CD Tools column lists the various options and their
levels of support.

</div>

## Inline Scanning

Sysdig provides a stand-alone inline scanner-- a containerized
application that can perform local analysis on container images (both
pulling from registries or locally built) and post the result of the
analysis to Sysdig Secure.

Other scanning integrations (i.e. the [Jenkins CI/CD
plugin](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/integrate-with-jenkins/#integrate-with-jenkins)) make use of this
component under the hood to provide local image analysis capabilities,
but it can also be used as a stand-alone component for custom pipelines,
or simply as a way to one-shot scan a container from any host.

The Sysdig inline scanner works as an independent container, without any
Docker dependency (it can be run using other container runtimes), and
can analyze images using different image formats and sources.

This feature has a variety of use cases and benefits:

- Images don't leave their own environment

- SaaS users don't send images and proprietary code to Sysdig's SaaS
    service

- Registries don't have to be exposed

- Images can be scanned in parallel more easily

- Images can be scanned before they hit the registry, which can

  - cut down on registry costs

  - simplify the build pipeline

### Prerequisites

At a minimum, the inline scanner requires:

- Sysdig Secure v2.5.0+ (with API token)

- Internet access to post results to Sysdig Secure (SaaS or On-Prem)

- Ability to run a container

{{% callout type="note" %}}
Using the `inline_script.sh` is deprecated. This script uses a different set of parameters; for more information about porting the parameters to
the inline scanner container, see [changes-from-v1xx](https://github.com/sysdiglabs/secure-inline-scan#changes-from-v1xx).
{{% /callout %}}

### Implement Inline Scanning

#### Quick Start

You can scan an image from any host by executing:

```yaml
docker run --rm quay.io/sysdig/secure-inline-scan:2 <image_name> --sysdig-token <my_API_token> --sysdig-url <secure_backend_endpoint>
…
…
Status is pass
View the full result @ https://secure.sysdig.com/#/scanning/scan-results/docker.io%2Falpine%3A3.12.1/sha256:c0e9560cda118f9ec63ddefb4a173a2b2a0347082d7dff7dc14272e7841a5b5a/summaries
PDF report of the scan results can be generated with -r option.
```

##### Upgrading

You can rerun this Docker command to upgrade to the latest inline scanning component at any time.

##### Common Parameters

<table>
<thead>
<tr class="header">
<th><p>Parameter</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Image name</strong> (mandatory)</p></td>
<td><p>Container image to be analyzed, following the usual <code>registry/repo:tag</code> format, i.e. <code></code> <code>docker.io/alpine:3.12.1</code>. If no tag is specified, the latest will be used.</p>
<p>Digest format is also supported, i.e.: <code>docker.io/alpine@sha256:c0e9560cda118f9ec6...</code></p></td>
</tr>
<tr class="even">
<td><p><strong>--sysdig-token</strong> (mandatory)</p></td>
<td><p>Sysdig API token, visible from the <a href="/en/docs/administration/administration-settings/user-profile-and-password/#user-profile-and-password">User Profile page</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>--sysdig-url:</strong></p></td>
<td><p>Not required for Sysdig Secure SaaS in the us-east region. For any other case, you must adjust this parameter. I.e. for SaaS us-west it is: <code>--sysdig-url https://us2.app.sysdig.com.</code> See also <a href="/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges">SaaS Regions and IP Ranges</a>.</p></td>
</tr>
</tbody>
</table>

###### Quick Help and Parameter List from -h

**Display a quick help and parameters description** from the image
itself by executing:
`docker run --rm quay.io/sysdig/secure-inline-scan:2 -h`.

**Sample output:**

```yaml
$ docker run quay.io/sysdig/secure-inline-scan:2 -h
Sysdig Inline Analyzer -- USAGE

  Container for performing analysis on local container images, utilizing the Sysdig analyzer subsystem.
  After image is analyzed, the resulting image archive is sent to a remote Sysdig installation
  using the -s <URL> option. This allows inline analysis data to be persisted & utilized for reporting.

  Usage: sysdig-inline-scan.sh -k <API Token> [ OPTIONS ] <FULL_IMAGE_TAG>

    == GLOBAL OPTIONS ==

    -k <TEXT>   [required] API token for Sysdig Scanning auth
                (ex: -k 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx')
                Alternatively, set environment variable SYSDIG_API_TOKEN
                Alias: --sysdig-token
    -s <URL>    [optional] Sysdig Secure URL (ex: -s 'https://secure-sysdig.svc.cluster.local').
                If not specified, it will default to Sysdig Secure SaaS URL (https://secure.sysdig.com).
                Alias: --sysdig-url
    --sysdig-skip-tls
                [optional] skip tls verification when calling secure endpoints
    -o          [optional] Use this flag if targeting onprem sysdig installation
                Alias: --on-prem
    -a <TEXT>   [optional] Add annotations (ex: -a 'key=value,key=value')
                Alias: --annotations
    -f <PATH>   [optional] Path to Dockerfile (ex: -f ./Dockerfile)
                Alias: --dockerfile
    -m <PATH>   [optional] Path to Docker image manifest (ex: -m ./manifest.json)
                Alias: --manifest
    -i <TEXT>   [optional] Specify image ID used within Sysdig (ex: -i '<64 hex characters>')
                Alias: --image-id
    -d <SHA256> [optional] Specify image digest (ex: -d 'sha256:<64 hex characters>')
                Alias: --digest
    -c          [optional] Remove the image from Sysdig Secure if the scan fails
    -r <PATH>   [optional] Download scan result pdf in a specified container-local directory (ex: -r /staging/reports)
                This directory needs to be previously mounted from the host to persist the data
                Alias: --report-folder
    -v          [optional] Increase verbosity
                Alias: --verbose
    --format <FORMAT>
                [optional] The only valid format is JSON. It sets the output format to a valid JSON which
                can be processed in an automated way.
    --write-json <PATH>
                Write the final JSON report to <PATH>.
    --time-profile
                Output information about the time elapsed in the different stages of the scan process
    --malware-scan-enable
                Enables malware scan on container.
                WARNING: it's generally a very slow process.
    --malware-scan-db-path <PATH>
                Local container path with updated ClamAV database.
                Will be used to call clamscan command as "clamscan --database=<PATH> ..."
    --malware-scan-output <DIR-PATH>
                Save JSON output of scan to path. Will be saved to <PATH>/malware_findings.json.
                Output is a JSON array of {"path": "...", "signature": "..."} objects.
                Note: path should exists and should be a directory.
    --malware-fail-fast true|false
                Fails immediately when a malware is found, skipping sending analysis
                results to Secure Backend.
                Default: true
    --malware-exclude REGEX
                Exclude dirs (and its content) and files which match the given regex.
                Arguments are passed to ClamAV --exclude AND --exclude-dir options, please
                refer to its official documentation.
                (https://www.clamav.net/documents/clam-antivirus-user-manual)

    == IMAGE SOURCE OPTIONS ==

    [default] If --storage-type is not specified, pull container image from registry.

        == REGISTRY AUTHENTICATION ==

        When pulling from the registry,
        the credentials in the config file located at /config/auth.json will be
        used (so you can mount a docker config.json file, for example).
        Legacy .dockercfg file is also supported.
        Alternatively, you can provide authentication credentials with:
        --registry-auth-basic     username:password  Authenticate using the provided <username> and <password>
        --registry-auth-token     <TOKEN>            Authenticate using this Bearer <Token>
        --registry-auth-file      <PATH>             Path to config.json or auth.json file with registry credentials
        --registry-auth-dockercfg <PATH>             Path to legacy .dockercfg file with registry credentials

        == TLS OPTIONS ==

        -n                    Skip TLS certificate validation when pulling image
                              Alias: --registry-skip-tls

    --storage-type <SOURCE-TYPE>

        Where <SOURCE-TYPE> can be one of:

        docker-daemon   Get the image from the Docker daemon.
                        Requires /var/run/docker.sock to be mounted in the container
        cri-o           Get the image from containers-storage (CRI-O and others).
                        Requires mounting /etc/containers/storage.conf and /var/lib/containers
        docker-archive  Image is provided as a Docker .tar file (from docker save).
                        Tarfile must be mounted inside the container and path set with --storage-path
        oci-archive     Image is provided as a OCI image tar file.
                        Tarfile must be mounted inside the container and path set with --storage-path
        oci-dir         Image is provided as a OCI image, untared.
                        The directory must be mounted inside the container and path set with --storage-path

    --storage-path <PATH>   Specifies the path to the source of the image to scan, that has to be
                            mounted inside the container, it is required if --storage-type is set to
                            docker-archive, oci-archive or oci-dir

    == EXIT CODES ==

    0   Scan result "pass"
    1   Scan result "fail"
    2   Wrong parameters
    3   Error during execution
```

#### Supported Execution Modes and Image Formats

The inline scanner can pull the target image from different sources.
Each case requires a different set of parameters and/or host mounts, as
described in the relevant [Execution
Examples](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#execution-examples).

- [Directly pulling from a registry:
    ](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#quick-start)default
    option if no other source is specified

- [Docker
    daemon:](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#docker-daemon)
    Retrieve the image from the host-local Docker daemon

- [Docker archive:
    ](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#docker-archive)Passing
    a Docker tar file (i.e. output of `docker save`)

- [OCI archive:
    ](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#oci-archive)passing
    an OCI tar file

- [OCI layout:
    ](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#oci-layout)OCI
    image directory

- [Container storage (CRI-O and
    others):](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#container-storage-build-w-buildah--scan-w-podman)
    CRI-O storage directory

#### Output Options

When the inline scanner has completed the image analysis, it sends the
metadata to the Sysdig Secure backend to perform the [policy
evaluation](/en/docs/sysdig-secure/policies/#policies) step. The scan
results can then be consumed inline or by accessing the Secure UI.

##### Container Exit Code

The container exit codes are:

- **0** - **image** **passed** policy evaluation

- **1** - **image** **failed** policy evaluation

- **2** - **incorrect** parameters (i.e. no API token)

- **3** - **other** execution errors

Use the exit code, for example, to decide whether to abort the CI/CD
pipeline.

##### Standard Output

The standard output produces a human-readable output including:

- Image information (digest, image ID, etc)

- Evaluation results, including the final pass / fail decision

- A link to visualize the complete scan report using the Sysdig UI

If you prefer JSON output, simply pass `--format JSON` as a parameter.

##### JSON Output

{{% callout type="note" %}}

This feature is in Technical Preview status.

{{% /callout %}}

You can write a JSON report, while keeping the human-readable output in
the console, by adding the following flag:
`--write-json /out/report.json`

Remember to bind mount the output directory from the host in the
container and provide the corresponding write permissions.

##### PDF Report

You can also download the scan result PDF in a specified container-local
directory. Remember to mount this directory from the host in the
container to retain the data.

`--report-folder /output`

#### Execution Examples

##### Docker Daemon

Scan a local image build; mounting the host Docker socket is required.
You might need to include Docker options '`-u root`' and
'`--privileged`', depending on the access permissions
for`/var/run/docker.sock`

```yaml
docker build -t <image-name> .

docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    quay.io/sysdig/secure-inline-scan:2 \
    --sysdig-url <omitted> \
    --sysdig-token <omitted> \
    --storage-type docker-daemon \
    --storage-path /var/run/docker.sock \
    <image-name>
```

##### Docker Archive

Trigger the scan, assuming the image is available as an image tarball at
`image.tar`. For example, the command
`docker save <image-name> -o image.tar` creates a tarball for
`<image-name>`. Mount this file inside the container:

```yaml
docker run --rm \
    -v ${PWD}/image.tar:/tmp/image.tar \
    quay.io/sysdig/secure-inline-scan:2 \
    --sysdig-url <omitted> \
    --sysdig-token <omitted> \
    --storage-type docker-archive \
    --storage-path /tmp/image.tar \
    <image-name>
```

##### OCI Archive

Trigger the scan assuming the image is available as an OCI tarball at
`oci-image.tar`.  Mount this file inside the container:

```yaml
docker run --rm \
    -v ${PWD}/oci-image.tar:/tmp/oci-image.tar \
    quay.io/sysdig/secure-inline-scan:2 \
    --sysdig-url <omitted> \
    --sysdig-token <omitted> \
    --storage-type oci-archive \
    --storage-path /tmp/oci-image.tar \
    <image-name>
```

##### OCI Layout

Trigger the scan assuming the image is available in OCI format in the
directory`./oci-image`. Mount the OCI directory inside the container:

```yaml
docker run --rm \
    -v ${PWD}/oci-image:/tmp/oci-image \
    quay.io/sysdig/secure-inline-scan:2 \
    --sysdig-url <omitted> \
    --sysdig-token <omitted> \
    --storage-type oci-dir \
    --storage-path /tmp/oci-image \
    <image-name>
```

##### Container Storage: Build w/ Buildah & Scan w/ Podman

Build an image using Buildah from a Dockerfile, and perform a scan. You
might need to include docker options `'-u root'`and `'--privileged'`,
depending on the access permissions for `/var/lib/containers`. Mount the
container storage folder inside the container:

```yaml
sudo buildah build-using-dockerfile -t myimage

sudo podman run \
--rm -u root --privileged \
-v /var/lib/containers/:/var/lib/containers \
quay.io/sysdig/secure-inline-scan:2 \
--storage-type cri-o \
--sysdig-token <omitted> \
localhost/myimage
```

#### Using a Proxy

To use a proxy, set the standard http\_proxy and https\_proxy variables
when running the container.

Example:

```yaml
docker run --rm \
    -e http_proxy="http://my-proxy:3128" \
    -e https_proxy="http://my-proxy:3128" \
    quay.io/sysdig/secure-inline-scan:2 \
    --sysdig-url <omitted> \
    --sysdig-token <omitted> \
    alpine
```

Both `http_proxy` and `https_proxy` variables are required, as some
tools will use a per-scheme proxy.The `no_proxy` variable can be used to
define a list of hosts that don't use the proxy.

### Perform Inline Malware Scanning

It is now possible to scan the image contents for malware as part of the
inline scanning process.

**Note** two important details to consider:

- **Malware scanning is resource intensive.** Enable this mode for the
    required set of images only, i.e. images coming from an
    untrustworthy source.

- **Malware contents in the image are not communicated back to the
    Sysdig backend**. The default behavior if malware is found is to
    consider the scan failed, report malware details, and abort
    analysis.

#### Malware Scanning Flags

- `--malware-scan-enable:`Enable malware detection as part of the
    inline scanner analysis process. Mounting an external malware
    database (Using ClamAV format) is highly recommended. If no existing
    malware database is detected, the analyzer will try to download one
    on the spot, which means the download time will be added to the
    analysis process and that it will require network connectivity to
    pull this database from the Internet.

- `--malware-scan-db-path <PATH>:`Local container path with updated
    ClamAV database. Will be used to call `clamscan` command as
    `clamscan --database=<PATH>`

- `--malware-fail-fast true|false:`Fail fast is `true` by default,
    meaning that, on malware found, the image contents will not be sent
    to the Sysdig backend for further policy evaluation. If you want to
    send the image contents to the Backend, even on malware detected,
    set this parameter to `false`.

    **About this flag, note:**

  - If no malware is detected, the image contents will be sent to
        the backend and follow the regular evaluation.

  - If malware is detected, the container will exit with error
        code 1.

- `--malware-scan-output <DIR-PATH>:` Save JSON output of scan to
    path. Will be saved to `<PATH>/malware_findings.json`. Path must
    exist and must be a directory; remember to mount this path from an
    external directory if you want to persist the data.

- `--malware-exclude REGEX:` It is possible to exclude image
    directories to speed up the malware analysis (for example
    directories that only contain signed software or sizable files).

Arguments are passed to ClamAV. For `--exclude` AND `--exclude-dir`
options, please refer to their [official
documentation](https://www.clamav.net/documents/clam-antivirus-user-manual).

#### Example: Mounting External Malware Database (Recommended)

Note that `/absolute/path/to/clamdb/folder` (and its contained files)
must have `read` and `execute` permissions set for the current Docker
user.

Refer to ClamAV [official
documentation](https://www.clamav.net/documents/clam-antivirus-user-manual).for
how to download and keep a local database in sync.

```yaml
docker run --rm -v /absolute/path/to/clamdb/folder:/malwaredb quay.io/sysdig/secure-inline-scan:2 --sysdig-token <API_Token> --malware-scan-enable --malware-scan-db-path /malwaredb malwares/malware-example:7

Skipping send analysis to Secure backend due to Malware scan failure.
This behaviour can be tuned via --malware-fail-fast option.
Path                                                                        | Signature
/data/29D6161522C7F7F21B35401907C702BDDB05ED47.bin
| Win.Trojan.Emotet-6799923-0
/data/29D6161522C7F7F21B35401907C702BDDB05ED47.bin
| Win.Trojan.Emotet-9769817-0
/data/29D6161522C7F7F21B35401907C702BDDB05ED47.bin
| Win.Trojan.Emotet-9769818-0
/data/29D6161522C7F7F21B35401907C702BDDB05ED47.bin
| Win.Trojan.Agent-1280578
/data/29D6161522C7F7F21B35401907C702BDDB05ED47.bin!(0)
| Win.Trojan.Agent-1280578
```

#### Example: Download Malware Database Inline

```yaml
docker run --rm quay.io/sysdig/secure-inline-scan:2 --sysdig-token <API_Token> --malware-scan-enable malwares/malware-example:7
```

{{% callout type="note" %}}

Malware detection is provided by a third-party software. Sysdig cannot
directly remediate false negatives or false positives reported by this
functionality.

{{% /callout %}}

## Pipeline Integration Examples

There are well-documented examples for a variety of pipelines:

- [Gitlab](https://sysdig.com/blog/gitlab-ci-cd-image-scanning/)

- [Github
    Actions](https://sysdig.com/blog/image-scanning-github-actions/)

- [AWS
    Codepipeline](https://sysdig.com/blog/image-scanning-aws-codepipeline-codebuild/)

- [Azure
    Pipelines](https://sysdig.com/blog/image-scanning-azure-pipelines/)

- [CircleCI](https://sysdig.com/blog/image-scanning-circleci/)

- [Tekton
    Pipelines](https://sysdig.com/blog/securing-tekton-pipelines-openshift/)

### Additional Options

You can also:

- [Integrate with Jenkins](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/integrate-with-jenkins/#integrate-with-jenkins)
