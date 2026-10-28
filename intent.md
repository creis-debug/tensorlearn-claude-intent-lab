# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded history and approves nothing in this intent.

## Problem

- **P-1** The fictional clinic receives avoidable calls from patients checking whether their results are ready. PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready."

## Proposed outcome

- **O-1** The patient is told when a result is ready for their review in the existing authenticated portal. PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal."
- **O-2** "Ready" means a doctor has reviewed **and** released the result. Lab upload alone never makes a result eligible. PRD: "A doctor must review and release the result before any notification is eligible." Product owner (notes): "'Ready' must not mean that the laboratory upload merely finished."
- **O-3** The message says only that an update is available and points the patient to the existing authenticated portal.

### How success will be observed

- **S-1** Results-status calls go down, counted as status-only calls, separately from clinical questions, and compared with a measured baseline. PRD: "Measure reduction in results-status calls…". Support lead: "count status-only calls separately from clinical questions; compare against a measured baseline."
- **S-2** Notification delivery failures are tracked. PRD: "…and delivery failures."
- **S-3** No baseline or numeric target is approved; both will be agreed during discovery. PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet." The superseded request's "90% call reduction in one week" is not a target.

## Affected users and systems

- **U-1** Patients, who receive the notification and view results in the existing portal.
- **U-2** Doctors, whose review-and-release step gates every notification.
- **U-3** Support staff, who take the results-status calls and measure them.
- **SYS-1** Existing authenticated patient portal and its existing result route (engineering lead: "the existing portal path is adequate").
- **SYS-2** The lab result and release workflow, which supplies the released-result event and its version.
- **SYS-3** The notification delivery path and its operational logs (channel still undecided, see OPEN-1).

## Scope

- **I-1** Use the existing portal result route.
- **I-2** Prevent duplicate notifications for the same released result version.
- **I-3** Handle delivery failures without sending the patient duplicate messages.

## Out of scope

- **X-1** Portal redesign (PRD scope).
- **X-2** A new or rebuilt patient dashboard: "a separate project" (engineering lead). The superseded request asked for this; it is excluded.
- **X-3** Appointment scheduling (PRD scope).
- **X-4** Clinical interpretation of results (PRD scope).
- **X-5** Notifying third parties (PRD scope). Whether the referring doctor is notified is a separate open question (OPEN-2) and is not approved here.
- **X-6** Sending results automatically on lab upload, putting result values in an SMS, and the 90% one-week target, all from the superseded request.

## Constraints

- **C-1** Doctor review and release are required before any notification is eligible (PRD).
- **C-2** No diagnosis, test result, clinical measurement or treatment detail in the message **or in notification/operational logs** (PRD; privacy owner: "no sensitive content in messages or operational logs").
- **C-3** Access is authenticated in the existing portal. No result attachments and no public result link (PRD).
- **C-4** At most one notification per released result version, including when a delivery fails (PRD).
- **C-5** No recipient other than the patient is authorized until OPEN-2 is decided (PRD; clinical owner: "do not infer approval for referring-doctor notifications from the old request").
- **C-6** Patient consent for the channel is still under review (privacy owner), so delivery cannot assume a channel or consent.

## Open questions and owners

- **OPEN-1 Channel** (SMS, email or portal push). Owners: **product owner** and **privacy owner**, deciding after the consent/delivery review. Not decided here, and SMS is not assumed.
- **OPEN-2 Referring-doctor notification**: whether the referring doctor may be told, and how recipients are authorized. Owners: **clinical owner** and **privacy owner**. The PRD says "It is not approved by this PRD." This is separate from the third-party exclusion in X-5.
- **OPEN-3 Baseline and numeric target** for the call-reduction measure, to be agreed during discovery (PRD). No target is set in this intent.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
