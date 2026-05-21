# Notes

## What is an RFQ?

An RFQ (Request for Quotation) is a formal request sent to vendors asking them to quote pricing, availability, and terms for a specific transportation requirement. It is used when the customer or platform already knows the required service details and needs comparable vendor responses before selecting the best option.

The RFQ process usually starts when the requirement is created with details such as pickup and drop locations, vehicle needs, dates, service expectations, and bidding rules. Eligible vendors review the RFQ and submit bids within the bid window. After bidding closes, the platform or customer compares the bids, may review vendor capability and compliance, and then awards the RFQ to the selected vendor.

## What is a bid window?

A bid window is the time period during which vendors are allowed to submit bids for an RFQ. It starts when the RFQ is published or opened for bidding, and ends at the configured bid closing time. After the bid window closes, vendors should not be able to create or modify bids unless an admin explicitly extends or reopens the RFQ.

## What is forecast?

A forecast is an expected future transportation requirement, usually shared before it becomes a confirmed booking or RFQ. It helps the platform, admins, and vendors plan vehicle capacity, pricing, lanes, and availability in advance. A forecast is not itself a confirmed booking unless it is later converted into an actual booking or RFQ.

## What is an indent?

An indent is a formal demand or planning request for transport capacity. In these use cases it is often close to a forecast: it captures the customer's expected movement requirement before final execution, such as lane, pickup date/time, vehicle requirement, material/load details, and commercial context.

An indent should not be treated as the same thing as an execution booking. It can become the anchor for an RFQ, bid review, award, or later booking creation, but the actual booking lifecycle starts only when a transport job is confirmed and created for execution.

## What is POD?

POD usually means Proof of Delivery in these use cases. It is the evidence that delivery happened, such as a signed receipt, uploaded document, photo, electronic acknowledgement, OTP confirmation, or scanned physical copy. POD can affect delivery confirmation, customer reporting, payment handoff, and POD pendency reports.

Be careful because POD can also mean Port of Discharge in some logistics contexts. In this documentation set, when POD appears near delivery confirmation, documents, reports, or receipts, read it as Proof of Delivery unless a location/port context explicitly says otherwise.

## What is LR?

LR means Lorry Receipt. It is a transport document or receipt generated or handed over around pickup/delivery, depending on customer and load policy. It typically records shipment, consignor/consignee, vehicle, route, and receipt details used for operational proof, customer handoff, and downstream document/reporting workflows.

LR is related to POD but not the same thing. LR is the transport receipt/document for the consignment, while POD is the evidence that delivery was completed.

## What is SAP/ERP payment sync?

SAP/ERP payment sync is the controlled exchange between the logistics platform and the external finance/accounting system. The platform may show invoices, payment attempts, receipts, refunds, and settlement status, but the accounting truth such as ledger posting, tax books, and finance close lives in SAP or the configured ERP.

This distinction matters because the platform should not silently invent final finance state. It should send or receive updates through approved APIs, store correlation references, handle retries safely, and keep its operational payment view consistent with the ERP result.

## What is SIM-based tracking?

SIM-based tracking is vehicle location tracking supplied by a SIM, GPS, or telematics provider instead of the driver's mobile app. It can be used when mobile GPS is unavailable, unreliable, or when customer/policy requires provider-backed vehicle tracking.

SIM-based tracking is a tracking source, not a booking status. The system may need to compare it with mobile GPS, manual updates, or other telemetry before deciding the latest trusted location shown to operations or customers.

## What is duty status?

Duty status is the driver's work-availability state, such as available, busy, offline, or otherwise unavailable. It helps dispatch and fleet operations decide whether a driver can be considered for assignment or monitoring.

Duty status should not be confused with load state. A driver may be on duty but not currently tied to a booking, or may be busy because they are already assigned to an active trip.

## What is pricing basis?

Pricing basis is the source or method used to calculate or choose the commercial price for a booking, quote, RFQ, or lane. Examples include customer rate card, vehicle type pricing, fixed lane rate, distance-based pricing, vendor contract, manual quote, spot rate, or RFQ outcome.

This term is important because two prices can have the same amount but different governance. A manually entered quote, an approved rate-card price, and a winning RFQ bid may each require different approvals, audit records, validity rules, and downstream handling.

## What is a Supplier, Truck, and Fleet in the Payments module?

In the Payments module, the `party_type` field identifies who the payment is for. There are three values:

**Supplier (party_type = 1)** is the customer who placed the booking — the shipper or consignor who owes money to WowTruck for freight services. Despite the label "Supplier", the `party_id` maps to `customer.customer_id`. This is the receivables side.

