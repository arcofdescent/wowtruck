# WowTruck Admin Feature List

This document summarizes the application capabilities menu-wise, based on the active navigation in `views/layouts/main.php`.

Many menu entries are controlled by Yii permissions such as `Yii::$app->user->can(...)`, so the exact menu shown to a user depends on their role and assigned permissions.

## Dashboard / Home

### Outstation Summary
Provides a high-level operational summary for outstation or intercity loads. This is intended for quickly reviewing current outstation booking performance, volumes, and operational status.

Code detail: route `/dashboard/outstationsummary` is exposed from the main layout menu. The menu entry is part of the reporting/dashboard layer, so it is primarily read-only and aimed at summarizing booking, fulfilment, sales, POD, or user-performance data.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### Local Summary
Provides a summary of local or intracity booking activity. This helps operations users monitor local load movement, booking progress, and business volume.

Code detail: route `/dashboard/localsummary` is exposed from the main layout menu. The menu entry is part of the reporting/dashboard layer, so it is primarily read-only and aimed at summarizing booking, fulfilment, sales, POD, or user-performance data.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### MO Performance Summary
Shows performance information for MO or field/operations users. This is likely used to track contribution, customer acquisition, booking activity, or operational productivity by user/team.

Code detail: route `/dashboard/moperformance` is exposed from the main layout menu. The menu entry is part of the reporting/dashboard layer, so it is primarily read-only and aimed at summarizing booking, fulfilment, sales, POD, or user-performance data.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### Fulfilment Report - Local
Reports fulfilment performance for local bookings. This helps identify how many local bookings were successfully fulfilled versus pending, failed, cancelled, or otherwise unresolved.

Code detail: route `/fulfilmentreport/local` is exposed from the main layout menu. The controller uses `BookingSearch::cancelled_bookings_search()` and creates both paginated and non-paginated data providers, which indicates the screen supports grid review plus full export/download behavior.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### Fulfilment Report - Outstation
Reports fulfilment performance for outstation bookings. This gives visibility into intercity load fulfilment and helps monitor service performance for longer-distance movements.

Code detail: route `/fulfilmentreport/outstation` is exposed from the main layout menu. The outstation version calls the same booking search method with the local flag disabled, so it reuses the fulfilment data model while separating local and outstation reporting paths.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### MIS Report Summary
Provides a management-level summary for business and operational reporting. This is useful for leadership or operations managers who need consolidated metrics rather than transaction-level views.

Code detail: route `/dashboard/outstationloadsummary` is exposed from the main layout menu. The menu entry is part of the reporting/dashboard layer, so it is primarily read-only and aimed at summarizing booking, fulfilment, sales, POD, or user-performance data.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### DSR Report Summary
Provides daily sales/reporting information. This helps teams review daily business activity, booking flow, and sales or operational progress.

Code detail: route `/dsrreport/dsr_reports` is exposed from the main layout menu. `DsrreportController` renders the report page and exposes JSON endpoints backed by `Booking::generateCsvDsrreport()` and `Booking::dsrCount()` for date/customer-filtered summaries.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### POD Status Report
Tracks Proof of Delivery status across bookings. This helps billing and operations teams identify which trips have pending, completed, disputed, or missing POD documentation.

Code detail: route `/podreport/pod_reports` is exposed from the main layout menu. `PodreportController` reads POD status from `BookingAddonInfo` and exposes datewise counts, pending POD lists, and age-wise POD counts as JSON endpoints.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### Booking Report Summary
Provides a consolidated summary of booking activity. This can be used to review booking counts, statuses, load categories, or business performance across selected filters.

Code detail: route `/bookingreport/booking_reports` is exposed from the main layout menu. `BookingreportController` renders the summary page and uses `Booking::getDatewiseCount()`, `Booking::getAgewiseStatusCount()`, and `Booking::getHighestAgeingBookings()` for AJAX/report data.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### Business Sales Summary
Summarizes business sales with filters such as date range, load type, customer category, and amount grouping. This is useful for revenue analysis and business trend review.

Code detail: route `/businesssalessummary/index` is exposed from the main layout menu. `BusinesssalessummaryController` posts filter values such as date range, load type, mobile flag, customer category, and amount bucket into `BusinessSalesSummary::downloadSalesReport()`.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

### Driver App Summary
Summarizes driver app activity and usage. This helps track driver-side adoption, activity, and possibly app-driven operational events.

Code detail: route `/driverappsummary/index` is exposed from the main layout menu. `DriverappsummaryController` renders an index view and posts date/value filters into `DriverAppSummary::downloadDriverAppSummaryReport()` for summarized app activity output.

Operationally, this item belongs to the Dashboard / Home area. Users would typically open it at the start of a day or review cycle to identify trends, backlog, exceptions, and areas needing follow-up.

