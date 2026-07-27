---
inclusion: manual
---
# Notification and Communication Catalogue — Steering Guide

## Dependencies
Read these artefacts before producing this one:
- 06 Data Model and Entity Catalogue
- 07 Business Rules Catalogue
- 08 External Integration Contract Summary

## Load These References
- `steering/inspection-method.md` (Stage 8: Notification and Communication Analysis)

---

## What to Produce

**File:** `09-notification-alert-and-communication-catalogue.md`

**Purpose:** Identify notifications, alerts, emails, messages, UI banners, audit communications, or other generated communications across the process or UX journey.

**Should include:**

- Communication ID
- Communication type: email, SMS, push notification, in-app alert, UI banner, system alert, audit event, webhook message, report, or other
- Triggering event or process step
- Recipient or audience where observable
- Sender or originating system where observable
- Channel or delivery mechanism
- Template, subject, title, body, variables, and placeholders where observable
- Conditions that cause the communication to be sent or suppressed
- Timing: immediate, scheduled, batch, retry, reminder, escalation, or unknown
- User action expected from the communication
- Business purpose of the communication
- Failure handling or delivery tracking where observable
- Related business entity, rule, scenario, or process step
- Evidence references
- Open questions

---

## Alert Documentation Scope

**User-facing communications (MUST include ALL without exception):**
- Emails, SMS, push notifications, webhooks to external parties
- UI banners, in-app alerts, user-visible messages
- Any communication triggered by or consumed by external services on behalf of users

**System/operational alerts (include business-significant only):**
- Document only business-significant operational alerts
- Do NOT exhaustively list all critical-level log entries (DB errors, deserialization failures, generic infrastructure errors)
- Include a summary note stating that additional critical logs exist beyond those documented
- Offer to generate a complete exhaustive alert catalogue only if the user explicitly requests it

**Rule:** If in doubt whether a system alert is "business-significant", include it if the alert represents a distinct failure condition that would require a unique operational response or business decision.

---

## Definition of Done

Done when:

- each communication has a `COMMS` ID,
- trigger, audience, channel, content/template, timing, and business purpose are captured where observable,
- generated messages across process and UX journeys are considered,
- suppression or eligibility rules are captured,
- failure handling is documented where observable.

**Do not proceed if:**

- emails or alerts are merely listed without triggers,
- UI messages and system communications are ignored,
- notification-related business rules are not linked.
