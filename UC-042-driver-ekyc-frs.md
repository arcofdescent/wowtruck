# Functional Requirements Specification

## UC-042: Automated Driver e-KYC via ULIP

### 1. Purpose

The system shall allow authorized fleet users and automated operational processes to verify a driver's Driving Licence details through ULIP/Sarathi before the driver is allowed to participate in fleet dispatch operations.

The verification shall confirm that the Driving Licence exists, is valid, belongs to the expected driver, and authorizes the driver for the required commercial or transport vehicle classes.

### 2. Scope

This use case covers automated Driver e-KYC verification using Driving Licence number, date of birth, and expected driver name.

The use case includes:

- Capturing driver verification details.
- Validating the submitted Driving Licence information.
- Checking the licence record against Sarathi data available through ULIP.
- Matching the submitted driver name against the official licence holder name.
- Checking whether the driver is permitted to operate commercial or transport vehicles.
- Updating the driver's verification status.
- Displaying successful, pending, and failed verification outcomes to the user.
- Preventing duplicate verification attempts for the same user action.
- Maintaining traceability for compliance and audit review.

The use case does not include vehicle RC validation, DigiLocker fallback verification, payment/billing reconciliation, or manual back-office approval unless added as separate requirements.

### 3. Actors

| Actor | Description |
| --- | --- |
| Fleet Operations Manager | Initiates or reviews driver e-KYC verification before assigning the driver to dispatch operations. |
| System Automated Job | Initiates verification automatically as part of onboarding, compliance refresh, or operational readiness workflows. |

### 4. Preconditions

- The driver profile must already exist in the system.
- The user or automated job must be authorized to perform driver verification.
- The driver must provide a Driving Licence number, date of birth, and expected full name.
- ULIP/Sarathi verification services must be available for automated lookup.

### 5. Trigger

The use case starts when an authorized fleet user or automated system process submits a driver for e-KYC verification.

### 6. Main Functional Flow

1. The system captures the driver's Driving Licence number, date of birth, and expected driver name.
2. The system checks that all mandatory fields are present and formatted correctly.
3. The system sends the licence lookup request to ULIP/Sarathi.
4. The system receives the official licence record, including licence holder name, licence validity, and authorized vehicle classes.
5. The system confirms that the licence record is active and valid.
6. The system compares the expected driver name with the official licence holder name.
7. The system calculates a name match confidence score.
8. The system checks whether the licence allows commercial or transport vehicle operation.
9. If all checks pass, the system marks the driver profile as verified.
10. The system displays the verified status, licence validity, permitted vehicle classes, and name match confidence score.
11. The system unlocks the driver's eligibility for asset dispatch workflows.

### 7. Alternate and Exception Flows

#### 7.1 Driving Licence Record Not Found

Condition: No matching Driving Licence record is found in Sarathi.

System behavior:

- The system shall reject the verification attempt.
- The system shall show the failure reason as `DL_RECORD_NOT_FOUND`.
- The system shall display the message: "No matching DL found in Sarathi records."
- The driver shall remain in an unverified state.
- Asset dispatch access shall remain locked.

#### 7.2 Name Match Mismatch

Condition: The expected driver name does not sufficiently match the official Sarathi name.

System behavior:

- The system shall reject the verification attempt.
- The system shall show the failure reason as `NAME_MISMATCH`.
- The system shall display the name match score and the official name returned by Sarathi.
- The driver shall remain in an unverified state.
- Asset dispatch access shall remain locked.

#### 7.3 Invalid Vehicle Class

Condition: The Driving Licence is valid, but the authorized vehicle classes do not permit commercial or transport fleet operation.

System behavior:

- The system shall reject the verification attempt.
- The system shall show the failure reason as `INVALID_VEHICLE_CLASS`.
- The system shall display the vehicle classes available on the licence.
- The driver shall remain in an unverified state for commercial dispatch.
- Asset dispatch access shall remain locked.

#### 7.4 Duplicate Verification Attempt