## New Booking

### Local
Allows users to create a new local or intracity booking. The flow is intended for city-level movements where pickup and drop are within a local operating region.

Code detail: route `/booking/multidrop` is exposed from the main layout menu. `BookingController::actionMultidrop()` loads customers through `CustomerSearch::bookingScreenSearch()` and renders `views/booking/create_update.php`, indicating the booking form starts from customer lookup/selection.

Operationally, this item belongs to the New Booking area. Users would use it when a customer request needs to become a load, enquiry, or RFQ record before operational execution begins.

### Outstation
Allows users to create a new outstation or intercity booking. This supports longer-distance shipment creation with route, vehicle, customer, and pricing details.

Code detail: route `/booking/intercity` is exposed from the main layout menu. `BookingController::actionIntercity()` uses the same customer search entry point and renders `views/booking/intercity.php`; related actions fetch corporate rate cards and process route/rate-card pickup/drop details.

Operationally, this item belongs to the New Booking area. Users would use it when a customer request needs to become a load, enquiry, or RFQ record before operational execution begins.

### Enquiry
Captures and manages customer enquiries or quote requests. This is useful before a confirmed booking is created, especially for sales or call-center workflows.

Code detail: route `/getquote/index` is exposed from the main layout menu. `GetquoteController::actionIndex()` lists `GetQuote` records with search, pagination, and newest-first sorting; create/update/view/delete actions follow the standard Yii CRUD pattern.

Operationally, this item belongs to the New Booking area. Users would use it when a customer request needs to become a load, enquiry, or RFQ record before operational execution begins.

### RFQ Requests
Manages corporate RFQ and indent forecast requests. This supports corporate customers who request rates, vehicle supply, or planned logistics capacity before booking confirmation.

Code detail: route `/corporateindentforecast/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the New Booking area. Users would use it when a customer request needs to become a load, enquiry, or RFQ record before operational execution begins.

## Booking Details

### Local Bookings
Provides a searchable/manageable list of local bookings. Users can review booking details, statuses, assignments, delivery progress, and related operational information.

Code detail: route `/booking/index` is exposed from the main layout menu. `BookingController::actionIndex()` provides the main local booking listing; booking view pulls assignments, pickup/drop locations, POD media, LR data, insurance, invoices, and city/pricing permission checks.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Outstation Bookings
Provides a searchable/manageable list of outstation bookings. This supports intercity load monitoring, updates, billing flow, and operational exception handling.

Code detail: route `/booking/outstationbookings` is exposed from the main layout menu. `BookingController::actionOutstationbookings()` is the outstation listing path, while shared view/update actions handle assignment, invoice, POD, LR, and communication details for individual trips.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Booking Exception
Tracks bookings that require special attention or manual intervention. These may include failed assignments, status mismatches, missing data, or operational edge cases.

Code detail: route `/bookingexception/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Consolidate Invoice
Supports generation and management of consolidated invoices. This is mainly useful for corporate customers where multiple bookings are billed together.

Code detail: route `/invoice/consolidateinvoice` is exposed from the main layout menu. `InvoiceController::actionConsolidateinvoice()` accepts customer, load type, date range, billing type, and filters; it calls `Invoice::consolidateinvoice()` and downloads a CSV with totals and bank/signature rows.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Consolidated Customer Summary
Shows customer-level summary information for consolidated billing. It helps finance teams validate customer invoice totals and related booking coverage.

Code detail: route `/clsummary/index` is exposed from the main layout menu. This is an operations and billing work area. Items here usually provide searchable grids, status review, invoice linkage, exception handling, and customer/trip drill-downs.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Consolidate Invoice Dashboard
Provides a dashboard for consolidated invoice processing. This likely helps track invoice generation progress, pending invoices, generated invoices, and exceptions.

Code detail: route `/invoice/consolidateinvoicedashboard` is exposed from the main layout menu. `InvoiceController::actionConsolidateinvoicedashboard()` uses `ClSummarySearch` and `ClSummary` helper methods to generate quarterly, top-customer, three-month, overall, and DPD summary data.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Submitted Invoice Details
Lists invoices that have been submitted into the billing or customer-facing process. This helps track invoice submission status and downstream payment follow-up.

Code detail: route `/submittedinvoice/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Corporate Ratecard
Manages corporate pricing and contracted rate cards. This includes rate definitions that are used while creating bookings or generating corporate invoices.

Code detail: route `/corporateratecard/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Corporate Disputes
Handles disputes raised against corporate invoices. Finance or operations users can track disputed items, reasons, and resolution progress.

