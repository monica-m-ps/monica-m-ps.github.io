---
linkTitle: "Events Feed"
title: "Events Feed"
weight: "1"
aliases:
  - /en/threat-detect/
  - /en/events-secure/
  - /en/events-feed
  - /en/docs/sysdig-secure/insights/#insights
  - /en/docs/sysdig-secure/insights/
  - /en/insights
description: "The Events page in Sysdig Secure provides an overview of your infrastructure, and allows you to deep-dive into specific security events, distinguish, false positives, and configure policies to enhance performance."
---

![](/image/events_feed_new.png)

Events provides a navigable interface to:

- Find and surface insights around the most relevant security events in your infrastructure.

- Slice and dice your event data with filters, zones, and groups.

- Inspect individual events in the event detail panel.

- Follow up on forensics and activity audits by directly linking to other sections of the product for additional event information.

Without filters or scope defined, the event list comprises all events within the timeline, in chronological order. Click any event to open the event detail panel on the right.

## Filter Secure Events

The Events feed can be filtered, grouped, and scoped in a variety of ways.

### Search and Filter

Use the top panel to:

- **Select Zones**: Select zones you have access to from the dropdown.

   {{% callout type="note" %}}

  Zones are in Technical Preview. To enable Zones, go to **Settings** > **User Profile**. Under **Sysdig Labs**, toggle **Zones scoping for all features** on. If Zones is not available from your User Profile, contact your Sysdig representative.

  {{% /callout %}}

- Filter by **Event Source**, such as AWS, Okta, and Github.