**Truck (party_type = 2)** is an individual driver/truck. The `party_id` maps to `driver.driver_id`. Only active/onboarded drivers qualify. Journal adjustments post to the driver's ledger via `DriverJournal`. This is the payables side.

**Fleet (party_type = 3)** is a transporter company or fleet operator that may manage multiple trucks and drivers. The `party_id` maps to `transporter_info.transporter_info_id`. Journal adjustments post to the transporter's ledger. This is also the payables side.

The key distinction between Truck and Fleet is the level of the entity: Truck is an individual driver, Fleet is the company/transporter that owns or manages trucks.

## What is fixed lane vs per-KM pricing?

Fixed lane and per-KM are the two pricing modes on a corporate rate card (`corporate_rate_card.lane_or_km`):

**Per KM (lane_or_km = 0):** The rate card stores a `cost_per_km` value. The total booking amount is calculated dynamically as `distance × cost_per_km`, where distance is the driving distance between pickup and drop coordinates.

**Fixed Lane (lane_or_km = 1):** A flat agreed amount for the entire route is stored as `total_amount`. The booking amount is that flat figure regardless of actual distance. The system back-calculates `cost_per_km = lane_amount / distance` for internal reference, but billing uses the flat rate.

Both modes are anchored to a specific pickup → drop location pair. The distinction is how the price is governed: per-KM is variable and distance-driven; fixed lane is a negotiated flat rate for that route. When a booking is created from a rate card, the mode is surfaced to the operator as `"LANE"` or `"KM"`.

## What is an idempotency key?

An idempotency key is a unique reference used to make repeated requests safe. If a payment callback, ERP sync, notification send, webhook, or retry is received more than once, the system can use the key to recognize the repeated operation and avoid creating duplicate business effects.

For example, a payment success callback might arrive twice from a gateway. With idempotency, the platform records one successful payment outcome instead of posting two receipts or triggering duplicate invoice updates.

## What is the booking lifecycle?

The booking lifecycle is the governed sequence of statuses that describes the execution state of a transport job. In the Platform 5.0 use cases the core flow is: Pending or Manual, Accepted, On The Way, Reached, On Delivery, and Delivered.

This lifecycle is about the booking itself, not every surrounding workflow. Assignment, GPS polling, POD upload, LR generation, invoices, payments, and notifications may all react to lifecycle changes, but they should not each invent their own meaning for whether a booking is active, delivered, cancelled, or closed.

## What is booking assignment?

Booking assignment is the link between a booking and the execution resource expected to perform it. That resource may be a driver, vehicle, vendor, transporter, or a configured combination depending on the workflow.

Assignment is not the same as delivery execution. A booking can be assigned and accepted before the vehicle has started moving. Assignment records also need their own history because offers, acceptances, rejections, no-response cases, cancellations, removals, and reassignments can happen before or during execution.

## What is the difference between booking status and assignment status?

Booking status describes where the transport job is in its execution lifecycle, such as Pending, Accepted, On The Way, Reached, On Delivery, or Delivered.

Assignment status describes the state of a specific assignment attempt, such as pending, accepted, rejected, cancelled, no response, driver cancelled, or driver removed. A booking may have several assignment attempts over time, but only one current booking lifecycle state.

This distinction matters during reassignment. A rejected assignment should not automatically mean the booking itself is cancelled; it may simply return to a manual or pending queue for another assignment attempt.

## What is manual intervention?

Manual intervention is an authorised admin or operations action that changes something the normal workflow would otherwise control, such as assignment, booking status, amount, closure, or exception handling.

It should be treated as a governed exception, not as a casual edit. The system should capture who made the change, why it was needed, what changed, and what downstream workflows may be affected.

## What is resource release?

Resource release means updating a driver, vehicle, or vendor resource so it becomes available again after an assignment ends. It usually happens after delivery confirmation, cancellation, rejection, driver removal, reassignment, or another terminal/exception outcome.

This is separate from booking status. If the booking is closed but the driver or vehicle is not released correctly, dispatch may wrongly treat the resource as busy. If the resource is released too early, the same driver or vehicle may be assigned to conflicting work.

## What is a scheduled booking?

A scheduled booking is a booking planned for future execution. It is not yet active for dispatch or vehicle movement, even though the booking record already exists.

Scheduled bookings are useful for planning capacity, customer visibility, and future assignment. They should not be confused with On The Way or On Delivery states, because those imply real execution movement.

## What is a bid approved booking?