Code detail: route `/corporateinvoicedisputes/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### AutoGenerate Invoice Success
Shows invoices that were successfully generated by the automated invoice process. This helps validate batch invoice runs and confirm generated output.

Code detail: route `/autoinvoicegenerationsummary/success` is exposed from the main layout menu. `AutoinvoicegenerationsummaryController::actionSuccess()` uses `AutoInvoiceGenerationSummarySearch::search1()` with a success flag, so the screen isolates successful auto-invoice runs.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### AutoGenerate Invoice Error
Shows failures from the automated invoice process. This helps teams identify bookings or customers where invoice generation failed and requires correction.

Code detail: route `/autoinvoicegenerationsummary/error` is exposed from the main layout menu. `AutoinvoicegenerationsummaryController::actionError()` uses the same search model with an error/failure flag, so finance users can review failures separately from successes.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Customer Summary
Provides a customer-level operational or billing summary. This helps users quickly understand customer activity, invoice state, and booking volume.

Code detail: route `/clsummary/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### Invoices
General invoice management area for viewing, managing, and possibly downloading invoice records. It supports finance workflows tied to booking billing.

Code detail: route `/invoice/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### IVR
Tracks bookings or booking requests received through IVR. This supports call-based booking intake and follow-up.

Code detail: route `/bookingivr/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### WhatsApp
Tracks booking records or requests received through WhatsApp. This supports messaging-based customer or driver communication flows.

Code detail: route `/bookingwhatsapp/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

### SMS
Tracks booking records or requests received through SMS. This is useful for lightweight booking intake, confirmation, or status communication.

Code detail: route `/bookingsms/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Booking Details area. Users would use it after booking creation to monitor progress, correct exceptions, manage documents, and hand records over to billing or customer communication flows.

## Maps

### View and Track
Provides a map-based view for tracking drivers or vehicles. This helps operations teams monitor live or near-live movement and current location.

Code detail: route `/driver/mapviews` is exposed from the main layout menu. `DriverController::actionMapviews()` builds the map page with booking and driver models, polling coordinates, date filters, tab selection, carrier filters, and tracking options.

Operationally, this item belongs to the Maps area. Users would use it during live operations to find drivers, verify positions, track trips, and make assignment or escalation decisions.

### Driver Search
Allows users to search for drivers using map/location-based criteria. This is useful for assignment planning and availability checks.

Code detail: route `/map/driversearch` is exposed from the main layout menu. This area is location-oriented. It relies on driver, booking, polling, and map views to support tracking, search, and operational assignment decisions.

Operationally, this item belongs to the Maps area. Users would use it during live operations to find drivers, verify positions, track trips, and make assignment or escalation decisions.

## Driver Alerts

### All Alerts
Shows all open driver alerts in one place. The menu also displays a badge count for open alerts.

Code detail: route `/alert-log/index` is exposed from the main layout menu. `AlertLogController::actionIndex()` uses `AlertLogSearch` and can optionally receive a type filter; individual alerts can be viewed or marked closed.

Operationally, this item belongs to the Driver Alerts area. Users would use it as an exception queue for driver/trip safety and delivery events, then close items after review or action.

### Halt Alerts
Tracks cases where a driver or vehicle has halted beyond expected limits. This helps operations teams detect potential trip delays or compliance issues.

Code detail: route `/alert-log/index?type=HALT` is exposed from the main layout menu. This area is backed by `AlertLog` records. Alerts are searchable, viewable, and can be closed by a user, creating an auditable operational follow-up flow.

Operationally, this item belongs to the Driver Alerts area. Users would use it as an exception queue for driver/trip safety and delivery events, then close items after review or action.

### Overspeed Alerts
Tracks overspeeding events. This supports safety monitoring and driver behavior follow-up.

Code detail: route `/alert-log/index?type=OVERSPEED` is exposed from the main layout menu. This area is backed by `AlertLog` records. Alerts are searchable, viewable, and can be closed by a user, creating an auditable operational follow-up flow.

Operationally, this item belongs to the Driver Alerts area. Users would use it as an exception queue for driver/trip safety and delivery events, then close items after review or action.

### Daily Distance Alerts
Tracks alerts related to daily distance thresholds or unusual distance behavior. This may be used for route compliance, utilization monitoring, or exception detection.

Code detail: route `/alert-log/index?type=DAILY_DISTANCE` is exposed from the main layout menu. This area is backed by `AlertLog` records. Alerts are searchable, viewable, and can be closed by a user, creating an auditable operational follow-up flow.

Operationally, this item belongs to the Driver Alerts area. Users would use it as an exception queue for driver/trip safety and delivery events, then close items after review or action.

### Delivery Alerts
Tracks delivery completion-related alerts. This helps teams follow up on trips where delivery status needs attention.

Code detail: route `/alert-log/index?type=DELIVERY_COMPLETION` is exposed from the main layout menu. This area is backed by `AlertLog` records. Alerts are searchable, viewable, and can be closed by a user, creating an auditable operational follow-up flow.

