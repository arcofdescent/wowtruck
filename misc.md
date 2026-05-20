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