A bid approved booking is a pre-execution state where an RFQ, forecast, or booking has an approved bid or selected winning vendor and is ready for the next confirmation or assignment step.

It means the commercial decision has moved forward, not necessarily that a truck is already assigned or moving. The booking still needs to pass the relevant execution workflow before it becomes an active trip.

## What is the difference between cancellation, rejection, and early closure?

Cancellation usually stops a booking before normal completion, often by customer, admin, vendor, or policy action. Rejection is usually an assignment-level or supply-side refusal, where a driver or vendor declines an assignment or booking offer.

Early closure is different: it means execution had started but the booking is closed before normal delivery completion under an authorised exception policy. Early closure needs stronger audit because it can affect POD, LR, payments, customer reporting, and resource release.

## What is driver availability vs load state?

Driver availability is the operational signal used by dispatch to decide whether a driver can be considered for work. It may include states such as available, busy, offline, blocked, or unavailable.

Load state is narrower: it indicates whether the driver or vehicle is currently tied to an active booking or trip. A driver may be on duty and available with no load, on duty but busy with an active load, or offline regardless of load history.

## What is the latest trusted location?

The latest trusted location is the current location the platform chooses to show after evaluating all available tracking sources. Sources may include mobile GPS, SIM-based tracking, third-party telemetry, manual updates, or imported tracking data.

It is not always the newest raw GPS point. The system may prefer a fresher, validated, higher-priority, or more reliable source and reject stale, impossible, duplicate, or suspicious points before updating the visible location.

## What is stale tracking?

Stale tracking means the latest accepted location is older than the configured freshness threshold. The vehicle or driver may still be moving, but the platform no longer has recent enough telemetry to trust the displayed location as current.

Stale tracking should be shown as a tracking quality issue, not as a booking lifecycle status. A booking can be On Delivery while its tracking is stale, and operations may need to intervene without changing the booking's execution state.

## What is a fleet relationship?

A fleet relationship is the active or historical link between a driver, vehicle, and vendor. It controls who can use which driver or vehicle for assignment, reporting, tracking visibility, compliance, and vendor-scoped operations.

This relationship can change over time, but old bookings must remain auditable under the relationship that existed when the work happened. Unlinking a driver or vehicle later should not erase historical assignment context.

## What is vehicle compliance state?

Vehicle compliance state is the derived readiness of a vehicle based on onboarding, e-KYC, required documents, expiry dates, verification outcome, maintenance policy, and any admin or policy blocks.

It is more than just "vehicle exists". A vehicle may be registered in the system but still not eligible for assignment because RC, insurance, permit, fitness, pollution certificate, or another required artefact is missing, expired, pending review, or failed verification.

## What is vehicle document expiry?

Vehicle document expiry is the validity deadline for compliance documents such as RC, insurance, fitness certificate, permit, or pollution certificate.

Expiry matters because the platform may need to warn users before the date, block assignment after the date, route the vehicle into a renewal workflow, or show the issue on fleet monitoring screens. The exact impact depends on configured policy.

## What is a route master or lane?

A route master or lane is a managed source-to-destination route used by the platform for booking validation, pricing, serviceability, dispatch planning, distance calculation, ETA, and reporting.

It is not just a pair of map pins. A lane can carry business rules such as allowed vehicle types, stored distance, fixed price, per-km pricing, customer rate card applicability, vendor contract scope, and reporting grouping.

## What is accepted distance?

Accepted distance is the final distance value the platform decides to use after comparing possible distance sources. Those sources may include map/GIS distance, stored lane distance, driver reading, paid kilometres, customer-care override, or manually approved distance.

This matters because distance can affect pricing, reporting, driver/vendor settlement, customer billing, and route planning. The accepted value should be auditable, especially when it differs from the map provider's calculated distance.

## What is a notification template vs notification attempt?

A notification template is the approved message definition: event, channel, language, variables, content, and version. It defines what the system intends to send for an event such as booking creation, dispatch, delivery, POD pending, payment update, or support ticket status.

A notification attempt is one actual send action to one recipient through one channel/provider. It stores details such as provider reference, delivery status, failure reason, retry count, fallback status, and timestamp.

## What is an integration connector vs integration transaction?

An integration connector is the configured connection to an external provider, such as a payment gateway, SAP, map provider, tracking provider, notification provider, OCR service, or KYC provider. It covers credentials, endpoint, environment, timeout, retry policy, fallback rules, and provider ownership.

An integration transaction is one actual request, response, retry, callback, webhook, or status lookup that happened through that connector. The connector is the setup; the transaction is the auditable event trail of external communication.