Operationally, this item belongs to the Driver Alerts area. Users would use it as an exception queue for driver/trip safety and delivery events, then close items after review or action.

## Customers

### New Customer - Admin Customer
Allows an admin user to create a customer record directly. This supports internal onboarding and manual customer setup.

Code detail: route `/customer/create` is exposed from the main layout menu. Customer screens use search models and CRUD-style views to manage customer profiles, notes, documents, segmentation, referrals, refunds, coupons, and corporate account data.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### New Customer - Corporate Customer
Creates corporate customer/user setup. This supports B2B customers with corporate account structures and user access.

Code detail: route `/user/createcorporatehouser` is exposed from the main layout menu. Customer screens use search models and CRUD-style views to manage customer profiles, notes, documents, segmentation, referrals, refunds, coupons, and corporate account data.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Customer Details - Intracity
Manages customer records for local/intracity business. Users can search, update, and maintain customer information relevant to local bookings.

Code detail: route `/customer/index` is exposed from the main layout menu. Customer screens use search models and CRUD-style views to manage customer profiles, notes, documents, segmentation, referrals, refunds, coupons, and corporate account data.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Customer Details - Intercity
Manages customer records for outstation/intercity business. This supports customers using long-distance logistics services.

Code detail: route `/customer/outstation` is exposed from the main layout menu. Customer screens use search models and CRUD-style views to manage customer profiles, notes, documents, segmentation, referrals, refunds, coupons, and corporate account data.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Customer Details - Corporate Customer
Manages corporate customer accounts and users. This supports account administration for corporate clients.

Code detail: route `/user/corporateindex` is exposed from the main layout menu. Customer screens use search models and CRUD-style views to manage customer profiles, notes, documents, segmentation, referrals, refunds, coupons, and corporate account data.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Leads
Manages prospective customer leads. Sales or operations teams can track leads before conversion into active customers.

Code detail: route `/leads/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Biz Cards
Manages business-card/contact records. This is likely used for storing potential customer contact data collected by field or sales teams.

Code detail: route `/businesscarddetails/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### MO Performance
Shows MO performance from the customer management context. This can be used to evaluate customer acquisition, follow-up, or field activity.

Code detail: route `the matching menu route` is exposed from the main layout menu. Customer screens use search models and CRUD-style views to manage customer profiles, notes, documents, segmentation, referrals, refunds, coupons, and corporate account data.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Employee Map
Provides a map view of employee or field-user locations. This supports field monitoring and territory-level visibility.

Code detail: route `/user/mapviews` is exposed from the main layout menu. Customer screens use search models and CRUD-style views to manage customer profiles, notes, documents, segmentation, referrals, refunds, coupons, and corporate account data.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Customer Refund
Manages customer refund records. This supports finance workflows where money needs to be returned or adjusted for customers.

Code detail: route `/customer-refund/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Customer Coupons
Manages coupons connected to customers. This can include coupon creation, allocation, tracking, and usage review.

Code detail: route `/coupons/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Customer Referrals
Tracks customer referral records. This supports referral programs and customer acquisition tracking.

Code detail: route `/customerreferral/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Referral Master
Configures referral program master data. This defines referral rules or reference values used by the referral workflow.

Code detail: route `/referralmaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

### Invoice Number Sequence
Manages invoice numbering rules or sequences. This is important for finance compliance and consistent invoice generation.

