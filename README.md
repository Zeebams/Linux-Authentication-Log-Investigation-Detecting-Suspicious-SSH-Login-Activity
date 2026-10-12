# Linux Authentication Log Investigation: Detecting Suspicious SSH Login Activity

## Project Overview

This project demonstrates how to investigate Linux SSH authentication logs to identify suspicious login patterns and assess potential unauthorized access.

Using a simulated `auth.log` dataset, I analyzed failed and successful authentication events, correlated source IP addresses with user accounts, reconstructed the event timeline, and documented recommendations for further investigation.

The exercise focuses on identifying suspicious patterns while distinguishing observed evidence from assumptions about attacker behavior.

**Project type:** SOC Analyst portfolio project
**Log source:** Simulated Linux SSH authentication logs
**Tools:** TextEdit and manual log analysis
**Dataset:** 13 simulated authentication events
**Status:** Initial investigation completed

## Objectives

* Identify failed and successful SSH authentication attempts.
* Detect repeated failures against multiple user accounts.
* Investigate a successful login following repeated failures.
* Correlate authentication events by timestamp, account, and source IP.
* Assess potential risks and identify evidence gaps.
* Recommend additional investigation steps.

## Dataset Description

The dataset contains 13 simulated SSH authentication events with the following fields:

* Timestamp
* Hostname
* SSH daemon process identifier
* Authentication outcome
* Username
* Source IP address
* Connection port

The dataset includes the following authentication patterns:

* Successful public-key logins for `a.owens`.
* Three failed password attempts against `root`.
* Four failed password attempts against `j.morgan`, followed by a successful password login.
* Two failed password attempts against `service_backup`.
* A later successful password login for `j.morgan` from a different source IP.

**Data note:** The dataset is simulated for educational purposes. Addresses in the `192.0.2.x` range are reserved for documentation and examples and do not identify real attackers.

## Investigation Methodology

I reviewed the authentication log entries and grouped them by event type, username, source IP address, and timestamp.

I then:

1. Counted failed password authentication events.
2. Identified source addresses associated with repeated failures.
3. Examined whether failures targeted multiple accounts.
4. Correlated failed attempts with subsequent successful authentication.
5. Reconstructed the event timeline.
6. Assessed potential security implications.
7. Documented evidence gaps and recommended further investigation.

## Key Findings

### 1. Repeated Failed Authentication

The dataset contained nine failed password authentication events.

| Account          | Failed Attempts | Source IP    |
| ---------------- | --------------: | ------------ |
| `root`           |               3 | `192.0.2.44` |
| `j.morgan`       |               4 | `192.0.2.44` |
| `service_backup` |               2 | `192.0.2.33` |
| **Total**        |           **9** | —            |

The repeated failures against multiple accounts from `192.0.2.44` may indicate password-guessing activity. However, the log alone does not prove that the attempts were malicious or originated from the same person.

### 2. Failed Attempts Followed by Successful Login

The `j.morgan` account recorded four failed password attempts from `192.0.2.44`, followed by a successful password login at 09:12:45.

The successful login occurred seven seconds after the last recorded failure.

This sequence warrants further investigation for possible password guessing or unauthorized access. A legitimate explanation remains possible.

### 3. Later Login from a Different IP Address

At 10:02:10, `j.morgan` successfully logged in from `192.0.2.10`.

The address differed from the source IP associated with the earlier failed attempts and successful login.

An IP address change is not sufficient evidence of malicious activity. Additional information, such as expected user activity, device details, approved access records, and related system events, would be required.

## Event Timeline

| Time              | Event                      | Account          | Source IP    |
| ----------------- | -------------------------- | ---------------- | ------------ |
| 09:12:03–09:12:14 | 3 failed password attempts | `root`           | `192.0.2.44` |
| 09:12:20–09:12:38 | 4 failed password attempts | `j.morgan`       | `192.0.2.44` |
| 09:12:45          | Successful password login  | `j.morgan`       | `192.0.2.44` |
| 09:45:17–09:45:22 | 2 failed password attempts | `service_backup` | `192.0.2.33` |
| 10:02:10          | Successful password login  | `j.morgan`       | `192.0.2.10` |

All times are taken from the simulated dataset.

## Risk Assessment

The authentication sequence raises concern about possible password-guessing activity and unauthorized account access.

Potential consequences, if access were unauthorized, could include access to sensitive files, unauthorized account creation, privilege escalation, or attempts to access other systems.

These are potential risks rather than confirmed events. The dataset does not establish that any of these actions occurred.

## Recommended Follow-Up Investigation

To continue the investigation, I would:

* Review process execution and Linux audit logs for activity after the successful login.
* Verify whether the `j.morgan` logins were expected.
* Examine account-management records for new accounts or privilege changes.
* Review available file access and endpoint monitoring data.
* Correlate authentication events with relevant system and network logs.
* Investigate related activity before taking containment actions.

## Limitations

This project uses simulated authentication logs and does not represent a confirmed real-world incident.

The dataset does not contain evidence of commands executed after login, file access, account creation, privilege escalation, or credential theft.

Consequently, the investigation identifies suspicious patterns and recommends further checks but does not claim to confirm a compromise.

## Skills Demonstrated

* Linux authentication log analysis
* SSH authentication investigation
* Detection of repeated login failures
* Event timeline reconstruction
* Source IP and account correlation
* Preliminary security risk assessment
* Incident report writing
* Evidence-based security reasoning
* Follow-up investigation planning

## Repository Contents

* `README.md` — Project overview, methodology, findings, and limitations.
* `linux_auth.log.txt` — Simulated Linux SSH authentication log.
* `incident_report.md` — Full investigation report.

## Disclaimer

This project was created for cybersecurity learning and portfolio demonstration. The log data is simulated, and all findings are limited to the supplied dataset.