Condition: The same verification action is submitted more than once within a short time window.

System behavior:

- The system shall detect the duplicate attempt.
- The system shall avoid performing duplicate external verification for the same action.
- The system shall return or display the existing verification result when available.
- The system shall prevent duplicate government-gateway billing or repeated compliance events for accidental repeated clicks.

### 8. Business Rules

| Rule ID | Requirement |
| --- | --- |
| BR-042-01 | The Driving Licence number is mandatory. |
| BR-042-02 | The driver's date of birth is mandatory. |
| BR-042-03 | The expected driver name is mandatory and must be at least two characters. |
| BR-042-04 | The Driving Licence must exist in Sarathi records. |
| BR-042-05 | The Driving Licence must be active and valid on the verification date. |
| BR-042-06 | The official licence holder name must match the expected driver name with at least 80% confidence. |
| BR-042-07 | The licence must include a commercial or transport-authorized vehicle class, such as `TRANS` or `HMV`. |
| BR-042-08 | A driver shall not be marked as verified unless all licence validity, name match, and vehicle class checks pass. |
| BR-042-09 | Failed verification shall prevent the driver from being used in dispatch workflows requiring verified commercial eligibility. |
| BR-042-10 | Each verification attempt shall be traceable for audit and compliance purposes. |

### 9. User Interface Requirements

#### 9.1 Idle State

- The system shall show a driver e-KYC form.
- The form shall collect Driving Licence number, date of birth, and expected driver name.
- The system shall guide the user to enter the Driving Licence number in the required Indian DL format.
- The user shall be able to submit the verification request only when mandatory fields are provided.

#### 9.2 Processing State

- The system shall show that verification is in progress.
- The system shall indicate that the licence is being checked against ULIP/Sarathi.
- The system shall prevent repeated accidental submissions while verification is in progress.

#### 9.3 Verified State

- The system shall show a successful verification status.
- The system shall display the official driver name, licence validity, permitted vehicle classes, and name match score.
- The system shall indicate that asset dispatch options are now available for the verified driver.

#### 9.4 Failed State

- The system shall show the verification failure reason clearly.
- The system shall display whether the failure was due to missing licence record, name mismatch, or invalid vehicle class.
- The system shall indicate that dispatch access remains locked until the issue is resolved.

### 10. Data Captured

The system shall capture the following business data for each verification attempt:

- Driver identifier.
- Driving Licence number.
- Date of birth used for lookup.
- Expected driver name.
- Verification result.
- Official licence holder name, when available.
- Licence validity date, when available.
- Authorized vehicle classes, when available.
- Name match confidence score, when available.
- External verification reference, when available.
- Date and time of verification.
- User or system process that initiated the verification.
- Correlation or audit reference for traceability.

### 11. Functional Outcomes

#### Successful Outcome

- Driver verification status is updated to `VERIFIED`.
- Verified licence details are stored against the driver profile.
- Commercial or transport vehicle classes are recorded.
- Asset dispatch eligibility is unlocked for the driver.

#### Failed Outcome

- Driver verification status remains unverified or failed.
- The failure reason is stored and displayed.
- Asset dispatch eligibility remains locked.
- The user may review the issue and retry after correcting the driver data or resolving the compliance problem.

### 12. Compliance and Audit Requirements

- The system shall maintain a complete audit trail for every verification attempt.
- The audit trail shall include who or what initiated the verification, when it occurred, and what result was returned.
- The system shall preserve the verification reference received from the external verification source when available.
- The system shall support correlation of internal verification activity with external verification logs.
- The system shall prevent unnecessary duplicate external verification attempts for repeated submissions of the same action.

### 13. Open Functional Decisions

The following points require product or compliance confirmation before this use case is finalized:

- Should vehicle RC status checks through Vahan be included before assigning the verified driver to a specific vehicle?
- Should DigiLocker-backed e-KYC be used as a fallback if direct Sarathi lookup is unavailable or times out?
- What retention period should apply to verification audit records and external verification references?