Code detail: route `/invoicenumbersequence/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Customers area. Users would use it for onboarding, account maintenance, sales follow-up, customer finance controls, and relationship-management tasks.

## Transporter

### Vendor Dashboard
Provides a dashboard for transporter/vendor onboarding and activity. It gives users a summary view before drilling into vendor records.

Code detail: route `/transporterinfo/dashboard` is exposed from the main layout menu. `TransporterinfoController::actionDashboard()` calculates vendor counts across ops and finance verification states, including pending and completed KYC buckets.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Vendor List
Lists and manages transporter/vendor records. This supports vendor profile maintenance, review, and operational usage.

Code detail: route `/transporterinfo/vendorindex` is exposed from the main layout menu. `TransporterinfoController::actionVendorindex()` uses `TransporterInfoSearch` and renders the vendor index view for transporter/vendor records.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### KYC Pending Vendors
Shows vendors whose KYC verification is pending. This helps onboarding teams prioritize incomplete vendor compliance tasks.

Code detail: route `/transporterinfo/pendinglist` is exposed from the main layout menu. `TransporterinfoController::actionPendinglist()` calls `vendorpendinglist()` on the search model and renders the pending list view.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Create Verified Vendor
Allows creation of a vendor that is treated as verified. This supports controlled vendor onboarding where verification has already been completed externally or by an admin.

Code detail: route `/transporterinfo/vendorcreate` is exposed from the main layout menu. `TransporterinfoController::actionVendorcreate()` saves transporter info, contact, bank, prepaid card, and upload-document data through `TransporterInfo::saveTransporterInfo()`.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### New Vendor Request
Creates or reviews a new vendor onboarding request. This supports the intake stage before approval or verification.

Code detail: route `/transporterinfo/create` is exposed from the main layout menu. Transporter screens manage vendors, vehicles, routes, drivers, KYC, contracts, attendance, restrictions, and incentives that support supply-side operations.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Vehicle
Manages fleet/vendor vehicle records. This can include vehicle details, documents, ownership/vendor mapping, and operational availability.

Code detail: route `/driver/fleetdriver` is exposed from the main layout menu. `DriverController::actionFleetdriver()` reuses `DriverSearch` in fleet mode and renders `fleetdriver`, including notes support for fleet vehicle/driver records.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Route Master
Defines route master data. This supports standardized route setup for fleet operations and rate planning.

Code detail: route `/fleetroutemaster/index` is exposed from the main layout menu. Transporter screens manage vendors, vehicles, routes, drivers, KYC, contracts, attendance, restrictions, and incentives that support supply-side operations.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Routes
Manages actual route records. This allows users to maintain route-level operational data used in bookings or fleet workflows.

Code detail: route `/fleetroutes/index` is exposed from the main layout menu. Transporter screens manage vendors, vehicles, routes, drivers, KYC, contracts, attendance, restrictions, and incentives that support supply-side operations.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Route Rates
Configures route-wise rates. This helps price fleet movements based on defined routes.

Code detail: route `/fleetrouterate/index` is exposed from the main layout menu. Transporter screens manage vendors, vehicles, routes, drivers, KYC, contracts, attendance, restrictions, and incentives that support supply-side operations.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Contract Vehicles
Manages vehicles under contract. This supports dedicated or contracted fleet operations with fixed vehicle relationships.

Code detail: route `/contractvehicleinfo/index` is exposed from the main layout menu. Transporter screens manage vendors, vehicles, routes, drivers, KYC, contracts, attendance, restrictions, and incentives that support supply-side operations.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Contract Vehicles Utilization
Tracks how contract vehicles are being used. This helps monitor productivity, idle time, and contract value realization.

Code detail: route `/contractvehicleinfo/contractvehicleutilization` is exposed from the main layout menu. Transporter screens manage vendors, vehicles, routes, drivers, KYC, contracts, attendance, restrictions, and incentives that support supply-side operations.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Contract Vehicle Attendance
Maintains attendance or availability status for contract vehicles. This supports daily operational planning.

Code detail: route `/contractvehicleattendance/index` is exposed from the main layout menu. Transporter screens manage vendors, vehicles, routes, drivers, KYC, contracts, attendance, restrictions, and incentives that support supply-side operations.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Driver Details
Manages driver records and documents. This is the main driver profile management area.

Code detail: route `/driver/index` is exposed from the main layout menu. `DriverController::actionIndex()` uses `DriverSearch`, optional preferred-location permission behavior, default sorting by newest driver, and AJAX-loaded notes for selected drivers.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Pending Activation
Shows drivers awaiting activation. This helps onboarding teams review and approve drivers before they can operate.

Code detail: route `/driver/pendingactivation` is exposed from the main layout menu. `DriverController::actionPendingactivation()` uses `DriverSearch::searchActivation()` and renders `driver_activation` for drivers waiting to be enabled.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Trip Sheet
Manages driver trip sheets. This supports trip-level documentation, settlement, and operational records.

Code detail: route `/drivertripsheet/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Block Driver Customer
Blocks specific driver-customer combinations. This supports service quality controls and customer-specific restrictions.

Code detail: route `/blockdrivercustomer/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Driver Blocks
Manages driver block rules or statuses. This helps prevent unsuitable or restricted drivers from being used.

Code detail: route `/driver-blocks/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Manage Quiz
Manages quiz questions for drivers. This may support training, compliance, or driver knowledge checks.

Code detail: route `/questions/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Driver Answers
Reviews submitted quiz answers from drivers. This helps evaluate training completion or response quality.

Code detail: route `/driveranswers/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

### Driver Rewards Points
Manages driver reward points. This supports incentive or loyalty-style programs for drivers.

Code detail: route `/rewards-points/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Transporter area. Users would use it to onboard and manage supply, confirm vendor/driver readiness, maintain fleet data, and enforce operational controls.

## Notification

### Sourcemessage
Manages source text/messages used for localization or templates. This is the base content before translation.

Code detail: route `/sourcemessage/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Notification area. Users would use it to maintain consistent communication content and monitor messages sent through app, SMS, WhatsApp, or localization flows.

### Translation
Manages translated message content. This supports multilingual app or notification experiences.