- Filter by Severity: Toggle the coloured buttons marked H, M, L and I to show or hide events of the severities High, Medium, Low or Info. For info about how severities are mapped to numbers, see [Threat Detect Policies](/en/docs/sysdig-secure/policies/threat-detect-policies/#see-at-a-glance).

- Select the star icon to the right of the search bar to favorite your current filter. Access your favorites and recent searches in the dropdown on the left of the search bar.

  ![](/image/events_fave.png)

- Search: Add a filter, or use free text search to find events.

  - You can search by the event title and scope label values, such as "my-cluster-name," visible in the events lists.

  - Click **Add Filter** for an initial drop-down list of valid scope elements. Keep clicking in the filter bar to view the next logical operand and value that you can add to your expression.

  ![](/image/filter_expression.png)

#### Understand Filter Expression Elements

Filters are additive. Click the operand after an element in an event to add it directly to the filter expression.

  ![](/image/event_filter_operand.png)

Click the is or is-not operand on an event listing to add it to your filter.

Click the star to save a constructed expression as a Favorite.

Click the icon on the left of the bar to access recently used Filter and Favorites .

### Quick Filters

Access the **Quick Filters** bar on the left for Kubernetes, Cloud and Host filters and tags. The number of hits are listed beside each tag.

### Group by Policy

Use the **Group by: Policy** option to sort the event feed into a more useable list. This is useful when a particular policy is generating many events,

| ![](/image/events_nogroup.png) | ![](/image/events_group.png) |
| :---------------------------: | :-------------------------: |
|           No group            |      Grouped by policy      |

### Change Time Span

The time bar allows you to change the time span of for events displayed, as well as pause the intake of events.

![](/image/time_bar_live.png)

Use buttons to view events in specific periods, such as the last ten minutes, three days, or two weeks.

To set a custom time span, select the current date on the left end of the time bar. A calendar will appear, where you can manually specify a range.

Use the pause/play button in the time bar to toggle between **Live** and **Paused**.

- When **Live**, events continually update.
- When **Paused**, new events are withheld, allowing you to review a point in time.

### Examine Trends Over Time

The Trend Over Time graph, above the events list, is a highly dynamic and interactive representation of your event activity over time.

1. Hover your mouse over any section to see the type of event represented.

    ![](/image/graph_drift.png)

2. Click and drag over a period to select it for more detail analysis.

   ![](/image/graph_click_drag.png)

    The graph will reload to represent that period in more detail.

   ![](/image/graph_zoom.png)

3. Use the time bar at the bottom of the screen to change the time period displayed.

## Event Detail Panel

### Review Details

The Event Detail panel is broken into sections, to enable you to quickly review an event, and decide your next step:

- **What**: Contains a brief description of what happened, and the relevant rule.

- **Process**: Contains process details, such as name and parent name.

- **Content**: Contains information such as the Event ID.

- **Who**: Contains information associated with user identity, such as user name, account ID, and source IP address.

- **Cloud**: Contains details related to cloud, such as cloud account ID and region.

- **Triggered Rule Tags**: An expandable list of associated triggered rule tags.

### Take Action

Depending on the event, actions may be available:

- Click **Policy** to edit the policy, or view the rule.

- Click **Tunable Exceptions** to apply suggestions for rule exceptions to create.

- Click the copy icons at the top of the event detail panel to copy the event URL, or copy the event ID in various formats, such as simple text, JSON, or CSV.

- Where available, click the [Captures](/en/docs/sysdig-secure/investigate/captures/) button.

- If set up in the associated policy, a **View Runbook** link or button connects your company's procedure documents.
  
- For Runtime events, the **Activity** shortcut button is available and links to [Activity Audit](/en/activity-audit).
  
- For Image Scanning, the **Scan Results** shortcut links to the [Scan Results](/en/docs/sysdig-secure/vulnerabilities/scanning/review-scan-results/#review-scan-results) page.
  
- For a birds-eye view of the related network activity and the ability to create a netsec policy, the **Network Activity** shortcut links to the [Netsec](/en/docs/sysdig-secure/network/policy-generation/) page. See also: [Quick Link to Netsec Typology](/en/events-feed#quick-link-to-netsec-topology).
  
- For auto-tuning policies to reduce noisy false positives, the **Tunable Events** shortcut  provides a link the Runtime Policy Tuner. Note that the tuner only detects and alerts on rules that have exception **definition**, so the link does not necessarily appear on every event. See also: [Tunable Exceptions](/en/events-feed#tuneable-exceptions).
  
- **Scope**

  The new scope selector allows for additional selector logic (such as: `in, not in, contains`, or `starts-with`), improving the scoping flexibility over previous versions. This scope selector also provides scope variables, allowing you to quickly switch between, for example, Kubernetes namespaces without having to edit the panel scope. See also: [Team Scope and the Event Feed](/en/events-feed#team-scope-and-the-event-feed).

  Note that the scope details listed can be entered in the free-text search field if desired.

- **Portable URLs**

  The Event Feed URL maintains the current filters, scope, and selected elements. You can share this URL with other users to let them display the same data.

### Process Tree

The Process Tree provides visualizations for workload-related events. 

{{% callout type="note" %}}

The Process Tree is enabled by default on:
- Linux Agent versions 12.16.0 and later.
- Windows Agent versions 1.2.0 and later.

{{% /callout %}}

Select an event from the Events Feed to open the Event Details pane. Here, you will see the **Process Tree** summary.

![](/image/process_tree.png)

Click **Explore** to open the Process Tree page.

#### Find Workload Events

Not all events include process trees at this time. Workload events invoke Falco policies that apply to system-call-related data.

Therefore, you can quickly filter for workload events with `ruleType = Falco - Syscall`

#### Explore Process Trees with Event Correlation

Event correlation lets you start from the process tree of a targeted event and view any other events within that process tree from a particular time frame.

No additional requirements are needed to enable this feature apart from previous process tree requirements.

![](/image/process_tree1.png)

- All Process Trees have a "focused" event that determines the scope and time box as to which events to show.
- The process tree always shows the ancestral lineage to the root process. Lineage to the offspring only shows if a Sysdig event has occurred.

##### Anatomy

**Header**
![](/image/events_header.png)

The header in the Process Tree consists of:

- Scope: Metadata on the focused event, which may include the cloud account metadata down to the pod and host metadata. All events shown in the tree have the same scope.
- Time Box Dropdown: The time elapsed before and after the focused event occurred. The default is 15 minutes before and after, with options for 10 and 5 minutes.
- Severity Filters: Based on the severity of the event to filter more or fewer events.

**Process Tree Overlay**

![](/image/process_anatomy.png)

- On the left side of the process tree is a timeline. The process tree starts at the focused event, shown with the bold outline, and the time of the event. The time of each related process is shown in relation to the event, for example `-7 hr` and `+ 2 s`
- Use the chevron on the left to expand or collapse any process with any number of offspring. Process names stacked within a tree also indicated collapsed processes (for example, the second-to-last line in the image).
- The latest, highest severity policy name for an event shows inline of each process within the tree. Any other events are indicated inline with the count of events by severity. The counts do not include the event with the details shown. For example, the last line in the image shows two unique events.

##### Process & Event Summary

![](/image/process_summary.png)

- **Process Summary:** Select a process with one or more events to summarize the process details, including the Process ID, Session ID, username and more. Each unique rule triggered will show under the process details, which will open up the Event Details panel.
- **Event Details:** These are the details provided from the events feed with all event information, including the rules, policies, and related metadata.

### Captures

![](/image/event_capture.png)

For runtime policy events that have an associated capture, you can find available options through the **Respond** button. Here, you can:

- View the capture directly in [Sysdig Inspect](https://sysdig.com/blog/sysdig-inspect/)

- Download the capture

- Delete the capture

If the event is scoped to a particular container, Sysdig Inspect automatically filters the displayed information to the scope of that Container ID.

### Netsec Topology

As part of an event triage, it may be useful to get a birds-eye-view of the network activity for example, to establish what is connected to what, who else a service communicates with, and whether the connection is expected or an outlier.

When relevant, the event detail **Respond** button provides a quick link to the [Network Activity topology](/en/docs/sysdig-secure/network/policy-generation/#use-topology-visualization), visible users with Advanced User privileges or above, as well as the ability for administrators to craft a unique netsec policy as needed.

The event should include cluster/ namespace/workload details (one of `deployment`, `daemonset`, `statefulset`, `job`, `cronjob`), and actual network activity on the workload for the Network Activity link to be offered.

| ![](/image/respond-closeup.png) | ![](/image/netsec-light.png) |
| ------------------------------- | ---------------------------- |

### Tunable Exceptions

Sysdig's Runtime Policy Tuner helps reduce false positives using rule exceptions. If there are potential exceptions that match one or more events from the same rule a **`{#}` Tunable Exceptions** button may appear in the event details, or under the **Respond** button. When clicked, a modal appears with suggestions of matching exceptions.

1. Select an event and open an event to view the event details. If there are available exceptions, you will see an option for **Tunable Exceptions**.

    ![](/image/tunable1.png)

   **NOTE:** You can also navigate to the same event details from the [Insights](/en/insights) module

2. Select **Tunable Exceptions**.

    The **Tune Rule** window appears.

   ![](/image/tune_rule.png)

4. Review the suggested exceptions and decide whether to use them:

   - Compare the **Existing Values** with the **Suggestion**.

   - Adjust the suggested values, if necessary.

     For example, if the suggestion said `contains: prod-app-1` but you wanted to apply the exception to all the clusters in production, you could edit it to  `contains: prod`.

   - Review the previously-applied exceptions that are also displayed, to gain context for the decision.

   - Click **View affected policies** to see all the places the rule and exception would be used.

4. Click **Apply**, or

   - If you want to manage Exceptions outside of Sysdig, click **View Exception as YAML/Terraform** to copy the snippet in your preferred format.

     ![](/image/view_exception.png)

{{% callout type="note" %}}

See also: [Tuning Best Practices](/en/docs/sysdig-secure/policies/threat-detect-policies/runtime-policy-tuning/#tuning-best-practices)

{{% /callout %}}

### Team Scope and the Event Feed

Not every label available in the Sysdig Platform is compatible with the set of labels used to define the scope of a security event in the Event Feed.

Therefore, to correctly determine if a set of events is visible for a certain Sysdig Secure team, the team scope must not use any label outside the following list.

**Permitted Labels**

```yaml
agent.tag.* (any label starting with agent.tag is valid)

host.hostName
host.mac

kubernetes.cluster.name
kubernetes.namespace.name
kubernetes.node.name
kubernetes.namespace.label.field.cattle.io/projectId
kubernetes.namespace.label.project

kubernetes.pod.name

container.name
container.image.id
container.image.repo
container.image.tag
container.image.digest

container.label.io.kubernetes.container.name
container.label.io.kubernetes.pod.name
container.label.io.kubernetes.pod.namespace
container.label.maintainer
```

Not using any label to define team scope (*Everywhere*) is also supported.

If the Secure team scope is defined using a label outside of the list above, the Event Feed will be empty for that particular team.
