- Remote Key Injection - Project
	- P2PE‑Compliant Remote Key Injection (RKI) / Remote Key Loading (RKL) Architecture
	  collapsed:: true
		- collapsed:: true
		  1. Terminal Serial Number as the P2PE Device Identifier
			- The terminal serial number is the canonical device identifier.
			- P2PE solutions must be able to identify and track each POI device uniquely, typically by serial number. Serial numbers are used to maintain lifecycle records (deployment, key injection, revocation, decommissioning) in TMS. [[listings.pcisecuritystandards](https://listings.pcisecuritystandards.org/documents/P2PE_HW-Hybrid_Solution_PROV_ReportingInstructions_v1.1.pdf)]
		- collapsed:: true
		  2. Certificate Binding and Trust Establishment
			- The terminal generates a PKI key pair locally and stores the private key in a secure area (secure element / SRED‑enabled POI).
			- The terminal exports a **CSR** containing the public key and the terminal serial number (typically as the **Common Name (CN)** or in a controlled mapping field). A **P2PE‑validated CA** signs the CSR to create a **device certificate**.
			- Binding the serial number to the certificate (e.g., as the CN or in a controlled mapping table) ensures that every cryptographic operation can be audited per device, which is a core P2PE control. [listings.pcisecuritystandards](https://listings.pcisecuritystandards.org/documents/P2PE_HW-Hybrid_Solution_PROV_ReportingInstructions_v1.1.pdf)
			- The **Device Certificate** (with serial bound via CN or issuance records) is used to
			  collapsed:: true
				- Authenticate the terminal to the KDH (via TMS).
				- Authorize key injection operations.
			- This satisfies several P2PE principles
			  collapsed:: true
				- **Device authentication** using certificates signed by a trusted, P2PE‑validated CA.
				- **Unique cryptographic identity per device**, tied to a tracked serial number.
				- **Lifecycle control**: certificate validity, renewal, and revocation are directly linked to the device’s lifecycle state; any compromise invalidates trust and blocks key injection until remediated.
				- **Key pair uniqueness**: P2PE requires that key pairs are not reused across certificate renewals; each certificate should correspond to a unique key pair. The “private key generated locally per device” approach supports this if new key generation is enforced on renewal.
		- collapsed:: true
		  3. Separation of Roles: Trust Authority, TMS (rki‑core), KDH, HSM, and Key Receiving Device
			- 3.1 Trust Authority (CA)
			  collapsed:: true
				- Validates that a certificate requestor is authorized to request a certificate for a given terminal serial number, Signs CSRs to issue **device certificates** that bind the public key to the terminal serial number (CN or mapped field), Maintains certificate lifecycle: renewal, revocation (CRL/OCSP), and expiry handling, Protects its private keys in an HSM and enforces dual control for critical CA functions.
			- 3.2 TMS (rki‑core Module) – Orchestrator
			  collapsed:: true
				- The **Terminal Management System (TMS)** module that bridges Terminals (KRDs), The CA (Trust Authority), and The KDH (and indirectly the HSM).
				- Ensures the serial number is the canonical identifier across all systems. Maintains a registry of terminals by **serial number** and it's status (active, blocked, decommissioned)
				- **Certificate workflow**: Receives CSRs from terminals, Validates that the serial number matches inventory and that the device state allows certificate issuance, Forwards CSRs to the CA, Receives issued certificates and returns them to terminals and finally, Stores certificate metadata/fingerprints linked to serial numbers.
				- **RKI Session coordination**
				  collapsed:: true
					- Orchestrates communication via a secure channel(mTLS) to KDH and KRD.
					- Takes Key injection request from Terminal and Validates terminal device certificates (chain to trusted P2PE CA, not expired/revoked, CN matches serial in inventory).
					- Enforces policy on whether the device is allowed to request keys and which key profiles are permitted.
					- Case 1: Terminal-Initiated RKI
						- Relays PEDI/PEDK requests to the KDH and relays TR‑34 key blocks back to the terminal.
						- Relays and Records PEDV acknowledgements and updates device state.
					- Case 2: TMS/Host-Initiated RKI
						- Initiates PEDI/PEDK requests on behalf of the terminal to the KDH and relays TR‑34 key blocks back to the terminal.
						- Relays and Records PEDV acknowledgements and updates device state.
				- **Audit & lifecycle enforcement**: Logs all certificate and key operations per serial number. Blocks key injection when the certificate is expired/revoked or the device is flagged as compromised or decommissioned and Supports renewal, re‑keying, and decommission workflows.
			- 3.3 Key Distribution Host (KDH)
			  collapsed:: true
				- The logical **key distribution authority** maintains **key profiles** and policies (key types, versions, KBN/KVC, algorithms, usage constraints).
				- Processes key requests from TMS, Validates that the requesting terminal (identified by certificate/serial) is authorized. Applies business rules (site, merchant, acquirer, etc.).
				- collapsed:: true
				  
				  Constructs **TR‑34 key blocks**:
					- Uses the KRD’s **public key** (from its certificate) to encrypt/wrap the key.
					- Signs the key block with the KDH signing key.
					- Includes header metadata (usage, exportability, version).
				- Interacts with the **HSM** by Requesting key generation or retrieval, Asks HSM to perform wrapping/signing operations, and never handles clear keys outside the HSM boundary.
			- 3.4 HSM (used by KDH)
			  collapsed:: true
				- A Secure Cryptographic Device that Securely stores KDH signing keys, KEKs, and optionally CA keys (logically separated), performs cryptographic operations, which are : Generate symmetric keys (DUKPT base, PIN, data, etc.), Wrap/encrypt keys for TR‑34 key blocks, and Sign key blocks.
			- 3.5 Key Receiving Device (KRD) – Payment Terminal
			  collapsed:: true
				- The **payment terminal / PED / POI** Generates its own key pair locally and keeps the private key protected. Creates and sends a CSR with its public key and serial number to TMS.
				- Enforces that keys are only loaded if the certificate is valid and chains to a trusted CA.
				- Supports key rotation, certificate renewal, and secure zeroization on tamper/decommission.
		- collapsed:: true
		  4. Overall Workflow
			- Step 1 – Terminal generates key pair and CSR
			  collapsed:: true
				- Generate a **PKI key pair** inside the secure element / SRED‑enabled POI.
				- Build a **CSR** including:
					- Public key.
					- **CN = terminal serial number** (or a value that the backend maps 1:1 to the serial).
				- Sign the CSR with the newly generated private key and Keep the private key inside the terminal; export only the CSR.
			- Step 2 – Terminal sends CSR to TMS (rki‑core)
			  collapsed:: true
				- Over an mTLS‑protected channel:
					- Terminal calls an endpoint such as:
						- `POST /rki/v1/devices/{serial}/certificate/request`
						- Body: `{ "csr": "<PEM CSR>", "serialNumber": "<SN>", "model": "...", "firmwareVersion": "..." }`
					- TMS validates:
						- mTLS client identity (if using client certs).
						- That the **serialNumber** in the payload matches:
							- The CN in the CSR.
							- A known device in inventory.
						- Device state (e.g., not decommissioned, not blocked).
			- Step 3 – TMS forwards CSR to the Trust Authority (CA)
			  collapsed:: true
				- TMS calls the CA’s issuance API (or uses SCEP/EST, depending on your CA):
					- Sends CSR + metadata (serial, device type, site, etc.).
					- CA performs its own checks:
						- Validates CSR signature.
						- Ensures the requested identity (CN/serial) is allowed.
						- Applies policy (validity period, extensions, etc.).
					- CA signs the CSR → **device certificate**.
					- CA returns the certificate (and optionally the chain) to TMS.
			- Step 4 – TMS stores mapping and returns certificate to terminal
			  collapsed:: true
				- collapsed:: true
				  
				  TMS stores:
					- Device record: serial → device certificate, issuance time, status etc. for later TMS-initiated RKI/RKL.
					- Audit log entry for certificate issuance, tracks certificate validity etc.
				- TMS responds to terminal:
					- `200 OK` with:
						- `certificate`: PEM/DER of the device cert.
						- `caChain`: intermediate/root as needed.
						- `validFrom`, `validTo`.
				- Terminal:
					- Imports the certificate into its secure store.
					- Associates it with the locally held private key.
					- Marks itself as “certificate‑ready” for RKL.
			- Step 5 - TMS-Initiated RKI with Terminal Readiness Check
			  collapsed:: true
				- In this model, the **TMS (rki-core)** stores the terminal's device certificate and executes the complete TR-34 protocol (PEDI/PEDK/PEDV) with the **KDH/HSM** on **behalf of the terminal**. The terminal remains the **Key Receiving Device (KRD)** in TR-34 terms. TR-34 explicitly allows the communication to be initiated by the KRD, KDH, **or some other component such as a terminal management system**. All RKL operations must occur over **mTLS-protected channels** and are logged per device serial number.
				- Step 5.1 – TMS Decides to Perform Key Injection
				  collapsed:: true
					- TMS determines that a key injection is required for a specific terminal, for example:
					  collapsed:: true
						- New device provisioning.
						- Scheduled key rotation.
						- Change of acquirer/merchant configuration.
						- Re-keying after a suspected compromise (with new keys).
					- TMS checks its inventory and policy:
					  collapsed:: true
						- Device serial number exists and is in an "active" state.
						- Device certificate is present in TMS records, Valid (not expired, not revoked).
						- Device is allowed to receive keys (not blocked, not decommissioned).
					- If any check fails, TMS aborts the RKI operation and logs the denial.
				- Step 5.2 – TMS Checks Terminal is Active and Sends RKI Initiate Command
				  collapsed:: true
					- TMS checks if the terminal is **active and reachable**, Terminal must have an active mTLS connection (persistent or polling).
					- TMS sends an **RKI initiate command** to the terminal over mTLS
					- Terminal validates the command is from an authorized TMS.
					- If terminal is not active or not ready:
					  collapsed:: true
						- TMS aborts the operation and logs the reason.
						- May schedule a retry.
				- Step 5.3 – Terminal Sends Acknowledgement of Readiness
				  collapsed:: true
					- Terminal responds to TMS with an **acknowledgement of readiness**:
					- Endpoint: Response to initiate command.
					- TMS validates:
						- The response is from the expected terminal.
						- The session nonce matches.
						- The terminal status is "ready".
				- Step 5.4 – TMS Establishes mTLS Session with KDH/HSM
				  collapsed:: true
					- TMS opens an **mTLS session** to the **KDH/HSM**:
					- TMS authenticates using its own service certificate.
					- KDH/HSM validates TMS identity and authorization.
				- Step 5.5 – TMS Sends PEDI (Identify) to KDH/HSM on Behalf of Terminal
				  collapsed:: true
					- TMS sends a **PEDI** command to KDH/HSM:
					- Includes:
						- Terminal's **device certificate** (stored in TMS).
						- Terminal serial number (CN or explicit field).
						- Optional metadata (application ID, key profile ID).
					- KDH/HSM validates:
						- Certificate chains to the trusted P2PE CA.
						- Certificate is not expired or revoked.
						- **CN matches the terminal serial number**.
						- Terminal is authorized for key injection.
					- If validation passes, KDH/HSM treats the session as **authenticated for that terminal** and returns a **session identifier** to TMS.
				- Step 5.6 – TMS Sends PEDK (Key Request) to KDH/HSM on Behalf of Terminal
				  collapsed:: true
					- TMS sends a **PEDK** command to KDH/HSM:
					- Specifies:
						- **Key type(s)** (e.g., DUKPT initial key, PIN key, data key).
						- **Key profile / version**.
						- **Desired key block format** (e.g., TR-34 variant, TR-31 inside TR-34).
						- **Target key slot**.
						- **Session identifier** (from Step 5.5).
					- KDH/HSM:
						- Verifies authorization policies for this device.
						- Generates or retrieves clear key material internally.
						- Wraps the key into a **TR-34 key block**:
							- Encrypted under a KEK associated with this device.
							- Signed by the KDH signing key.
							- Includes metadata (KBN, KVC, algorithm, usage).
						- Returns the **TR-34 key block** to TMS.
						  
						  ---
				- Step 5.7 – TMS Delivers TR-34 Key Block to Terminal
				  collapsed:: true
					- TMS sends the TR-34 key block to the terminal over mTLS
					- Terminal:
						- Validates the key block signature.
						- Unwraps/decrypts the key block using internal keys.
						- Loads the resulting key into the specified slot.
						- Performs integrity checks (e.g., KCV).
						  
						  ---
				- Step 5.8 – Terminal Sends Verification to TMS
				  collapsed:: true
					- Terminal sends an acknowledgement to TMS:
					- Endpoint: `POST /rki/v1/devices/{serial}/key-injection/result`
					- Body includes:
						- **Timestamp**.
						- **Outcome** (success/failure, error details).
						- **KCV confirmation**.
					- TMS:
						- Logs the result against the device serial number.
						- Updates lifecycle state (e.g., "keys loaded", "rotation complete").
				- Step 5.9 – TMS Sends PEDV (Verify) to KDH/HSM on Behalf of Terminal
				  collapsed:: true
					- TMS sends the **PEDV** command to KDH/HSM:
					- Includes:
						- **Session identifier**.
						- **Verification result** (success/failure).
						- **KCV confirmation**.
					- KDH/HSM:
						- Validates the PEDV response.
						- Marks the key as delivered and confirmed.
						- Logs the transaction.
					- TMS:
						- Records final outcome in audit log.
						- On failure:
							- May trigger retry logic.
							- May flag device for investigation.
					- The **terminal performs the final verification**, preserving the P2PE principle that the KRD must confirm successful key import.
					  
					  ---
		- collapsed:: true
		  5. Architecture
			- ```
			  ┌──────────┐              ┌──────────┐              ┌──────────────┐
			  │ TERMINAL │              │   TMS    │              │   KDH/HSM    │
			  │  (KRD)   │              │          │              │              │
			  └────┬─────┘              └────┬─────┘              └──────┬───────┘
			       │                         │                           │
			       │                         │  [1] Check device         │
			       │                         │  - cert valid             │
			       │                         │  - serial matches         │
			       │                         │  - policy allows          │
			       │                         │                           │
			       │  [2] RKI Initiate       │                           │
			       │<────────────────────────│                           │
			       │                         │                           │
			       │                         │                           │
			       │                         │                           │
			       │  [3] Ready Ack          │                           │
			       │────────────────────────>│                           │
			       │  {status: ready}        │                           │
			       │                         │                           │
			       │                         │  [4] mTLS session         │
			       │                         │──────────────────────────>│
			       │                         │                           │
			       │                         │  [5] PEDI                 │
			       │                         │──────────────────────────>│
			       │                         │  {terminal cert, serial}  │
			       │                         │                           │
			       │                         │  {session ID}             │
			       │                         │<──────────────────────────│
			       │                         │                           │
			       │                         │  [6] PEDK                 │
			       │                         │──────────────────────────>│
			       │                         │  {session ID, key types}  │
			       │                         │                           │
			       │                         │  {TR-34 key block}        │
			       │                         │<──────────────────────────│
			       │                         │                           │
			       │  [7] Deliver key        │                           │
			       │<────────────────────────│                           │
			       │  {key block, slot}      │                           │
			       │                         │                           │
			       │  Load key               │                           │
			       │  Verify KCV             │                           │
			       │                         │                           │
			       │  [8] Result             │                           │
			       │────────────────────────>│                           │
			       │  {success/fail, KCV}    │                           │
			       │                         │                           │
			       │                         │  [9] PEDV                 │
			       │                         │──────────────────────────>│
			       │                         │  {session ID, result}     │
			       │                         │                           │
			       │                         │  {confirmation}           │
			       │<────────[10] RKI Success│<──────────────────────────│
			       │                         │                           │
			  ```
		- collapsed:: true
		  6. Lifecycle Management
			- 6.1 Certificate Renewal
			  collapsed:: true
				- TMS monitors certificate expiry dates.
				- collapsed:: true
				  
				  Before certificate expiry:
					- collapsed:: true
					  
					  TMS triggers renewal:
						- Instructs terminal to generate a **new key pair** (no reuse of old keys).
						- Terminal creates a new CSR with the same serial (CN) or updated mapping.
						- Terminal sends CSR to TMS → CA.
					- CA issues a new certificate with fresh validity dates.
					- TMS updates mappings; terminal imports the new certificate.
			- 6.2 Certificate Revocation
			  collapsed:: true
				- collapsed:: true
				  
				  Triggered by:
					- Suspected or confirmed compromise.
					- Device loss, tamper, or decommission.
				- collapsed:: true
				  
				  Actions:
					- TMS marks device as “blocked” or “decommissioned” in inventory.
					- TMS requests CA to revoke the certificate (adds serial to CRL / OCSP).
					- KDH denies any further PEDI/PEDK operations for that serial.
					- Any attempt to inject keys is denied until compliance is restored.
			- 6.3 Key Rotation
			  collapsed:: true
				- Periodic or event‑driven key rotation:
					- Initiated by TMS policy or schedule.
					- TMS starts a TMS‑initiated RKI flow (PEDI/PEDK/PEDV on behalf of terminal) with new key material.
					- Old keys are securely zeroized according to policy.
			- 6.4 Decommissioning
			  collapsed:: true
				- collapsed:: true
				  
				  When a terminal is retired:
					- TMS marks device as “decommissioned”.
					- TMS requests CA to revoke the certificate.
					- KDH removes or disables key profiles for that serial.
					- Terminal performs secure zeroization of all sensitive keys (often triggered by a TMS command or local policy).
		- collapsed:: true
		  7. Audit and Logging Requirements
			- To support P2PE compliance, the following must be logged and retained per organizational and P2PE policy:
			  collapsed:: true
				- Certificate operations
				  collapsed:: true
					- CSR requests, certificate issuances, renewals, revocations.
					- Associated serial numbers, timestamps, requestors, outcomes.
				- RKI operations
				  collapsed:: true
					- Every PEDI/PEDK/PEDV session:
						- Terminal serial number.
						- Timestamp.
						- Key profile/version requested.
						- Outcome (success/failure, error codes).
					- Any policy denials (e.g., blocked device, expired certificate).
				- Lifecycle state changes
				  collapsed:: true
					- Activation, blocking, decommissioning.
					- Tamper events, zeroization events.
				- Logs must be:
				  collapsed:: true
					- Protected against tampering and unauthorized access.
					- Correlated by **serial number** to provide a complete device history. [[listings.pcisecuritystandards](https://listings.pcisecuritystandards.org/documents/P2PE_HW-Hybrid_Solution_PROV_ReportingInstructions_v1.1.pdf)]
				- collapsed:: true
				  
				  Log **who/what initiated** each RKI operation:
					- “TMS‑initiated” vs “terminal‑initiated”.
					- User or system account that triggered the TMS action (if applicable).
				- collapsed:: true
				  
				  Log:
					- TMS decision logic (e.g., “scheduled rotation”, “provisioning”, “policy change”).
					- Any manual overrides or operator actions.
					- This strengthens auditability and supports P2PE requirements for traceability and accountability.
- Implementation in Phases
	- Phase 1 : Emulated Terminal with TMS(rki-module) initiated RKI
	- Phase 2 : TMS to the Terminal
	- Integration : E2E RKI/RKL Integration