Code detail: route `/translation/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Notification area. Users would use it to maintain consistent communication content and monitor messages sent through app, SMS, WhatsApp, or localization flows.

### Manage Template
Manages language/message templates. These templates can be reused across notifications or app messaging.

Code detail: route `/language/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Notification area. Users would use it to maintain consistent communication content and monitor messages sent through app, SMS, WhatsApp, or localization flows.

### Message List
Shows FCM or push notification status records. This helps users verify delivery, failures, and notification history.

Code detail: route `/fcmnotificationstatus/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Notification area. Users would use it to maintain consistent communication content and monitor messages sent through app, SMS, WhatsApp, or localization flows.

### Notification Template
Configures reusable notification templates. This allows standardized communication for operational events.

Code detail: route `/notificationmessage/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Notification area. Users would use it to maintain consistent communication content and monitor messages sent through app, SMS, WhatsApp, or localization flows.

### Feedback
Shows user support feedback. This helps support or product teams review user-submitted issues and comments.

Code detail: route `/support-feedback/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Notification area. Users would use it to maintain consistent communication content and monitor messages sent through app, SMS, WhatsApp, or localization flows.

### Announcements
Creates and manages app announcements. This supports broadcasting important messages to users.

Code detail: route `/appannouncement/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Notification area. Users would use it to maintain consistent communication content and monitor messages sent through app, SMS, WhatsApp, or localization flows.

## Reports

### Invoice Generated
Reports invoices that have been generated. This helps finance teams validate billed bookings.

Code detail: route `/invoicestate/invoicegenerated` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

### Invoice Cancelled
Reports invoices that have been cancelled. This supports cancellation review and audit workflows.

Code detail: route `/invoicestate/invoicecancelled` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

### Invoice Not Generated
Reports bookings or records where invoices have not yet been generated. This helps identify billing backlog.

Code detail: route `/invoicestate/invoicenotgenerated` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

### Freight Index
Provides freight or pricing index reporting. This can support pricing analysis or market/rate review.

Code detail: route `/freightindex/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

### GST Utility Report
Provides GST-related reporting/export support. This is useful for tax, accounting, and compliance activities.

Code detail: route `/gstandnongstreport/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

### Vendor Ledger
Shows vendor payment or ledger information. This supports vendor reconciliation and finance review.

Code detail: route `/ccavenuebreakups/vendorpayment` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

### CCAvenue Payment Breakups
Reports payment breakups for CCAvenue transactions. This helps reconcile payment gateway collections.

Code detail: route `/ccavenuebreakups/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

### Invoiced MIS Report
Provides an MIS report focused on invoiced transactions. This combines management reporting with billing status.

Code detail: route `/reports/invoicemisreport` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Reports area. Users would use it for finance review, operational analysis, statutory/tax support, and management reporting.

## More / Master Management

### Intercity Price
Configures outstation/intercity pricing. These rates are used for booking cost calculation and quote generation.

Code detail: route `/pricelist/intercityindex` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Length
Maintains vehicle length master data. This supports vehicle selection and pricing configuration.

Code detail: route `/vehiclelengthmaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Tonnage
Maintains tonnage master data for bookings or vehicles. This helps standardize load capacity values.

Code detail: route `/bookingtonnagemaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Event Logging
Shows or manages operational event logs. This supports tracking important system or booking lifecycle events.

Code detail: route `/eventlogging/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### OutStation Promo
Manages promotions for outstation services. This supports discounting or campaign-driven pricing.

Code detail: route `/outstationpromotion/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Communication
Manages communication records. This may include booking/customer/driver communication history or templates.

Code detail: route `/communication/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### POD Tracking
Manages Proof of Delivery tracking records. This helps operations and billing teams monitor POD completion.

Code detail: route `/podtracking/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Complaints
Manages complaints and complaint handling. This supports customer service and issue resolution workflows.

Code detail: route `/complaintshandling/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Drivers Response
Reviews driver responses, likely from WhatsApp or messaging flows. This helps operations follow up on driver confirmations or replies.

Code detail: route `/whatsappresponse/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Optin Numbers
Manages opt-in numbers for WhatsApp or notification workflows. This supports compliant communication with users.

Code detail: route `/whatsappoptstatus/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Push Notification
Sends or manages push notifications. This is used for app-based communication campaigns or alerts.

Code detail: route `/pushnotificationnew/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Cross Selling Product
Manages products used for cross-selling. This supports promoting additional services or products to customers/users.

Code detail: route `/crosssellingproduct/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Event Type
Configures calendar or global event types. This supports structured calendar entries and operational scheduling.

Code detail: route `/globaleventtypes/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Calendar
Manages operational calendar entries. This can include holidays, working days, or special events.

Code detail: route `/globalcalendar/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Price Groups
Groups pricing rules together. This helps maintain pricing variations by customer, geography, package, or booking type.

