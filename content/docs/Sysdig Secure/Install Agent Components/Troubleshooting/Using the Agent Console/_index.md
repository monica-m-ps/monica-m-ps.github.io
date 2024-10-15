---
linkTitle: "Using the Agent Console"
title: "Using the Agent Console"
weight: "20"
aliases:
  - /en/using-the-agent-console.html
  - /en/using-the-agent-console
  - /en/docs/installation/sysdig-agent/agent-troubleshooting/using-the-agent-console/
  - /en/docs/installation/troubleshooting/using-the-agent-console/
Description: "Sysdig provides an Agent Console to interact with the Sysdig agent. This
is a troubleshooting tool to help you view configuration files and
investigate agent configuration problems quickly."
---

## Access Agent Console

1. Select **Integrations** > **Data Sources** > **Sysdig Agent**.
2. On the **Sysdig Agents** page, locate the agent you want to explore.
3. Select the three-dot menu on the right-hand side of the view.
4. Click **Agent Console**.

## Agent Console Commands

### View Help

The `?` command displays the commands to manage Prometheus configuration
and targets monitored by the Sysdig agent.

```yaml
$ prometheus ?
$ prometheus config ?
$ prometheus config show ?
```

### Command Syntax

The syntax of the Agent Console commands is as follows:

```yaml
directory command
directory sub-directory command
directory sub-directory sub-sub-directory command
```

### View Version

Run the following to find the version of the agent running in your
environment:

```yaml
$ version
```

An example output:

```yaml
12.0.0
```

### Troubleshoot Prometheus Metrics Collection

These commands help troubleshoot Prometheus targets configured in your
environment.

For example, the following commands display and scrape the Prometheus
endpoints respectively.

```yaml
$ prometheus target show
$ prometheus target scrape
```

#### Sub-Directory Commands

The Promscrape CLI consists of the following sections.

-   `config`: Manages Sysdig agent-specific Prometheus configuration.

-   `metadata`: Manages metadata associated with the Prometheus targets
    monitored by the Sysdig agent.

-   `stats`: Helps view the global- and job-specific Prometheus
    statistics.

-   `target`: Manages Prometheus endpoints monitored by Sysdig agent.

#### Prometheus Commands

##### Show

The `show` command displays the information about the subsection. For
example, the following example displays the configuration of the
Prometheus server.

```yaml
$ prometheus config show

5  Configuration      Value
6  Enabled            True
7  Target discovery   Prometheus service discovery
8  Scraper            Promscrape v2
9  Ingest raw         True
10  Ingest calculated  True
11  Metric limit       2000
```

##### Scrape

The `scrape` command scrapes a Prometheus target and displays the
information. The syntax is:

```yaml
$ prometheus target scrape -url <URL>
```

For example:

```yaml
$ prometheus target scrape -url http://99.99.99.3:10055/metrics

# HELP go_gc_duration_seconds A summary of the GC invocation durations.
7  # TYPE go_gc_duration_seconds summary
8  go_gc_duration_seconds{quantile="0"} 7.5018e-05
9  go_gc_duration_seconds{quantile="0.25"} 0.000118155
10  go_gc_duration_seconds{quantile="0.5"} 0.000141586
11  go_gc_duration_seconds{quantile="0.75"} 0.000171626
12  go_gc_duration_seconds{quantile="1"} 0.00945638
13  go_gc_duration_seconds_sum 0.114420898
14  go_gc_duration_seconds_count 607
```

### View Agent Configuration

The Agent configuration commands have a different syntax.

Run the following to view the configuration of the agent running in your
environment:

```yaml
$ configuration show-default-yaml
$ configuration show-backend-yaml
# docker environments
$ configuration show-dragent-yaml
# Kubernetes environments
$ configuration show-configmap-yaml

```

The output displays the configuration file. Sensitive data, such as the
credentials, are obfuscated.

```yaml
customerid: "********"
watchdog:
  max_memory_usage_mb: 2048
```

## Security Considerations

-   User-sensitive configuration is obfuscated and not visible through
    the CLI.

-   All the information is read-only. You cannot currently change any
    configuration by using the Agent console.

-   Runs completely inside the agent. It does not use bash or any other
    Linux terminals to prevent the risk of command injection.

-   Runs only via a TLS connection with the Sysdig backend.


## Disable Agent Console

This is currently turned on by default. To turn off Agent Console for a
particular team:

1.  Navigate to **Settings** &gt; **Teams**.

2.  Select the team that you want to disable **Agent Console** for.

3.  From **Additional Permissions**, Deselect **Agent CLI**.

4.  Click **Save**.

To turn it off in your environment, edit the following in the
`dragent.yaml` file:

```yaml
command_line:
  enabled: false