Code detail: route `/pricegroup/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Price List
Maintains local pricing records. These are likely used for local booking cost calculations.

Code detail: route `/pricelist/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Manage Package
Manages hourly or package-based booking plans. This supports predefined packages for local services.

Code detail: route `/hourlypackage/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Vehicle Manufacturer
Maintains vehicle manufacturer master data. This standardizes vehicle records.

Code detail: route `/vehiclemanufacturer/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Vehicle Master
Maintains vehicle model/master data. This supports vehicle selection and driver/fleet setup.

Code detail: route `/vehiclemaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Truck Body
Maintains truck body type data. This supports matching load requirements to suitable vehicles.

Code detail: route `/truckbody/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Booking Source
Maintains booking source values. This helps classify where bookings originate from, such as app, call center, or partner channels.

Code detail: route `/bookingsource/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Filter Master
Configures booking filter master values. This supports search/filter behavior in booking workflows.

Code detail: route `/bookingfiltermaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Booking Calls
Tracks booking-related calls. This supports call-center operations and booking follow-up.

Code detail: route `/bookingphonecall/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Booking Materials
Manages material or goods data for bookings. This helps classify shipment contents.

Code detail: route `/booking-materials/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### New Customer Coupons
Manages coupons intended for new customers. This supports acquisition and promotional campaigns.

Code detail: route `/customercoupons/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Manage Feeds
Manages feed content. This may be used for driver/customer app content or operational messages.

Code detail: route `/feeds/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Driver Type
Configures driver type master values. This helps classify drivers by business or operational category.

Code detail: route `/drivertype/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Driver Reject Reason
Configures standard driver rejection reasons. This supports consistent onboarding or activation decisions.

Code detail: route `/driverrejectreason/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Customer Company Type
Configures customer company classifications. This supports segmentation and reporting.

Code detail: route `/customercompanytype/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Payment Grace Period
Configures allowed payment grace periods. This supports customer credit and collections workflows.

Code detail: route `/paymentgraceperiod/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Address Type
Configures customer address types. This standardizes pickup, drop, billing, and other address categories.

Code detail: route `/customeraddresstype/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Industry Segment
Configures industry segment master values. This supports customer segmentation and reporting.

Code detail: route `/industorysegment/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Customer Reject Reason
Configures standard customer rejection reasons. This supports consistent customer onboarding decisions.

Code detail: route `/customerrejectreason/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Customer Source
Configures customer acquisition/source values. This supports lead tracking and business analytics.

Code detail: route `/customersource/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Verification Status
Configures customer verification statuses. This supports customer onboarding and compliance state tracking.

Code detail: route `/customerverificationstatus/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Customer Document Type Master
Configures document types required from customers. This supports structured document uploads and verification.

Code detail: route `/customeruploadfiletype/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Customer Block Reason
Configures reasons for blocking customers. This supports consistent account restriction workflows.

Code detail: route `/customerblockreason/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Email Templates
Manages system email templates. This supports standardized email communication.

Code detail: route `/emailtemplatemaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### States
Maintains state master data. This supports addresses, routes, taxes, and geography-based configuration.

Code detail: route `/statemaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Cities
Maintains city master data. This supports serviceability, pricing, routing, and address capture.

Code detail: route `/citymaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Regions
Maintains region master data. This supports geographic grouping and operational assignment.

Code detail: route `/regionmaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Hub Details
Maintains hub/location details. Hubs can be used for operations, routing, assignment, or reporting.

Code detail: route `/hub/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Insurance Details
Maintains insurance company/details master data. This supports driver, vehicle, or shipment insurance workflows.

Code detail: route `/insurancemaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Digital Signature
Manages digital signatures used on documents or invoices. This supports authenticated document generation.

Code detail: route `/digitalsignature/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Tally Integration Customer
Tracks or configures customer synchronization with Tally. This supports accounting integration.

Code detail: route `/tallyintegrationstatus/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Tally Integration Driver
Tracks or configures driver/vendor synchronization with Tally. This supports accounting integration for payouts and ledgers.

Code detail: route `/tallyintegrationstatus/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Goods Category
Configures goods categories. This standardizes shipment classification.

Code detail: route `/goodscategory/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Payment Mode
Configures supported payment modes. This supports payment capture and accounting consistency.

Code detail: route `/paymentmodemaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Company Details
Maintains company account/legal details. This can be used in invoices, documents, and accounting records.

Code detail: route `/companyaccountdetails/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Holidays
Maintains the holiday calendar. This supports operations, billing calendars, and scheduling logic.

Code detail: route `/holidays/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Contract Group
Configures contract grouping. This supports contract vehicle/customer grouping and related reporting.

Code detail: route `/contractgroup/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Settings Master
Manages setting categories or setting types. This supports system-level configurable behavior.

Code detail: route `/settingtype/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Language Master
Manages supported languages. This supports localization and multilingual templates.

Code detail: route `/languagemaster/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Party Settings
Configures party-related settings. This may affect customer/vendor/driver behavior depending on party type.

Code detail: route `/settings/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Performance
Provides performance-related configuration or reporting. This may be used to track operational or user performance.

Code detail: route `/performance/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Product Dispute
Manages product or service disputes. This supports dispute tracking outside the corporate invoice dispute flow.

Code detail: route `/productdispute/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

### Cross Lead
Manages cross-lead records. This supports cross-selling or lead-sharing workflows.

Code detail: route `/crosslead/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the More / Master Management area. Users would update these records carefully because downstream screens rely on this data for dropdowns, pricing, validation, reporting, and automated calculations.

## Payments

### Payments
Manages payment records and collections. This is the primary area for viewing and processing payment transactions.

Code detail: route `/payment/index` is exposed from the main layout menu. `PaymentController::actionIndex()` is the main payment listing, with create/update/bulk-payment actions and helper endpoints for party lookup and pending invoice amounts.

Operationally, this item belongs to the Payments area. Users would use it for collection tracking, reconciliation, payout/account adjustments, and credit/debit corrections.

### Payment Automation
Supports automated payment processing or review. This is likely restricted to finance leadership or specific finance roles.

Code detail: route `/paymentautomation/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Payments area. Users would use it for collection tracking, reconciliation, payout/account adjustments, and credit/debit corrections.

### Corporate Payments
Manages corporate quick payments. This supports B2B collection workflows and corporate payment tracking.

Code detail: route `/corporatequickpayment/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Payments area. Users would use it for collection tracking, reconciliation, payout/account adjustments, and credit/debit corrections.

### Customer Journal
Maintains customer accounting journal entries. This supports adjustments, reconciliation, and account history.

Code detail: route `/customer-journal/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Payments area. Users would use it for collection tracking, reconciliation, payout/account adjustments, and credit/debit corrections.

### Driver/Vehicle Journal
Maintains driver or vehicle journal entries. This supports payout adjustments, vehicle-related accounting, and reconciliation.

Code detail: route `/driver-journal/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Payments area. Users would use it for collection tracking, reconciliation, payout/account adjustments, and credit/debit corrections.

### Cashback Journal
Tracks cashback-related journal entries. This supports promotional accounting and cashback reconciliation.

Code detail: route `/cashbackjournal/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Payments area. Users would use it for collection tracking, reconciliation, payout/account adjustments, and credit/debit corrections.

### Credit/Debit Note
Creates and manages credit notes and debit notes. This supports invoice adjustments, corrections, and finance approvals.

Code detail: route `/creditdebitnote/index` is exposed from the main layout menu. `CreditdebitnoteController` is a Yii CRUD controller and the form supports credit/debit note type selection plus GST/remarks-related fields in the view.

Operationally, this item belongs to the Payments area. Users would use it for collection tracking, reconciliation, payout/account adjustments, and credit/debit corrections.

## Admin

### System Settings
Manages core application settings. This is an admin-level area for controlling configurable system behavior.

Code detail: route `/systemsettings/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Admin area. Only privileged users should use these screens because they can change access, settings, and core operating behavior.

### Admin Users
Manages admin user accounts. This supports creating, updating, and maintaining internal user access.

Code detail: route `/user/index` is exposed from the main layout menu. In code, this appears as a route-backed Yii screen with a controller, model/search model, and matching views under `views/`. For master-data entries, the common behavior is a searchable index plus create, update, view, and delete-style actions where permissions allow them.

Operationally, this item belongs to the Admin area. Only privileged users should use these screens because they can change access, settings, and core operating behavior.

### Permissions
Manages roles and permission assignments. This controls which menus and actions each user can access.

Code detail: route `/admin/assignment` is exposed from the main layout menu. The menu points to the Yii RBAC admin assignment module, used to map users to roles/permissions that control menu visibility and controller access.

Operationally, this item belongs to the Admin area. Only privileged users should use these screens because they can change access, settings, and core operating behavior.

## Account

### Change Password
Allows the logged-in user to update their password. This supports account security and credential maintenance.

Code detail: route `/user/changepassword` is exposed from the main layout menu. Account entries are user-session utilities rather than business workflows.

Operationally, this item belongs to the Account area. Every logged-in user may use these controls to maintain access security or leave the session.

### Logout
Ends the current user session and exits the admin application.
Code detail: route `/site/logout` is exposed from the main layout menu. Account entries are user-session utilities rather than business workflows.

Operationally, this item belongs to the Account area. Every logged-in user may use these controls to maintain access security or leave the session.

