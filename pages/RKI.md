- Remote Key Injection - Project Inception
  collapsed:: true
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
- Detailed Technical Reference/Workflow [ Documentation Reference ](https://docs.futurex.com/)
  collapsed:: true
	- Phase A (Object Signing) → Phase B (RKI/RKL)
	- Referencing CryptoHub 7.3.0.x | v3 Mode 2 — One-Pass TR-34
	- ARCHITECTURE OVERVIEW
	  collapsed:: true
		- Three mTLS-secured connections underpin this entire workflow. All three must be established before Phase A or Phase B can operate.
		- ```
		  ┌─────────────────────────────────────────────────────────────────────┐
		  │                                                                     │
		  │  ┌──────────┐   Channel 1 (mTLS)     ┌──────────┐                   │
		  │  │ TERMINAL │◄──────────────────────►│   TMS    │                   │
		  │  │  (KRD)   │  Standard mTLS         │          │                   │
		  │  └──────────┘  (Terminal ↔ TMS)      └───┬──────┘                   │
		  │                                          │                          │
		  │                              Channel 2   │   Channel 3              │
		  │                         (mTLS Host API)  │   (REST/TLS)             │
		  │                                          ▼                          │
		  │                                   ┌──────────────┐                  │
		  │                                   │  CryptoHub   │                  │
		  │                                   │  RKMS / KDH  │                  │
		  │                                   │  + CA        │                  │
		  │                                   └──────────────┘                  │
		  │                                                                     │
		  └─────────────────────────────────────────────────────────────────────┘
		  
		  ```
		- | Channel | Parties | Carries | Protocol |
		  |---|---|---|---|
		  | **1** | Terminal ↔ TMS | RKI initiate, cert relay, key delivery, KCV result | Standard mTLS |
		  | **2** | TMS ↔ CryptoHub (RKMS/KDH) | RKLG, PEDI, PEDK, PEDV (Host API) | mTLS — Verify Peer = Enabled |
		  | **3** | TMS ↔ CryptoHub CA | CSR submission, cert retrieval, revocation (REST API) | REST over TLS |
	- PRE-PHASE 0 — mTLS Infrastructure Setup (One-Time)
	  collapsed:: true
		- All three channels must be established before any Phase A or Phase B operation can occur.
		- CHANNEL 1 — Terminal ↔ TMS (Standard mTLS)
		  collapsed:: true
			- This is a standard mutual TLS connection. Both sides hold X.509 certificates and validate each other during the handshake. The terminal's device certificate (issued in Phase A) serves as its TLS client certificate on this channel.
			- What each side needs
			  collapsed:: true
				- | Side | Certificate | Private Key | Trusted CA |
				  |---|---|---|---|
				  | **Terminal** | X.509 device cert (CN = serial, issued by CryptoHub CA in Phase A) | Generated on-device, non-extractable | TMS server CA |
				  | **TMS** | TLS server certificate | TMS private key | CryptoHub Issuing CA (same CA that signed terminal device certs) |
				- **Note:** The terminal's device certificate does not exist until Phase A runs for the first time. Channel 1 is therefore bootstrapped by Phase A — the first connection uses a pre-provisioned manufacturer certificate; subsequent connections use the CryptoHub-issued device certificate.
			- Standard mTLS Handshake: Terminal → TMS
			  collapsed:: true
				- ```
				  Terminal (Client)                         TMS (Server)
				  ─────────────────                         ────────────
				  1. TCP connect to TMS
				  
				  2. ClientHello
				   └── TLS version, cipher suites, random
				  
				                                          3. ServerHello
				                                             └── Selected cipher, random
				                                          4. Certificate
				                                             └── TMS TLS server cert
				                                          5. CertificateRequest
				                                             └── Requests client cert
				                                          6. ServerHelloDone
				  
				  7. Certificate
				   └── Terminal X.509 device cert
				       (CN = terminal serial)
				  8. ClientKeyExchange
				   └── Pre-master secret encrypted
				       with TMS public key
				  9. CertificateVerify
				   └── Signature over handshake
				       using terminal private key
				       (proves possession of private key)
				  10. ChangeCipherSpec
				  11. Finished (encrypted)
				  
				                                          12. Verify terminal cert chain
				                                              └── Against CRL Certificate CA
				                                                  in POS device group
				                                          13. Verify CN/serial in RKID
				                                          14. ChangeCipherSpec
				                                          15. Finished (encrypted)
				  
				  ══════════════════════════════════════════
				  mTLS session established — Channel 1 open
				  ══════════════════════════════════════════
				  ```
			- Supported TLS ciphers (verified from documentation):
			  collapsed:: true
				- | Cipher | TLS Version |
				  |---|---|
				  | `TLS_AES_256_GCM_SHA384` | TLSv1.3 |
				  | `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384` | TLSv1.2 |
				  | `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384` | TLSv1.2 |
				  | `TLS_DHE_RSA_WITH_AES_256_GCM_SHA384` | TLSv1.2 (strongest recommended) |
				  | `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` | TLSv1.2 |
				- *"It is recommended enabling only the strongest TLS ciphers that our applications can support. Unless our organization requires it, we disable the TLSv1.0 and TLSv1.1 protocols and the DES ciphers."*
		- CHANNEL 2 — TMS ↔ CryptoHub RKMS/KDH (mTLS, Host API)
		  collapsed:: true
			- This is the most security-critical channel. All Host API commands (RKLG, PEDI, PEDK, PEDV) and all TR-34 key material flow over it. CryptoHub enforces mTLS with **Verify Peer = Enabled**.
			- #### Step C2.1 — CryptoHub: Configure TLS Profile
			  collapsed:: true
				- **Navigation:**
				  ```
				  Settings → Networking → TLS Settings → Profiles → [ Add Profile ]
				  ```
				- | Field | Value | Description |
				  |---|---|---|
				  | **Name** | e.g., `TMS-mTLS-Profile` | Profile identifier |
				  | **Min Protocol** | TLS 1.2 | Minimum TLS version |
				  | **Max Protocol** | TLS 1.3 | Maximum TLS version |
				  | **Verify Peer** | **Enabled** | Enforces mTLS — CryptoHub requires the connecting TMS to present a valid certificate signed by one of the Trusted CAs |
				  | **PKI Mode** | **User Certificates** | Use your own certificate (previously imported via the Certificates menu) |
				  | **TLS Certificate** | CryptoHub server cert | Certificate CryptoHub presents to TMS |
				  | **Certificate Authorities — Trusted** | CA that signed TMS client cert | CryptoHub validates TMS cert against this CA |
				  | **Certificate Authorities — Auxiliary** | Intermediate CAs if needed | Builds chain for validation |
				  | **Ciphers** | `TLS_AES_256_GCM_SHA384`, `TLS_DHE_RSA_WITH_AES_256_GCM_SHA384` | Enable strongest supported ciphers |
				- > Source: [TLS settings](/CryptoHub/7.3.0.x/Administrator_guide/Settings/Networking/TLS_settings) — *"Verify Peer — Enabled: Enforces mutual authentication (MTLS). The HSM will require the connecting client to present a valid certificate signed by one of the CAs listed in the Trusted Certificate Authorities for this profile."*
			- #### Step C2.2 — CryptoHub: TLS Keys and Certificates
			  collapsed:: true
				- **Navigation:**
				  ```
				  Settings → Networking → TLS Settings → Keys → [ Add Key ]
				  Settings → Networking → TLS Settings → Certificates → [ Add Certificate Store ]
				  ```
				- **Keys page:** Select **[ Add Key ]** to create a new TLS private key. Select **[ Request Signing ]** from Actions to generate a CSR — submit this to the CA that TMS trusts to produce the CryptoHub server certificate.
				- **Certificates page:** Select **[ Add Certificate Store ]** to create a new store. Expand a store and select **[ Download Certificate ]** to add a certificate. Actions: **Edit**, **Delete**, **Connect TLS Profile**.
			- #### Step C2.3 — CryptoHub: Assign TLS Profile to Excrypt Port
			  collapsed:: true
				- **Navigation:**
				  ```
				  Settings → Networking → TLS Settings → Servers
				  ```
				- Assign the mTLS profile to the Excrypt (Host API) port:
				  
				  | Server type | Default port | Assignment |
				  |---|---|---|
				  | Excrypt (Host API) | 9000 | Assign `TMS-mTLS-Profile` |
				  | Management | 443 | Separate admin profile |
				  
				  > *"All TLS profiles and port values are initially preset to the default values."*
			- #### Step C2.4 — CryptoHub: Create TMS Client Application Identity
			  collapsed:: true
				- **Navigation:**
				  ```
				  Identity and Access → Applications & Partitions → Applications → [ + Add ]
				  ```
				- **Create Application wizard (verified from documentation):**
				  
				  | Step | Field | Value |
				  |---|---|---|
				  | **Basic Info** | Login Name | e.g., `TMS-Application` |
				  | **Basic Info** | Common Name | e.g., `Terminal Management System` |
				  | **Basic Info** | HSM Application | Enabled (hardened, stored on HSM) |
				  | **Basic Info** | Locked | Disabled (can log in) |
				  | **Partitions** | Assign partitions | Partitions granting RKI and key injection access |
				  | **Authentication** | Type | **API Key** or **TLS Certificate** |
				- **Authentication options (verified from documentation):**
					- **API Key** — after selecting **[ DEPLOY ]**, browser prompts to download a `.txt` file with the API key. Used with `RKLG [AP=<api-token>; HD=1]`
					- **TLS Certificate** — use the **TLS Provider** drop-down to select the TLS identity provider that references the CA which signed the TMS client certificate. Requires a TLS identity provider.
				- > Source: [Manage applications](/CryptoHub/7.3.0.x/Administrator_guide/Identity_and_access/How-tos/Manage_applications) — *"TLS Certificate: Use the TLS Provider drop-down menu and select the TLS provider to use. (Requires a TLS identity provider.)"*
			- #### Step C2.5 — TMS: Generate Keypair and Client Certificate
			  collapsed:: true
				- ```
				  1. TMS generates RSA-2048 keypair
				   └── Private key: stored securely in TMS, never leaves TMS
				  
				  2. TMS generates PKCS#10 CSR
				   └── Subject DN: CN=TMS-Application, O=<org>
				   └── Self-signed by TMS private key (proof of possession)
				  
				  3. CSR submitted to the CA that CryptoHub trusts
				   └── CA signs → issues TMS client certificate
				  
				  4. TMS stores: signed client cert + private key
				   └── Presented during TLS handshake to CryptoHub (Channel 2)
				  
				  5. TMS loads CryptoHub server CA cert as trusted anchor
				   └── Used to validate CryptoHub's server cert during handshake
				  ```
			- #### Step C2.6 — CryptoHub: KDH Signing Keypair and Certificate
			  collapsed:: true
				- The RKMS Series needs its own signing keypair and certificate. This is the `CC` token returned in PEDI — the terminal uses it to verify the `KA` signature on PEDK responses.
				- **Navigation:**
				  ```
				  Services → [ Deploy New Service ] → Excrypt Remote Key Loading
				  → Service Info → Choose KDH Certificate
				  ```
				- **KDH Certificate options (verified from documentation):**
				  
				  | Option | When to use |
				  |---|---|
				  | **Existing KDH Certificate** | Use an existing KDH cert already on CryptoHub |
				  | **New KDH Key Pair** | Generate a new RSA keypair for the KDH |
				- **New KDH Key Pair:**
				  
				  | Field | Value |
				  |---|---|
				  | **New KDH Name** | e.g., `RKMS-KDH-Signing` |
				  | **Modulus** | 2048 |
				  | **Exponent** | 65537 (0x10001) |
				- > *"A Public Key Infrastructure (PKI) key is not generated at this stage."* — generated when the service is deployed.
				- After deployment: KDH private key is hardware-protected, non-extractable. The KDH certificate is returned as `CC` in PEDI and used to sign `KA` in PEDK.
			- #### Step C2.7 — CryptoHub: KRD CA Configuration
			  collapsed:: true
				- The RKMS Series must trust the CA that signed the terminal device certificates.
				- **Navigation:**
				  ```
				  Services → Deployed Services → [Excrypt RKL Service]
				  → Service Info → Choose KRD CA
				  ```
				- **KRD CA options (verified from documentation):**
				  
				  | Option | When to use |
				  |---|---|
				  | **Use KDH CA to Authenticate KRD Certificates** | Same CA for both KDH and KRD |
				  | **Existing CA** | Use a separate CA already on CryptoHub |
				  | **New CA** | Upload Issuing CA cert (DER/PEM) + optional CRL |
				- > Source: [Configure service info](/CryptoHub/7.3.0.x/Administrator_guide/Key_injection/Remote_key_loading/Excrypt_RKL/Configure_service_info) — *"You must use a Key Distribution Handler (KDH) and a Key Receiving Devices (KRD) certificate authority (CA) to correctly configure the service."*
			- #### Step C2.8 — Standard mTLS Handshake: TMS → CryptoHub (Channel 2)
			  collapsed:: true
				- ```
				  TMS (Client)                              CryptoHub (Server, port 9000)
				  ────────────                              ─────────────────────────────
				  1. TCP connect to CryptoHub port 9000
				  
				  2. ClientHello
				   └── TLS version, cipher suites, random
				  
				                                          3. ServerHello
				                                             └── Selected cipher, random
				                                          4. Certificate
				                                             └── CryptoHub TLS server cert
				                                          5. CertificateRequest
				                                             └── Requests TMS client cert
				                                             └── (because Verify Peer = Enabled)
				                                          6. ServerHelloDone
				  
				  7. Certificate
				   └── TMS client cert (CN=TMS-Application)
				  8. ClientKeyExchange
				  9. CertificateVerify
				   └── Signature using TMS private key
				       (proves TMS holds the private key)
				  10. ChangeCipherSpec
				  11. Finished (encrypted)
				  
				                                          12. Verify TMS cert chain
				                                              └── Against Trusted CAs in TLS Profile
				                                          13. Match to registered application identity
				                                          14. ChangeCipherSpec
				                                          15. Finished (encrypted)
				  
				  ══════════════════════════════════════════
				  mTLS session established — Channel 2 open
				  ══════════════════════════════════════════
				  
				  16. TMS authenticates application identity:
				  
				    API Key mode:
				    Request:  [AORKLG;AP<api-token>;HD1;]
				    Response: [AORKLG;ANY;UG<roles>;SU<user>;LN1;JW<jwt>;]
				  
				    PKI Nonce-Signature mode (two steps):
				    Step 1 — Receive Challenge:
				    Request:  [AORKLG;CE<hex-cert>;CT<chain>;]
				    Response: [AORKLG;ANY;BO<nonce>;TH<session-id>;]
				  
				    Step 2 — Challenge Response:
				    TMS computes: SHA-256(nonce_bytes) → sign with TMS private key → SI (hex)
				    Request:  [AORKLG;TH<session-id>;SI<sig-hex>;HD1;]
				    Response: [AORKLG;ANY;UG<roles>;LN1;JW<jwt>;]
				  
				  Session authorized for PEDI / PEDK / PEDV
				  ```
				- **RKLG response tokens (verified from documentation):**
				  
				  | Token | Meaning |
				  |---|---|
				  | `AN=Y` | Login step succeeded |
				  | `UG` | Active roles (CSV) — confirms TMS has required permissions |
				  | `LN=1` | Login complete |
				  | `SU` | Users in the authorization context |
				  | `JW` | Session JWT — valid 10 minutes; refreshed automatically in last minute |
				  | `BO` | Nonce to sign (PKI mode, Step 1) |
				  | `TH` | Session identifier (PKI mode, echo back in Step 2) |
				  | `BB` | Error message on failure |
				- **RKLG PKI error conditions (verified from documentation):**
				  
				  | Error | Cause | Fix |
				  |---|---|---|
				  | `USER NOT LOGGED IN` | Raw `[AORKLG;CE<cert>;]` over FXCLI transport | Use `fxcli.kmes login pki` verb for FXCLI; use bracket message for direct TCP socket |
				  | `INVALID CERTIFICATE DATA` | Cert not hex-encoded X.509 DER or chain incomplete | Confirm hex-encoded DER; supply intermediates in `CT` |
				  | `FAILED TO VERIFY CERTIFICATE` | Cert does not chain to PKI identity provider trust anchor | Verify cert chain; confirm device time within cert validity |
				  | Challenge response rejected | Nonce reused after failed attempt | Request a new challenge; never reuse a nonce |
				  
				  ---
		- CHANNEL 3 — TMS rki-module ↔ CryptoHub CA (REST/TLS)
		  collapsed:: true
			- Used in Phase A for CSR submission, certificate retrieval, and revocation.
			- #### What is needed
			  collapsed:: true
				- | Item | How obtained |
				  |---|---|
				  | CryptoHub REST TLS certificate | Configured in TLS Settings (Management port 443) |
				  | TMS trusts CryptoHub REST cert | CA cert loaded into TMS HTTP client trust store |
				  | TMS REST API credentials | API Key or JWT from RKLG login |
				  | Issuing CA service UUID | `GET /api/v2/x509/services` |
			- **REST API base URL:**
			  collapsed:: true
				- ```
				  https://<cryptohub-host>/api/v2/
				  Authorization: Bearer <jwt-or-api-key>
				  ```
			- **Key REST endpoints used in Phase A:**
			  collapsed:: true
				- ```
				  POST /api/v2/x509/requests                          ← submit CSR
				  POST /api/v2/x509/requests/{uuid}/approve           ← approve if numApprovals > 0
				  GET  /api/v2/x509/{uuid}                            ← retrieve issued cert
				  GET  /api/v2/x509/services/{serviceUuid}/issuance-policy
				  PATCH /api/v2/x509/services/{serviceUuid}/issuance-policy
				  POST /api/v2/x509/{uuid}/revoke                     ← revoke cert
				  GET  /api/v2/x509/crls/export                       ← export updated CRL
				  ```
		- Channel Setup Summary
		  collapsed:: true
			- ```
			  ┌──────────────────────────────────────────────────────────────────────┐
			  │  Channel Setup Summary                                               │
			  ├──────────────────────────────────────────────────────────────────────┤
			  │  Channel 1: Terminal ↔ TMS (Standard mTLS)                           │
			  │  ├── Terminal cert: X.509 device cert (issued by CryptoHub CA, Ph.A) │
			  │  ├── Terminal private key: on-device, non-extractable                │
			  │  ├── TMS server cert: signed by CA trusted by terminal               │
			  │  ├── TMS trusts: CryptoHub Issuing CA (validates terminal cert)      │
			  │  └── Handshake: standard mTLS — mutual cert exchange + verify        │
			  │                                                                      │
			  │  Channel 2: TMS ↔ CryptoHub RKMS/KDH (mTLS, Host API)                │
			  │  ├── CryptoHub TLS profile: Verify Peer = Enabled                    │
			  │  ├── CryptoHub server cert: KDH signing cert (= CC in PEDI)          │
			  │  ├── TMS client cert: signed by CA in CryptoHub Trusted CAs          │
			  │  ├── TMS application identity: Client Application on CryptoHub       │
			  │  ├── Auth: RKLG API Key [AP=<token>; HD=1]                           │
			  │  │   OR:   RKLG PKI [CE=<cert>] → [TH+SI] (nonce-signature)          │
			  │  ├── KDH cert: hardware-protected, non-extractable (= CC in PEDI)    │
			  │  └── KRD CA: Issuing CA cert loaded into Excrypt RKL service         │
			  │                                                                      │
			  │  Channel 3: TMS rki-module ↔ CryptoHub CA (REST/TLS)                 │
			  │  ├── HTTPS to CryptoHub management port (443)                        │
			  │  ├── TMS trusts CryptoHub REST TLS cert                              │
			  │  └── Auth: Bearer JWT or API Key in Authorization header             │
			  └──────────────────────────────────────────────────────────────────────┘
			  ```
			  
			  ---
	- PHASE A — Device Certificate Lifecycle (Object Signing via CryptoHub CA)
	  collapsed:: true
		- Phase A is a hard prerequisite to Phase B. No RKI session can proceed without a valid, RKMS-trusted device certificate stored in TMS and resident on the terminal.
		- A.1 — TMS Certificate State Decision
		  collapsed:: true
			- ```
			  ┌──────────────────────────────────────────────────────────────────┐
			  │  TMS Internal Certificate State Check                            │
			  │                                                                  │
			  │  Does a device cert exist for this serial number?                │
			  │    ├── NO  ───────────────────────────────► A.2 (New Issue)      │
			  │    └── YES                                                       │
			  │         ├── Valid (not expired, not revoked) ──► Phase B         │
			  │         ├── Expired / near expiry ──────────► A.2 (Renewal)      │
			  │         └── Revoked / decommissioned ───────► A.5 (Revoke)       │
			  └──────────────────────────────────────────────────────────────────┘
			  ```
			- **TMS stores per device:**
			  collapsed:: true
				- | Field | Description |
				  |---|---|
				  | Signed X.509 device certificate | DER/PEM |
				  | Issued date | From CA response |
				  | Expiry date | From CA response |
				  | Serial number → cert binding | Used in PEDI `CD` token |
				  | Certificate status | Valid / Expired / Revoked |
		- A.2 — Terminal Generates RSA Keypair (On-Device, Channel 1)
		  collapsed:: true
			- TMS sends a command to the terminal over **Channel 1 (mTLS)** instructing it to generate an RSA keypair internally.
			- **Critical security property:** The terminal's private key is generated on-device and **never leaves the terminal**. It is used in Phase B to decrypt the TR-34 ephemeral key (B.7) and to sign the PEDV verification receipt (B.9).
			- RSA-2048 keypair generated in tamper-resistant secure element
			- Private key: non-extractable
			- Public key: exported in `PKCS#10` CSR (A.3)
			- **No CryptoHub Host API command at this step**
		- A.3 — Terminal Generates `PKCS#10` CSR → Sends to TMS (Channel 1)
		  collapsed:: true
			- Terminal produces a `PKCS#10` CSR and sends it to TMS over **Channel 1 (mTLS)**:
				- ```
				  CertificationRequest ::= SEQUENCE {
				  certificationRequestInfo  CertificationRequestInfo,
				  signatureAlgorithm        AlgorithmIdentifier,   ← SHA-256 with RSA
				  signature                 BIT STRING             ← signed by terminal private key
				  }
				  CertificationRequestInfo ::= SEQUENCE {
				  version       INTEGER { v1(0) },
				  subject       Name,                              ← CN=<terminal serial>, O=<mfr>
				  subjectPKInfo SubjectPublicKeyInfo,              ← terminal RSA public key
				  attributes    [0] Attributes
				  }
				  ```
			- The self-signature proves the terminal holds the private key (proof of possession). The CA validates this before issuing a certificate.
		- A.4 — TMS rki-module Submits CSR to CryptoHub Issuing CA (Channel 3)
		  collapsed:: true
			- TMS rki-module submits the terminal's PKCS#10 CSR to the CryptoHub Issuing CA over
			- **Channel 3 (REST/TLS)**:
			  collapsed:: true
				- ```
				  POST /api/v2/x509/requests
				  Authorization: Bearer <jwt>
				  Content-Type: application/json
				  {
				  "serviceUuid": "<issuing-ca-service-uuid>",
				  "csr": "<base64-encoded-PKCS10-CSR>"
				  }
				  ```
			- **CryptoHub CA Architecture:**
			  collapsed:: true
				- ```
				  [Root CA — offline, high-trust anchor]
				  └── Signs Issuing CA certificate (once, then offline)
				  - [Issuing CA Service — online]
				  ├── CA cert signed by Root CA
				  ├── Signing key: hardware-protected, non-extractable on CryptoHub
				  ├── Issuance Policy:
				  │     ├── Certificate Profile (validity, SHA-256 digest,
				  │     │   Key Encipherment usage, CN = terminal serial)
				  │     ├── Allowed key types (RSA-2048 minimum)
				  │     ├── numApprovals (0 = auto-issue)
				  │     └── Duplicate CN handling
				  └── Issues: terminal device certificates (leaf/end-entity)
				  ```
			- Source: [Issuing CAs overview](/CryptoHub/7.3.0.x/Administrator_guide/PKI_&_CA/Managing_certificates/Issuing_CAs/Overview) — *"Typically, an offline unit hosts your Root CA for security reasons while you keep the Issuing CA online to enable easier requests and approvals."*
			- **If numApprovals > 0:**
			  ```
			  POST /api/v2/x509/requests/{uuid}/approve
			  Authorization: Bearer <jwt>
			  ```
			- A.4b — CryptoHub CA Signs and Returns Device Certificate (Channel 3)
				- CA validates CSR self-signature, checks key type, applies cert profile, signs with Issuing CA private key (hardware-protected, non-extractable), returns signed X.509 v3 device certificate.
					- ```
					  GET /api/v2/x509/{uuid}
					  Authorization: Bearer <jwt>
					  ```
				- **TMS stores:** signed cert (DER/PEM), issued date, expiry date, serial → cert binding
				- **TMS → Terminal (Channel 1 mTLS):** relay signed device certificate
				- **Trust chain established:**
				  ```
				  Root CA cert → Issuing CA cert → Device cert (CN = terminal serial)
				  ```
				  This full chain must be loadable into the RKMS Series' KRD CA configuration for PEDI validation to succeed.
		- A.5 — Certificate Renewal and Revocation
		  collapsed:: true
			- **Renewal:** Re-run A.2–A.4b with a new keypair.
			- **Revocation (Channel 3):**
			  ```
			  POST /api/v2/x509/{uuid}/revoke
			  POST /api/v2/x509/services/{serviceUuid}/revoke
			  GET  /api/v2/x509/crls/export
			  ```
			- After revocation: CRL regenerated per configured CRL period; device blocked from future RKI (`AN=N` on PEDI).
	- PHASE B — RKI/RKL Key Delivery, Audit, and Lifecycle Management
	  collapsed:: true
		- B.1 — TMS Pre-Flight: Device and Key State Decision
		  collapsed:: true
			- ```
			  ┌──────────────────────────────────────────────────────────────────┐
			  │  TMS Key State Check                                             │
			  │                                                                  │
			  │  Does a valid key injection record exist for this device?        │
			  │    ├── NO / never injected ──────────────► B.2 (New Injection)   │
			  │    └── YES                                                       │
			  │         ├── Key valid, within policy ────► Skip (no action)      │
			  │         ├── Key expired / rotation due ──► B.2 (Re-key/Reload)   │
			  │         ├── Key compromised / revoked ───► B.12c (Key Revoke)    │
			  │         └── Device decommissioned ───────► B.12d (Decommission)  │
			  └──────────────────────────────────────────────────────────────────┘
			  ```
			- **Mandatory pre-flight checks:**
			  collapsed:: true
				- | Check | Condition | Fail action |
				  |---|---|---|
				  | Certificate valid | Not expired, not revoked | Trigger Phase A |
				  | Serial match | Cert CN/serial matches RKID record | Block RKI |
				  | Policy allows | Serial in group with valid CA | Block RKI |
				  | Group state | Not locked; connections not exceeded | Block RKI |
				  | Key state | New / valid / expired / revoked | Route to sub-flow |
			- **POS Device Group — RKL Options (verified from documentation):**
			  collapsed:: true
				- | Setting | Options | Notes |
				  |---|---|---|
				  | **CRL Certificate** | One or more CA certs | Validates remote device certs |
				  | **RKMS CA** | CA certificate | Validates RKMS Series certs returned in PEDI `CC` |
				  | **Default Signer** | One of the CRL CAs | Required when multiple CAs configured |
				  | **TR-34 Support** | Legacy / Strict / **Strict + Ext. Nonce** | Default: implicit `a0`, extended nonce |
				  | **RKL Ephemeral key type** | TDES / AES | Must match terminal capability |
				  | **Hashing** | SHA-1 / SHA-256 / Both | Both by default; must select at least one |
				  | **One Pass Timeout** | Default: **30 seconds** | Between PEDI→PEDK or PEDK→PEDK |
				  | **Allowed Connections** | Default: **1** | Simultaneous sessions per device |
				  | **Automatically Clear Injections** | Optional | Clears after configured period |
				  | **Associated Keys** | Key + Slot | Keys available to devices in this group |
			- **Notifications tab:**
			  collapsed:: true
				- | Setting | Description |
				  |---|---|
				  | **Enable Notifications** | Activates notification dispatch |
				  | **Notify on successful injections** | Sends notification on success |
				  | **Notify on failed injections** | Sends notification on failure |
		- B.2 — TMS Sends RKI Initiate to Terminal (Channel 1)
		  collapsed:: true
			- TMS sends a command to the terminal over **Channel 1 (mTLS)** signaling that an RKI session is about to begin. Applies to new injection, re-key, and post-revocation reload.
		- B.3 — Terminal Sends Ready Acknowledgment (Channel 1)
		  collapsed:: true
			- Terminal responds `{status: ready}` to TMS over **Channel 1 (mTLS)**.
		- B.4 — TMS Opens mTLS Session to CryptoHub (Channel 2)
		  collapsed:: true
			- TMS executes the full mTLS handshake (C2.8) then authenticates:
				- ```
				  TCP connect → TLS ClientHello → ServerHello + CryptoHub cert
				  → CertificateRequest → TMS presents client cert
				  → CertificateVerify → Finished → mTLS established
				  
				  API Key:  [AORKLG;AP<api-token>;HD1;]
				          → [AORKLG;ANY;UG<roles>;LN1;JW<jwt>;]
				  
				  PKI:      Step 1: [AORKLG;CE<hex-cert>;CT<chain>;]
				                  → [AORKLG;ANY;BO<nonce>;TH<session-id>;]
				          Step 2: sign SHA-256(nonce) with TMS private key → SI
				                  [AORKLG;TH<session-id>;SI<sig-hex>;HD1;]
				                  → [AORKLG;ANY;UG<roles>;LN1;JW<jwt>;]
				  ```
				- Session authorized for PEDI/PEDK/PEDV.
		- B.5 — PEDI: Identification Request (Channel 2)
		  collapsed:: true
			- **Command:** `PEDI` | `VS=3` | `MD=2`
			- **PEDI Request Tokens (verified from documentation):**
			  collapsed:: true
				- | Token | Length | Data Type | Accepted Values | Required? |
				  |---|---|---|---|---|
				  | `AO` | 4 | Alphabetic | `PEDI` | Yes |
				  | `VS` | 1 | Numeric | `3` | Yes |
				  | `MD` | 1 | Numeric | `2` | Yes |
				  | `CD` | 1–16000 | Base64 or Hex | Terminal's PKCS#7 or X.509 device encryption cert | Yes |
				  | `YA`–`YI` | 1–16000 | Base64 or Hex | Additional certs for validating `CD` | Optional |
				  | `DG` | 1–128 | ASCII | Device group name | Optional |
				  | `RF` | 1 | Numeric | `0`=X.509 only; `1`=full PKCS#7 chain (excl. root) | Optional |
				  | `TD` | 1–2 | Numeric | `1`–`10` (default `1`); chain depth (only if `RF=1`) | Optional |
			- **PEDI Response Tokens (verified from documentation):**
			  collapsed:: true
				- | Token | Length | Data Type | Returned Values | Meaning |
				  |---|---|---|---|---|
				  | `AO` | 4 | Alphabetic | `PEDI` | Response echo |
				  | `AN` | 1 | — | `Y`/`I`/`L`/`N` | Key verification result |
				  | `CC` | 1–16000 | Hexadecimal | DER hex | RKMS Series signing cert — relay to terminal |
				  | `RD` | 8–32 | Hexadecimal | Random bytes | RKMS nonce — **retain for PEDK `KA`** |
			- **`AN` codes:**
			  collapsed:: true
				- | Code | Meaning | Recovery |
				  |---|---|---|
				  | `Y` | Device found, keys available, certs verified | Proceed to PEDK |
				  | `I` | Key not cleared; multiple loads not enabled | Clear injection or enable setting |
				  | `L` | Device locked | Manually unlock on RKMS Series |
				  | `N` | Device not found or cert chain invalid | Verify serial in group; verify CA chain |
			- **Verified PEDI example (from documentation):**
			  collapsed:: true
				- ```plaintext
				  Request: [AOPEDI;DGv3-m2-ch_BasicSplit;RG1;RF1;
				  CD-----BEGIN CERTIFICATE-----
				  MIID8DCCAtigAwIBAgIJALPvkC07mOJsMA0GCSqGSIb3DQEBCwUAMIGPMQswCQYD
				  VQQGEwJVUzELMAkGA1UECAwCVFgxETAPBgNVBAcMCEJ1bHZlcmRlMRAwDgYDVQQK
				  DAdGdXR1cmV4MRQwEgYDVQQLDAtFbmdpbmVlcmluZzEQMA4GA1UEAwwHY2Etcm9v
				  ...-----END CERTIFICATE-----
				  ;YA-----BEGIN CERTIFICATE-----...-----END CERTIFICATE-----
				  ;YB-----BEGIN CERTIFICATE-----...-----END CERTIFICATE-----
				  ;VS3;MD2;]
				  
				  Response: [AOPEDI;ANY;
				  CC3082042706092A864886F70D010702A0820418...
				  RDFD09FDB6841265CF60C390B139DBCE90;]
				  ```
			- **Log verification:**
			  collapsed:: true
				- |Field | Value | Verified meaning |
				  |---|---|---|
				  | `AN` | `Y` | Device found, certs verified, keys available |
				  | `CC` | `3082042706092A864886...` | RKMS signing cert (PKCS#7 chain, hex DER) — relay to terminal |
				  | `RD` | `FD09FDB6841265CF60C390B139DBCE90` | 16-byte RKMS nonce — **retain for PEDK `KA`** |
			- **30-second clock starts NOW.**
		- B.6 — PEDK: Key Request (Channel 2)
		  collapsed:: true
			- **Command:** `PEDK` | `VS=3` | `MD=2`
			- **PEDK Request Tokens (verified from documentation):**
			  
			  | Token | Length | Data Type | Accepted Values | Required? |
			  |---|---|---|---|---|
			  | `AO` | 4 | Alphabetic | `PEDK` | Yes |
			  | `VS` | 1 | Numeric | `3` | Yes |
			  | `MD` | 1 | Numeric | `2` | Yes |
			  | `CE` | 1 | Numeric | `1`=PKCS#1 v1.5 (default); `2`=PSS | Optional |
			  | `SA` | Variable | Numeric | Salt length; defaults to hash length | Optional (PSS only) |
			- **PEDK Response Tokens (verified from documentation):**
			  
			  | Token | Length | Data Type | Returned Values | Meaning |
			  |---|---|---|---|---|
			  | `AO` | 4 | Alphabetic | `PEDK` | Response echo |
			  | `AN` | 1 | Alphabetic | `Y`/`N` | Success code |
			  | `DG` | 1–128 | ASCII | Group name | Device group confirmed |
			  | `KN` | 1–10 | Numeric | `0`–`9` | Remaining keys after this delivery |
			  | `AP` | 1–8000 | Hexadecimal | TR-34 one-pass structure | Complete key blob |
			  | `CE` | 1 | Numeric | `1` or `2` | Padding method used |
			  | `RG` | 1 | Numeric | `0`=SHA1; `1`=SHA256 | Hashing algorithm |
			  | `KA` | 1–8000 | Hexadecimal | RSA signature | RKMS signature over response |
			- **`AP` — TR-34 One-Pass Structure (verified from documentation):**
			  ```
			  AP contains:
			  ├── Target key, encrypted by ephemeral symmetric key (TDES or AES)
			  ├── Ephemeral key, encrypted by terminal's RSA public key (from CD in PEDI)
			  └── Entire structure signed by RKMS Series private key (KDH cert = CC from PEDI)
			  ```
			- **`KA` covers (in order):** `VS + MD + AN(PEDI) + DG + KN + AP + RG + CE + SA + RD(PEDI nonce)`
			- **Verified PEDK example (from documentation):**
			- ```plaintext
			  PEDI Nonce: E1AE30FF19568D900E6AEEACF55D5EAC
			  - Request:  [AOPEDK;CE1;VS3;MD2;SE1;]
			  - Response: [AOPEDK;ANY;DGv3-m2-ch_BasicSplit;KN5;RG1;CE1;
			  AP308205C906092A864886F70D010702...
			  KA063D192E9F50F1379E2664D311F413...;]
			  ```
			- **Log verification:**
			  collapsed:: true
				- | Field | Value | Verified meaning |
				  |---|---|---|
				  | `AN` | `Y` | Keys delivered |
				  | `DG` | `v3-m2-ch_BasicSplit` | Device group confirmed |
				  | `KN` | `5` | 5 remaining keys |
				  | `RG` | `1` | SHA-256 |
				  | `CE` | `1` | PKCS#1 v1.5 |
				  | `AP` | `308205C9...` | TR-34 blob — relay to terminal |
				  | `KA` | `063D192E...` | RKMS signature over VS+MD+AN+DG+KN+AP+RG+CE+SA+RD |
		- B.7 — TMS Delivers Key Block to Terminal (Channel 1)
		  collapsed:: true
			- TMS relays `AP` and slot assignment to terminal over **Channel 1 (mTLS)**:
			- ```
			  Terminal (hardware-protected secure element):
			  1. Verify RKMS KA signature using CC cert from PEDI
			  2. Decrypt ephemeral key using terminal RSA private key
			  3. Decrypt target key using ephemeral key
			  4. Load key into designated slot
			  5. Compute KCV
			   └── TDES: encrypt 8 zero bytes, take first 3 bytes
			   └── AES:  encrypt 16 zero bytes, take first 3 bytes
			  ```
			  
			  ---
		- B.8 — Terminal Returns Load Result + KCV (Channel 1)
		  collapsed:: true
			- Terminal sends `{success/fail, KCV}` to TMS over **Channel 1 (mTLS)**.
		- B.9 — PEDV: Key Verification Request (Channel 2)
		  collapsed:: true
			- **Command:** `PEDV` | `VS=3` | `MD=1`
			- **PEDV Request Tokens (verified from documentation):**
			  collapsed:: true
				- | Token | Length | Data Type | Accepted Values | Required? |
				  |---|---|---|---|---|
				  | `AO` | 4 | Alphabetic | `PEDV` | Yes |
				  | `VS` | 1 | Numeric | `3` (default) | Optional |
				  | `MD` | 1 | Numeric | `1` (default) | Optional |
				  | `KV` | Variable | Hexadecimal | Key Verification ASN.1 structure | Yes |
				  | `KA` | 1–1024 | Hexadecimal | Terminal RSA signature | Yes |
				  | `CE` | 1 | Numeric | `1`=PKCS#1 v1.5; `2`=PSS | Optional |
				  | `SA` | Variable | Numeric | Salt length | Optional (PSS only) |
				  | `RD` | 1–8 or 32 | Hexadecimal | Device-generated nonce | Yes |
			- **Terminal `KA` signs:** `KV + device nonce (RD) + RKMS nonce (RD from PEDI) + CE + SA`
			  
			  > *"If the nonce is less than 8 (32 if ext. nonce option is selected) characters, it is automatically padded by prepending zeros."*
			- **PEDV Response Tokens (verified from documentation):**
			  collapsed:: true
				- | Token | Length | Data Type | Returned Values | Meaning |
				  |---|---|---|---|---|
				  | `AO` | 4 | Alphabetic | `PEDV` | Response echo |
				  | `BB` | 1 | Alphabetic | `Y`=all confirmed, device locked; `N`=keys still available | Key status |
				  | `RD` | 1–8/32 | Hexadecimal | RKMS nonce | For response signature verification |
				  | `CE` | 1 | Numeric | `1` or `2` | Padding method |
				  | `KA` | 1–1024 | Hexadecimal | RKMS RSA signature | Signs: `BB + device nonce + RKMS nonce + CE + SA` |
			- **Verified PEDV example (from documentation):**
			  collapsed:: true
				- ```plaintext
				  Request: [AOPEDV;
				  KV3041020101313C300D13064B6579312D310201001300...
				  KA4BD7B95A4E44853BEFB4552DEEF07D3B6759B32DF4...
				  CE2;SA64;RDEBDB09FA;]
				  
				  Response: [AOPEDV;BBY;
				  KA52C97B1C8E6DC1A87A78A00BAAE5F8569DCD323BBC...
				  CE2;RDE2011631;SA64;]
				  ```
			- **Verified signature computation (from documentation):**
			  collapsed:: true
				- ```
				  Request KA:
				  Format: "%s%s%s%02X%04X" % (KV, deviceNonce, rkmsNonce, padding, saltLength)
				  Device Nonce: EBDB09FA | RKMS Nonce: C03B49C0 | Padding: 2 | Salt: 64
				  Sig Data (hex): [KV][EBDB09FA][C03B49C0][02][0040]
				  SHA1 of decoded sig data: 35FE5204F8BCE0ADA345D73982539E306AB8BC7F
				  - Response KA:
				  Format: BB + deviceNonce + rkmsNonce + padding + saltLength
				  Success: Y(59) | Device Nonce: EBDB09FA | RKMS Nonce: E2011631
				  Sig Data (hex): 59EBDB09FAE2011631020040
				  SHA1 of decoded sig data: BBB1827DAC8C51B01FD2F9BD28652E95F199407B
				  ```
			- **Critical:** `BB=Y` confirms signed receipt only — not key operability. Run a test PIN translation before marking session complete.
		- B.10 — TMS Records Result and Dispatches Notifications
		  collapsed:: true
			- 1. Records injection result in RKID (success/fail, KCV, timestamp, slot)
			- 2. Updates device record
			- 3. Relays RKI success/failure to terminal over **Channel 1 (mTLS)**
			- 4. Dispatches `EXNO` notification to RKL Notification Servers (if enabled)
			  
			  ---
		- B.11 — TMS Audit: Key Injection Logs
		  collapsed:: true
			- collapsed:: true
			  ```
			  Services → Deployed Services → [Service] → [ Injection Logs ]
			  ```
				- | View | What it shows |
				  |---|---|
				  | **View Devices** | Time-stamped loading events per device |
				  | **View Keys** | Key used for each loading event |
				  | **View Device Logs** | Full device-level log |
				  | **Export CSV** | Time, key type, device, cryptogram, checksum, KSN, pass/fail, notes |
			- **Audit log types (verified from documentation):**
			  collapsed:: true
				- | Log type | Captures |
				  |---|---|
				  | **PED Injection** | All PED injection processes |
				  | **Object creation** | Key lifecycle events |
				  | **Authentication** | Log-ins, log-outs, failures |
				  | **Transaction** | Transactional operations |
				  | **Web API** | API calls to the device |
				  | **Notifications** | All notifications sent |
		- B.12 — TMS Key Lifecycle Management
		  collapsed:: true
			- ```
			  ├── Key valid, within policy ──────────► No action
			  ├── Rotation due ──────────────────────► B.12a (Re-key: re-run B.2–B.9)
			  ├── Load failed ───────────────────────► B.12b (Retry/Alert)
			  ├── Key compromised ───────────────────► B.12c (Revoke + re-run Phase A + B)
			  └── Device decommissioned ─────────────► B.12d (Delete from RKID + revoke cert)
			  ```
			- **B.12a — Re-key:** Re-run B.2–B.9. "Automatically Clear Injections" automates old record clearance.
			- **B.12b — Retry:** Record failure; dispatch notification; retry per policy; escalate if limit exceeded.
			- **B.12c — Key Revocation:** Flag device; revoke cert if needed (A.5); re-run Phase A + B.2–B.9.
			- **B.12d — Decommission:**
			  ```
			  Classic Tools → Device Key Loading → Remote Key Injection Database
			  → Right-click device → Delete Batch → Select batch → OK
			  ```
			- Revoke cert (A.5); lock/remove serial from RKID; all future PEDI → `AN=N`.
	- Complete Chronological Flow
	  collapsed:: true
		- ```
		  ══════════════════════════════════════════════════════════════════════
		  PRE-PHASE 0 — mTLS / TLS INFRASTRUCTURE SETUP (one-time)
		  ══════════════════════════════════════════════════════════════════════
		  
		  CHANNEL 1: Terminal ↔ TMS (Standard mTLS)
		  ├── Terminal device cert: issued in Phase A (A.2–A.4b)
		  ├── TMS server cert: signed by CA trusted by terminal
		  ├── TMS trusts: CryptoHub Issuing CA (validates terminal cert)
		  └── Handshake: standard mTLS — mutual cert exchange
		                 ClientHello → ServerHello → CertificateRequest
		                 → Terminal presents device cert → CertificateVerify
		                 → ChangeCipherSpec → Finished
		  
		  CHANNEL 2: TMS ↔ CryptoHub RKMS/KDH (mTLS, Host API)
		  C2.1  CryptoHub: TLS Profile (Verify Peer = Enabled)
		        Settings → Networking → TLS Settings → Profiles → [ Add Profile ]
		        └── Trusted CAs: CA that signed TMS client cert
		  C2.2  CryptoHub: TLS Keys + Certificates
		        Settings → Networking → TLS Settings → Keys → [ Add Key ]
		        → [ Request Signing ] → CSR → signed by CA TMS trusts
		  C2.3  CryptoHub: Assign mTLS profile to Excrypt port (9000)
		        Settings → Networking → TLS Settings → Servers
		  C2.4  CryptoHub: TMS Client Application Identity
		        Identity and Access → Applications → [ + Add ]
		        └── Auth: API Key (download .txt) OR TLS Certificate + TLS Provider
		  C2.5  TMS: generate RSA keypair + PKCS#10 CSR
		        └── CSR signed by CA trusted by CryptoHub TLS profile
		        └── TMS stores: signed cert + private key
		  C2.6  CryptoHub: KDH signing keypair + certificate
		        Services → Excrypt RKL → Service Info → New KDH Key Pair
		        └── RSA-2048, hardware-protected, non-extractable
		        └── This cert = CC returned in PEDI
		  C2.7  CryptoHub: KRD CA configuration
		        Services → Excrypt RKL → Service Info → Choose KRD CA
		        └── Upload Issuing CA cert (+ optional CRL)
		  C2.8  Handshake: mTLS — mutual cert exchange
		        TCP → ClientHello → ServerHello + CryptoHub cert
		        → CertificateRequest → TMS presents client cert
		        → CertificateVerify → Finished → mTLS established
		        Auth: RKLG API Key [AP=<token>; HD=1] → AN=Y, LN=1
		        OR:   RKLG PKI [CE=<cert>] → BO+TH
		                       [TH+SI] → AN=Y, LN=1
		  
		  CHANNEL 3: TMS rki-module ↔ CryptoHub CA (REST/TLS)
		  ├── TMS trusts CryptoHub REST TLS cert (load into HTTP trust store)
		  ├── TMS obtains API Key or JWT for REST API auth
		  └── Verify: GET /api/v2/x509/services → retrieve Issuing CA service UUID
		  
		  ══════════════════════════════════════════════════════════════════════
		  PHASE A — OBJECT SIGNING (Device Certificate Lifecycle)
		  ══════════════════════════════════════════════════════════════════════
		  
		  A.1  TMS: cert state check (internal)
		     ├── No cert / expired  ──► A.2
		     ├── Valid              ──► Phase B
		     └── Revoked            ──► A.5
		  
		  A.2  TMS → Terminal [Ch.1 mTLS]: generate RSA keypair (on-device)
		     Private key: hardware-protected, never leaves terminal
		     No CryptoHub Host API command at this step
		  
		  A.3  Terminal → TMS [Ch.1 mTLS]: PKCS#10 CSR
		     CN=serial, self-signed by terminal private key (proof of possession)
		  
		  A.4  TMS rki-module → CryptoHub CA [Ch.3 REST/TLS]:
		     POST /api/v2/x509/requests  [Authorization: Bearer <jwt>]
		     [If numApprovals > 0]: POST /api/v2/x509/requests/{uuid}/approve
		  
		  A.4b CryptoHub CA → TMS [Ch.3 REST/TLS]: signed X.509 device cert
		     GET /api/v2/x509/{uuid}
		     TMS stores: cert + dates + serial binding
		     TMS → Terminal [Ch.1 mTLS]: relay signed cert
		     Trust chain: Root CA → Issuing CA → Device cert (CN=serial)
		  
		  A.5  [Renewal: repeat A.2–A.4b]
		     [Revocation (Ch.3)]:
		       POST /api/v2/x509/{uuid}/revoke
		       GET  /api/v2/x509/crls/export
		  
		  ══════════════════════════════════════════════════════════════════════
		  PHASE B — RKI/RKL + Key Audit and Lifecycle
		  ══════════════════════════════════════════════════════════════════════
		  
		  B.1  TMS: pre-flight check (internal)
		     ✓ Cert valid | ✓ Serial in RKID | ✓ Policy allows
		     ✓ Group not locked | ✓ Key state routing
		  
		  B.2  TMS → Terminal [Ch.1 mTLS]: RKI Initiate
		  
		  B.3  Terminal → TMS [Ch.1 mTLS]: Ready Ack {status: ready}
		  
		  B.4  TMS → CryptoHub [Ch.2 mTLS]: handshake + RKLG auth
		     → mTLS established → AN=Y, LN=1
		  
		  B.5  TMS → CryptoHub [Ch.2]: PEDI [VS=3, MD=2, CD=device cert,
		                                    YA-YI=chain, DG=group, RF=1]
		     CryptoHub validates: cert chain vs KRD CA, serial in RKID,
		                          group policy, locked state
		     CryptoHub → TMS: AN=Y, CC=KDH signing cert (hex DER), RD=nonce
		     ⚠ 30-second clock starts | ⚠ Retain RD
		  
		  B.6  TMS → CryptoHub [Ch.2]: PEDK [VS=3, MD=2, CE=1]
		     [Repeat per key type, within 30s each]
		     CryptoHub → TMS: AP=TR-34 blob, KN=remaining,
		                      KA=sig(VS+MD+AN+DG+KN+AP+RG+CE+SA+RD)
		  
		  B.7  TMS → Terminal [Ch.1 mTLS]: {AP key block, slot}
		     Terminal: verify KA → decrypt ephemeral key → decrypt key
		             → load into slot → compute KCV
		  
		  B.8  Terminal → TMS [Ch.1 mTLS]: {success/fail, KCV}
		  
		  B.9  TMS → CryptoHub [Ch.2]: PEDV [KV=ASN.1, KA=terminal sig,
		                                    CE=2, SA=64, RD=device nonce]
		     CryptoHub → TMS: BB=Y, KA=RKMS sig, RD=nonce, CE=2, SA=64
		     ⚠ BB=Y = signed receipt only; run test PIN translation
		  
		  B.10 TMS: record result in RKID
		     TMS → Terminal [Ch.1 mTLS]: RKI Success/Failure
		     TMS → Notification Server: EXNO (if configured)
		  
		  B.11 TMS: audit injection logs
		     Services → Deployed Services → Injection Logs
		     Audit: PED Injection, Transaction, Object creation,
		            Authentication, Web API, Notifications
		  
		  B.12 TMS: key lifecycle decision
		     ├── Rotation due ──► B.12a (Re-key: re-run B.2–B.9)
		     ├── Load failed  ──► B.12b (Retry/Alert)
		     ├── Compromised  ──► B.12c (Revoke + re-run Phase A + B)
		     └── Decommission ──► B.12d (Delete from RKID + revoke cert)
		  ```
	- Error States — Complete Reference
	  collapsed:: true
		- | Symptom                               | Root Cause                                        | Recovery                                                                                                       |
		  | ------------------------------------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
		  | `AN=N` on PEDI                        | Serial not in RKID or cert chain invalid          | Verify serial registered in device group; verify CRL Certificate CA matches the CA that signed the device cert |
		  | `AN=I` on PEDI                        | Key not cleared; multiple loads not enabled       | Clear existing injection OR enable "Allow multiple key loads without clearing"                                 |
		  | `AN=L` on PEDI                        | Device locked — too many failed attempts          | Manually unlock device on RKMS Series                                                                          |
		  | Session timeout between PEDI and PEDK | >30s elapsed                                      | Reduce processing time; increase One Pass Timeout in group config                                              |
		  | `BB=Y` but key not operational        | PEDV confirms signed receipt, not key persistence | Run test PIN translation before marking session complete                                                       |
		  
		  |                                       |
- Implementation in Phases [ Task management ]
  collapsed:: true
	- Phase 1 : Emulated Terminal with TMS(rki-module) initiated RKI
	- Phase 2 : TMS to the Terminal
	- Integration : E2E RKI/RKL Integration
- RKI CORE MODULE : A Roadmap Outline
  collapsed:: true
	- Mastering the Rust programming language from absolute zero, and building a production-grade Remote Key Injection (RKI) core module that speaks the Futurex Excrypt RKL v3 Mode 2 protocol.
	- By the end i would be able to:
	  collapsed:: true
		- Internalized Rust's ownership semantics, trait design, async I/O, cryptography, and TLS architecture
		- Constructed a PCI-compliant, mTLS-secured, audit-ready key delivery system
		- Developed a terminal simulator with full Key Receiving Device (KRD) behavior
		- Built a mock CryptoHub that faithfully implements RKL v3 Mode 2
		- Produced architecture decision records, threat models, and operational runbooks
	- PART I — FOUNDATION (Days 1–10)
	  collapsed:: true
		- **Theme:** Rust fundamentals + Project scaffolding + mTLS infrastructure
		- Day 1: Ownership, Borrowing, and Project Genesis
		  collapsed:: true
			- **Rust Concepts:** Ownership, references, `String` vs `&str`, `struct`, `impl`, Cargo workspace, modules
			- **RKI Component:** `Cargo.toml`, `lib.rs` skeleton, `error.rs` skeleton, `config.rs` skeleton
			- **Protocol Context:** Overview of the three-channel architecture. Why ownership matters for key material safety — keys must never be implicitly copied.
			- **Key Topics:**
			  collapsed:: true
				- Why Rust exists: memory safety without garbage collection
				- The stack vs the heap
				- Ownership rules: each value has exactly one owner
				- Borrowing: `&T` (shared) vs `&mut T` (mutable)
				- String vs `&str` — owned vs borrowed data
				- Structs and impl blocks
				- Module system and project layout
			- **Exercise:** Create the project skeleton. Define `RkiConfig` struct with ownership-correct fields.
		- Day 2: Enums, Pattern Matching, and the Result Monad
		  collapsed:: true
			- **Rust Concepts:** `enum`, `match`, `Option<T>`, `Result<T, E>`, `?` operator, exhaustive matching
			- **RKI Component:** `RkiError` enum, `ProtocolVersion` enum, `An` (answer code) enum
			- **Protocol Context:** The `AN` codes in PEDI responses — `Y`, `I`, `L`, `N`. Error handling in protocol messages.
			- **Key Topics:**
				- Enums as types with variants
				- Pattern matching with `match`
				- `Option<T>` — handling the absence of a value
				- `Result<T, E>` — handling fallible operations
				- The `?` operator for error propagation
				- Exhaustive matching and the compiler's guarantee
			- **Exercise:** Define `RkiError` with variants for TLS errors, protocol errors, config errors. Implement `ProtocolVersion` as an enum.
		- Day 3: Error Handling as a Discipline
		  collapsed:: true
			- **Rust Concepts:** `thiserror`, error contexts, error taxonomy, `From` trait, error chaining
			- **RKI Component:** Complete `error.rs` with `RkiError`, `TlsError`, `ProtocolError`, `ConfigError`
			- **Protocol Context:** Error states in RKL — timeout, invalid cert, device locked. How errors propagate through the ceremony.
			- **Key Topics:**
				- Error taxonomy — categorize by source
				- `thiserror` derive macro
				- `From` implementations for automatic conversion
				- Error chaining with `#[source]`
				- Display vs Debug formatting
				- When to use `anyhow` vs `thiserror`
			- **Exercise:** Complete the error type hierarchy. Write tests that verify error messages are useful.
		- Day 4: String Manipulation and Tokenization
		  collapsed:: true
			- **Rust Concepts:** `String`, `Vec<u8>`, `split`, `parse`, byte-level parsing, `Cow<'a, str>`
			- **RKI Component:** `RklMessage` parser — tokenize `[AORKLG;AP...;]` frames
			- **Protocol Context:** RKL messages are bracket-delimited, semicolon-separated token strings. Parsing must handle hex, base64, and ASCII values.
			- **Key Topics:**
			  collapsed:: true
				- String vs byte slices for protocol parsing
				- The `split` family of methods
				- Parsing numbers from strings with `parse::<T>()`
				- Handling variable-length token values
				- `Cow<'a, str>` for borrowed/owned flexibility
				- Efficient tokenization without unnecessary allocation
			- **Exercise:** Write the RKL message parser. Tests: parse a real PEDI request from the documentation, handle malformed messages.
		- Day 5: Configuration and the Builder Pattern
		  collapsed:: true
			- **Rust Concepts:** `serde`, `toml`, builder pattern, `Default`, validation
			- **RKI Component:** `RkiConfig`, `TlsChannelConfig`, `CryptohubConfig`, `TerminalConfig`
			- **Protocol Context:** CryptoHub connection settings — host, port, TLS profile, CA paths. Terminal channel settings.
			- **Key Topics:**
				- Serde derive for TOML deserialization
				- Builder pattern for complex configs
				- `Default` trait
				- Configuration validation at load time
				- Environment variable overrides
			- **Exercise:** Define complete configuration types. Load from `rki-core.toml`. Validate required fields.
		- Day 6: Async Foundations — Tokio and the Reactor
		  collapsed:: true
			- **Rust Concepts:** `async fn`, `await`, `tokio::spawn`, channels, `Arc` and `Mutex`
			- **RKI Component:** Async scaffolding — `run()` entrypoint, task structure
			- **Protocol Context:** Three concurrent channels. Terminal channel accepts connections while Host channel communicates with CryptoHub.
			- **Key Topics:**
				- What is async? Cooperative scheduling
				- Tokio runtime — the reactor
				- `async fn` and `await`
				- Spawning tasks with `tokio::spawn`
				- Channels for inter-task communication
				- `Arc<Mutex<T>>` for shared state
			- **Exercise:** Create async entrypoint. Spawn a task that listens on the terminal channel. Use a channel to communicate.
		- Day 7: TLS with rustls — Certificate and Key Types
		  collapsed:: true
			- **Rust Concepts:** `Certificate`, `PrivateKey`, `RootCertStore`, PEM parsing, type safety
			- **RKI Component:** `tls/mod.rs` — `load_cert_chain()`, `load_private_key()`, `build_root_store()`
			- **Protocol Context:** All three channels require TLS. CryptoHub requires specific cipher suites and mTLS.
			- **Key Topics:**
				- X.509 certificates and private keys
				- PEM vs DER encoding
				- rustls types: `Certificate`, `PrivateKey`, `RootCertStore`
				- Loading certificates from files
				- Building trust stores
				- Type safety — certificates are not strings
			- **Exercise:** Write TLS loading utilities. Load test certificates. Verify they parse correctly.
		- Day 8: mTLS Server (Terminal Channel)
		  collapsed:: true
			- **Rust Concepts:** `ServerConfig`, client cert verification, `tokio-rustls`, `TcpListener`
			- **RKI Component:** `tls/terminal.rs` — TMS server accepting terminal mTLS connections
			- **Protocol Context:** Terminal presents its device certificate. TMS validates it against the issuing CA. This is Channel 1 in our architecture.
			- **Key Topics:**
				- `ServerConfig` with client cert requirement
				- `WebPkiClientVerifier` for client cert validation
				- `TcpListener` and `TcpStream`
				- `tokio-rustls` for async TLS
				- Handshake flow
				- Extracting client certificate from connection
			- **Exercise:** Build the terminal channel server. Accept connections from a test client. Verify client certificate.
		- Day 9: mTLS Client (Host Channel)
		  collapsed:: true
			- **Rust Concepts:** `ClientConfig`, client identity, server cert pinning, `TcpStream`
			- **RKI Component:** `tls/host.rs` — TMS client for CryptoHub Host API
			- **Protocol Context:** TMS presents its client certificate to CryptoHub. CryptoHub requires mTLS with Verify Peer = Enabled. This is Channel 2.
			- **Key Topics:**
				- `ClientConfig` with client certificate
				- Server certificate validation
				- Root store configuration
				- `TcpStream::connect` with TLS
				- ALPN configuration
				- Cipher suite selection (TLS 1.2/1.3, DHE_RSA)
			- **Exercise:** Build the host channel client. Connect to a mock CryptoHub. Verify mTLS handshake.
		- Day 10: REST Client (CA Channel)
		  collapsed:: true
			- **Rust Concepts:** `reqwest`, `rustls` integration, header auth, JSON serialization
			- **RKI Component:** `tls/ca.rs` — `CaRestClient` with Bearer token
			- **Protocol Context:** CryptoHub CA exposes REST API at `/api/v2/`. Used for CSR submission, cert retrieval, revocation. This is Channel 3.
			- **Key Topics:**
				- `reqwest::Client` with custom TLS config
				- Bearer token authentication
				- JSON request/response with `serde_json`
				- REST endpoint design
				- Error handling for HTTP status codes
			- **Exercise:** Build the CA REST client. Query `GET /api/v2/x509/services`. Handle authentication errors.
		- **Milestone 1 (Day 10):** All three channels establish TLS connections with mutual authentication. Unit tests pass for config loading, error mapping, message parsing, and TLS handshakes against mock endpoints. First ADR written (ADR-0001: use-rustls).
	- PART II — CERTIFICATE LIFECYCLE (Days 11–20)
	  collapsed:: true
		- **Theme:** Traits and generics + PKI + Complete certificate lifecycle
		- Day 11: Traits and the Command Pattern
		  collapsed:: true
			- **Rust Concepts:** `trait`, generics, `impl Trait`, trait objects, `dyn`, associated types
			- **RKI Component:** `commands/mod.rs` — `RklCommand` trait definition
			- **Protocol Context:** RKLG, PEDI, PEDK, PEDV are all commands sent over the Host Channel. They share a common structure.
			- **Key Topics:**
				- Traits as shared behavior
				- Generic parameters vs associated types
				- `impl Trait` for parameter position
				- `dyn Trait` for dynamic dispatch
				- Trait objects with `Box<dyn Trait>`
				- When to use generics vs trait objects
			- **Exercise:** Define `RklCommand` trait with `serialize()`, `parse_response()`, `command_name()` methods.
		- Day 12: RKLG — Login (API Key Mode)
		  collapsed:: true
			- **Rust Concepts:** Trait implementation, serialization, `AsRef<str>`, unit testing
			- **RKI Component:** `commands/rklg.rs` — `RklgApiKey` struct implementing `RklCommand`
			- **Protocol Context:**
			  ```
			  Request:  [AORKLG;AP<api-token>;HD1;]
			  Response: [AORKLG;ANY;UG<roles>;SU<user>;LN1;JW<jwt>;]
			  ```
			- **Key Topics:**
				- Implementing a trait for a struct
				- Serializing command to RKL message format
				- Parsing response tokens
				- JWT extraction from response
				- Role parsing from `UG` token
				- Unit tests with real documented examples
			- **Exercise:** Implement `RklgApiKey`. Test against the documented example.
		- Day 13: RKLG — Login (PKI Nonce-Signature Mode)
		  collapsed:: true
			- **Rust Concepts:** Two-step async trait, state machines, enum-based state
			- **RKI Component:** `commands/rklg.rs` — `RklgPki` with challenge/response state
			- **Protocol Context:**
			  ```
			  Step 1: [AORKLG;CE<hex-cert>;CT<chain>;]
			  Response: [AORKLG;ANY;BO<nonce>;TH<session-id>;]
			  
			  Step 2: sign SHA-256(nonce) → SI
			  Request: [AORKLG;TH<session-id>;SI<sig-hex>;HD1;]
			  Response: [AORKLG;ANY;UG<roles>;LN1;JW<jwt>;]
			  ```
			- **Key Topics:**
			  collapsed:: true
				- Multi-step protocol commands
				- State machine representation with enums
				- Certificate serialization to hex
				- SHA-256 nonce signing
				- Signature format — PKCS#1 v1.5
				- Session ID tracking
			- **Exercise:** Implement `RklgPki`. Test the two-step flow with a mock nonce.
		- Day 14: PEDI — Identification Request
		  collapsed:: true
			- **Rust Concepts:** Complex token validation, builder pattern, optional fields
			- **RKI Component:** `commands/pedi.rs` — `PediRequest`, `PediResponse`, `PediAn` enum
			- **Protocol Context:**
			  ```
			  Request:  [AOPEDI;VS3;MD2;CD{device_cert};YA-YI{chain_certs};DG{group};RF1;]
			  Response: [AOPEDI;ANY;CC{rkms_signing_cert_hex};RD{rkms_nonce};]
			  ```
			- **Key Topics:**
				- Builder pattern for complex commands
				- Optional token fields (YA-YI, RF, TD)
				- Certificate serialization (PEM to hex)
				- Chain certificate handling
				- Response parsing — CC and RD tokens
				- `PediAn` enum for Y/I/L/N codes
			- **Exercise:** Implement `PediRequest` and `PediResponse`. Test with documented example including chain certificates.
		- Day 15: PEDK — Key Request
		  collapsed:: true
			- **Rust Concepts:** Response parsing, binary hex decoding, signature data
			- **RKI Component:** `commands/pedk.rs` — `PedkRequest`, `PedkResponse`
			- **Protocol Context:**
			  ```
			  Request:  [AOPEDK;VS3;MD2;CE1;]
			  Response: [AOPEDK;ANY;DG{group};KN{remaining};AP{tr34_blob};KA{rkms_sig};RG1;CE1;]
			  ```
			- **Key Topics:**
				- Minimal request construction
				- Response parsing — multiple tokens
				- AP token — TR-34 blob (hex encoded)
				- KA token — RKMS signature
				- KN — remaining key count
				- Hex decoding for binary data
			- **Exercise:** Implement `PedkRequest` and `PedkResponse`. Parse a documented PEDK response.
		- Day 16: PEDV — Key Verification
		  collapsed:: true
			- **Rust Concepts:** ASN.1 structures, signature bytes, complex token handling
			- **RKI Component:** `commands/pedv.rs` — `PedvRequest`, `PedvResponse`
			- **Protocol Context:**
			  ```
			  Request:  [AOPEDV;KV{asn1};KA{terminal_sig};CE2;SA64;RD{device_nonce};]
			  Response: [AOPEDV;BBY;KA{rkms_sig};RD{nonce};CE2;SA64;]
			  ```
			- **Key Topics:**
				- KV token — ASN.1 structure (hex)
				- KA token — terminal signature
				- BB code parsing (Y/N)
				- SA token — salt length for PSS
				- RD token — device nonce
				- Signature verification data layout
			- **Exercise:** Implement `PedvRequest` and `PedvResponse`. Test with documented example.
		- Day 17: RSA Key Generation and `PKCS#10` CSR
		  collapsed:: true
			- **Rust Concepts:** `rsa` crate, `sha2`, DER encoding, cryptographic randomness
			- **RKI Component:** `crypto/keypair.rs`, `crypto/csr.rs`
			- **Protocol Context:** Terminal generates RSA keypair on-device. Public key exported in PKCS#10 CSR. Private key never leaves terminal.
			- **Key Topics:**
				- RSA keypair generation (2048-bit)
				- RSA public/private key types
				- PKCS#10 CSR structure
				- Subject DN construction (CN=serial)
				- Self-signature for proof of possession
				- DER encoding
			- **Exercise:** Generate RSA keypair and create PKCS#10 CSR. Verify CSR signature.
		- Day 18: CA Integration — CSR Submission
		  collapsed:: true
			- **Rust Concepts:** REST refinement, retry logic, UUID handling, async reqwest
			- **RKI Component:** `ca_client.rs` — `submit_csr()`, `approve_request()`, `fetch_certificate()`
			- **Protocol Context:**
			  ```
			  POST /api/v2/x509/requests
			  POST /api/v2/x509/requests/{uuid}/approve
			  GET  /api/v2/x509/{uuid}
			  ```
			- **Key Topics:**
				- POST request with JSON body
				- Base64 encoding of CSR
				- Request UUID tracking
				- Polling for certificate availability
				- Error handling for HTTP status codes
				- Bearer token authentication
			- **Exercise:** Implement CA client methods. Test against mock CA.
		- Day 19: Certificate Storage and Management
		  collapsed:: true
			- **Rust Concepts:** `Arc` for shared state, file I/O, `zeroize` for sensitive data
			- **RKI Component:** `cert_store.rs` — `CertStore`, `DeviceCertRecord`
			- **Protocol Context:** TMS stores device certificates, issued dates, expiry dates. Used in PEDI `CD` token.
			- **Key Topics:**
				- `Arc<RwLock<CertStore>>` for shared access
				- Certificate file persistence (DER/PEM)
				- Device serial → certificate mapping
				- Expiry tracking
				- Zeroization of private keys in memory
				- CRUD operations
			- **Exercise:** Implement `CertStore`. Test store and retrieve operations.
		- Day 20: Certificate Lifecycle Integration
		  collapsed:: true
			- **Rust Concepts:** Orchestration, async sequencing, error propagation, `select!`
			- **RKI Component:** `orchestrator.rs` — `run_certificate_lifecycle()`
			- **Protocol Context:** Complete flow: terminal generates keypair → CSR → TMS submits to CA → cert retrieved → stored → delivered to terminal.
			- **Key Topics:**
				- Orchestrating multiple async steps
				- Error handling across the full flow
				- Terminal channel coordination
				- CA channel requests
				- Timeout handling
				- Integration test with mock CA and mock terminal
			- **Exercise:** Implement complete certificate lifecycle. Write integration test.
		- **Milestone 2 (Day 20):** Terminal (simulated) generates RSA keypair, produces CSR, TMS submits to mock CA, retrieves signed certificate, stores it. Integration test passes. ADR-0004 (zero-trust key storage) written.
		  
		  ---
	- PART III — KEY DELIVERY: SESSION MANAGEMENT (Days 21–30)
	  collapsed:: true
		- **Theme:** Advanced async + Session state + Key delivery start
		- Day 21: Session Management and JWT Storage
		  collapsed:: true
			- **Rust Concepts:** `Arc<RwLock<T>>`, lifetime management, `chrono`, state enums
			- **RKI Component:** `session.rs` — `RkiSession`, `SessionState`
			- **Protocol Context:** RKLG returns JWT valid for 10 minutes. Refreshed automatically in the last minute.
			- **Key Topics:**
				- Session state enum (`Unauthenticated`, `Authenticated`, `Expired`)
				- JWT storage and expiry tracking
				- Thread-safe session sharing
				- Automatic refresh logic
				- Session invalidation
			- **Exercise:** Implement `RkiSession` with expiry tracking. Test refresh logic.
		- Day 22: Host Connection Pooling
		  collapsed:: true
			- **Rust Concepts:** Connection lifecycle, reconnection, keep-alive, resource management
			- **RKI Component:** `connection.rs` — `HsmConnection`
			- **Protocol Context:** Host Channel is a persistent TLS connection. Must handle disconnects, reconnects, and session re-authentication.
			- **Key Topics:**
				- Connection state management
				- TLS session reuse
				- Reconnection with exponential backoff
				- Connection pooling for multiple concurrent sessions
				- Graceful shutdown
			- **Exercise:** Implement `HsmConnection` with reconnection. Test connection drop and recovery.
		- Day 23: RKLG Integration — Full Authentication Flow
		  collapsed:: true
			- **Rust Concepts:** End-to-end command execution, response validation
			- **RKI Component:** `orchestrator.rs` — `authenticate_hsm()`
			- **Protocol Context:** Complete RKLG flow — both API Key and PKI modes.
			- **Key Topics:**
				- Command execution over Host Channel
				- Response parsing and validation
				- Session establishment
				- Role verification
				- Error handling for failed authentication
			- **Exercise:** Implement `authenticate_hsm()`. Test both auth modes against mock.
		- Day 24: PEDI Integration — Device Identification
		  collapsed:: true
			- **Rust Concepts:** Token construction from cert store, chain handling
			- **RKI Component:** `orchestrator.rs` — `identify_device()`
			- **Protocol Context:** Build PEDI request from stored certificate. Send over Host Channel. Receive RKMS nonce and signing cert.
			- **Key Topics:**
				- Certificate retrieval from store
				- Chain certificate inclusion
				- Request serialization
				- Response parsing — CC and RD
				- 30-second clock start
			- **Exercise:** Implement `identify_device()`. Test against mock CryptoHub.
		- Day 25: The 30-Second Clock
		  collapsed:: true
			- **Rust Concepts:** `tokio::time::timeout`, `Instant`, race conditions
			- **RKI Component:** `resilience.rs` — `OnePassClock`
			- **Protocol Context:** After PEDI response, TMS has 30 seconds to send PEDK. Exceeding this invalidates the session.
			- **Key Topics:**
				- `Instant` for elapsed time tracking
				- `tokio::time::timeout` for enforcing limits
				- Race condition prevention
				- Graceful timeout handling
				- Configurable timeout (default 30s)
			- **Exercise:** Implement `OnePassClock`. Test timeout enforcement.
		- Day 26: Error Recovery and Retry
		  collapsed:: true
			- **Rust Concepts:** Exponential backoff, jitter, retry policies
			- **RKI Component:** `resilience.rs` — `RetryPolicy`, `with_retry()`
			- **Protocol Context:** Network failures, timeouts, and transient errors require retry logic.
			- **Key Topics:**
				- Retry policy configuration
				- Exponential backoff with jitter
				- Retryable vs non-retryable errors
				- Maximum retry count
				- Circuit breaker pattern
			- **Exercise:** Implement `RetryPolicy`. Test retry with mock failures.
		- Day 27: Mock CryptoHub Server
		  collapsed:: true
			- **Rust Concepts:** Test doubles, scriptable responses, TCP server
			- **RKI Component:** `tests/common/mock_cryptohub.rs`
			- **Protocol Context:** A mock CryptoHub that implements RKL v3 Mode 2 commands for testing.
			- **Key Topics:**
				- TCP server with TLS
				- Scriptable response sequences
				- Command dispatch (RKLG, PEDI, PEDK, PEDV)
				- Configurable error injection
				- Logging of received commands
			- **Exercise:** Build mock CryptoHub. Test RKLG and PEDI against it.
		- Day 28: Integration — PEDI with Mock CryptoHub
		  collapsed:: true
			- **Rust Concepts:** Multi-process testing, real TCP, full integration
			- **RKI Component:** `tests/integration/key_delivery_tests.rs`
			- **Protocol Context:** Full PEDI flow against mock CryptoHub.
			- **Key Topics:**
				- Integration test setup
				- Spawning mock CryptoHub
				- Establishing Host Channel
				- Executing PEDI
				- Verifying response
			- **Exercise:** Write integration test for PEDI. Pass with real TCP connection.
		- Day 29: Audit Logging
		  collapsed:: true
			- **Rust Concepts:** `tracing`, structured events, JSON output
			- **RKI Component:** `audit.rs` — `AuditLogger`, `AuditEvent`
			- **Protocol Context:** PCI DSS requires audit logging of all key injections without key material.
			- **Key Topics:**
				- `tracing` crate
				- Structured events with fields
				- JSON formatting for machine parsing
				- Audit event types (injection, authentication, error)
				- Log level configuration
			- **Exercise:** Implement `AuditLogger`. Log a complete ceremony. Verify no key material in logs.
		- Day 30: Module Organization Review
		  collapsed:: true
			- **Rust Concepts:** Visibility, `pub(crate)`, module boundaries, API design
			- **RKI Component:** Full codebase refactor
			- **Protocol Context:** Ensure clean separation of concerns for maintainability.
			- **Key Topics:**
				- Module visibility (`pub`, `pub(crate)`, private)
				- Public API design
				- Internal vs external interfaces
				- `cargo clippy` for linting
				- Code review practices
			- **Exercise:** Run `cargo clippy`. Fix all warnings. Review public API.
		- **Milestone 3 (Day 30):** TMS authenticates to mock CryptoHub (both API Key and PKI modes), executes PEDI, receives RKMS nonce and signing cert, enforces 30-second clock. Audit logs capture all events. ADR-0005 (command trait design) written.
	- PART IV — KEY DELIVERY: THE CEREMONY (Days 31–38)
	  collapsed:: true
		- **Theme:** Cryptography deep dive + The one-pass ceremony
		- Day 31: TR-34 One-Pass Structure
		  collapsed:: true
			- **Rust Concepts:** ASN.1 decoding, binary formats, `x509-parser`
			- **RKI Component:** `crypto/tr34.rs` — `Tr34Blob` parser
			- **Protocol Context:** AP token contains TR-34 one-pass structure: encrypted target key, encrypted ephemeral key, signature.
			- **Key Topics:**
				- ASN.1 DER encoding
				- TR-34 structure layout
				- Parsing nested structures
				- Hex decoding for AP token
				- Field extraction
			- **Exercise:** Parse a TR-34 blob from documented example. Verify structure.
		- Day 32: RKMS Signature Verification (KA on PEDK)
		  collapsed:: true
			- **Rust Concepts:** RSA signature verification, `ring` APIs
			- **RKI Component:** `crypto/signature.rs` — `verify_rkms_signature()`
			- **Protocol Context:** KA signs: `VS + MD + AN(PEDI) + DG + KN + AP + RG + CE + SA + RD(PEDI nonce)`
			- **Key Topics:**
				- RSA signature formats (`PKCS#1 v1.5`, PSS)
				- Public key extraction from CC cert
				- Signature verification with `ring`
				- Signature data construction
				- Constant-time verification
			- **Exercise:** Verify KA signature from documented PEDK response.
		- Day 33: Ephemeral Key Decryption (Terminal Side)
		  collapsed:: true
			- **Rust Concepts:** RSA-OAEP decryption, AES/TDES unwrapping
			- **RKI Component:** `crypto/decrypt.rs` — key unwrap
			- **Protocol Context:** Terminal decrypts ephemeral key using its RSA private key, then decrypts target key using ephemeral key.
			- **Key Topics:**
				- RSA-OAEP decryption
				- AES/TDES key unwrapping
				- Key schedule derivation
				- Constant-time operations
				- Error handling for decryption failures
			- **Exercise:** Implement key unwrap. Test with known test vectors.
		- Day 34: KCV Computation
		  collapsed:: true
			- **Rust Concepts:** TDES/AES ECB modes, constant-time comparison
			- **RKI Component:** `crypto/kcv.rs` — `compute_kcv()`
			- **Protocol Context:** Terminal computes KCV: encrypt zero bytes, take first 3 bytes.
			- **Key Topics:**
				- TDES/AES ECB mode encryption
				- Zero-byte encryption
				- Key Check Value (KCV) extraction
				- Constant-time comparison for verification
				- KCV format for PEDV
			- **Exercise:** Implement `compute_kcv()`. Test with documented examples.
		- Day 35: PEDV — Terminal Signature Generation
		  collapsed:: true
			- **Rust Concepts:** `PKCS#1` PSS, ASN.1 KV structure
			- **RKI Component:** `crypto/signature.rs` — `build_pedv_signature()`
			- **Protocol Context:** Terminal KA signs: `KV + device_nonce + rkms_nonce + CE + SA`
			- **Key Topics:**
				- PSS signature generation
				- ASN.1 KV structure construction
				- Nonce concatenation
				- Signature data layout
				- Salt length handling
			- **Exercise:** Generate PEDV signature. Verify against documented example.
		- Day 36: Complete Key Ceremony Integration
		  collapsed:: true
			- **Rust Concepts:** Full orchestration, all commands in sequence
			- **RKI Component:** `orchestrator.rs` — `run_key_delivery()`
			- **Protocol Context:** RKLG → PEDI → PEDK → Terminal → PEDV
			- **Key Topics:**
				- Orchestrating multi-step ceremony
				- State machine for ceremony progress
				- Error handling at each step
				- Timeout enforcement throughout
				- Audit logging at each step
			- **Exercise:** Implement `run_key_delivery()`. Test against mock.
		- Day 37: Terminal Simulator — Full KRD Behavior
		  collapsed:: true
			- **Rust Concepts:** Separate binary, state machine, complete implementation
			- **RKI Component:** `src/bin/terminal.rs`
			- **Protocol Context:** Terminal receives TR-34 blob, verifies signature, decrypts key, computes KCV, signs PEDV.
			- **Key Topics:**
				- Separate binary with clap CLI
				- Terminal channel TLS client
				- Key storage in memory only
				- Full KRD state machine
				- KCV computation and return
				- PEDV signature generation
			- **Exercise:** Build complete terminal simulator. Test full ceremony.
		- Day 38: End-to-End Key Delivery Integration
		  collapsed:: true
			- **Rust Concepts:** Full system integration, multi-process testing
			- **RKI Component:** `tests/integration/end_to_end_tests.rs`
			- **Protocol Context:** Complete ceremony with mock CryptoHub and terminal simulator.
			- **Key Topics:**
				- Multi-process integration testing
				- Mock CryptoHub + terminal simulator
				- Complete ceremony execution
				- Response validation at each step
				- End-to-end assertion
			- **Exercise:** Write complete end-to-end test. Pass with real TCP connections.
		- **Milestone 4 (Day 38):** Complete key injection ceremony works end-to-end. Terminal receives TR-34 blob, verifies signature, decrypts key, computes KCV, signs PEDV. Mock CryptoHub returns BB=Y with valid signature.
	- PART V — OPERATIONAL EXCELLENCE (Days 39–44)
	  collapsed:: true
		- **Theme:** Production hardening + Observability + Security
		- Day 39: Failure Mode Testing
		  collapsed:: true
			- **Rust Concepts:** Fault injection, edge cases, property testing
			- **RKI Component:** `tests/unit/failure_modes.rs`
			- **Protocol Context:** AN=I (key not cleared), AN=L (device locked), AN=N (device not found), timeout, invalid cert.
			- **Key Topics:**
				- Fault injection techniques
				- Testing error paths
				- Edge case identification
				- Property-based testing concepts
				- Test coverage analysis
			- **Exercise:** Write failure mode tests for all AN codes. Verify error handling.
		- Day 40: Concurrency and Load Handling
		  collapsed:: true
			- **Rust Concepts:** `JoinSet`, backpressure, rate limiting
			- **RKI Component:** `metrics.rs` — throughput tracking
			- **Protocol Context:** Multiple concurrent terminal sessions. Host connection pooling.
			- **Key Topics:**
				- `JoinSet` for concurrent tasks
				- Backpressure mechanisms
				- Rate limiting
				- Metrics collection
				- Performance monitoring
			- **Exercise:** Implement concurrent session handling. Test with 100 simulated terminals.
		- Day 41: PCI DSS Compliance
		  collapsed:: true
			- **Rust Concepts:** Documentation as code, compliance mapping
			- **RKI Component:** `docs/PCI_COMPLIANCE.md`
			- **Protocol Context:** PCI DSS requirements for key management, TLS, audit logging.
			- **Key Topics:**
				- PCI DSS requirement mapping
				- Key management controls
				- TLS configuration requirements
				- Audit log requirements
				- Compliance documentation
			- **Exercise:** Complete PCI compliance documentation. Map controls to implementation.
		- Day 42: Zeroization and Key Material Safety
		  collapsed:: true
			- **Rust Concepts:** `zeroize`, memory hygiene, `secrecy` patterns
			- **RKI Component:** `crypto/zeroize.rs`
			- **Protocol Context:** Key material must never persist in memory. Zeroize on drop.
			- **Key Topics:**
				- `Zeroize` and `ZeroizeOnDrop` traits
				- Memory hygiene practices
				- Key material in structs
				- Preventing accidental copies
				- Testing zeroization
			- **Exercise:** Implement zeroization for all key-bearing types. Test memory clearing.
		- Day 43: Notification and Alerting (EXNO)
		  collapsed:: true
			- **Rust Concepts:** Webhook clients, async dispatch, delivery tracking
			- **RKI Component:** `notifications.rs` — `ExnoDispatcher`
			- **Protocol Context:** RKL Notification Servers receive EXNO notifications on success/failure.
			- **Key Topics:**
				- Webhook client implementation
				- Async notification dispatch
				- Delivery tracking and retry
				- Notification payload format
				- Error handling for failed notifications
			- **Exercise:** Implement `ExnoDispatcher`. Test notification delivery.
		- Day 44: Security Assessment
		  collapsed:: true
			- **Rust Concepts:** Architecture review, attack surface analysis
			- **RKI Component:** `docs/THREAT_MODEL.md`
			- **Protocol Context:** STRIDE analysis of all three channels, key material flows, and trust boundaries.
			- **Key Topics:**
				- STRIDE methodology
				- Attack surface identification
				- Trust boundary definition
				- Threat mitigation strategies
				- Security documentation
			- **Exercise:** Complete threat model. Identify and document all threats.
		- **Milestone 5 (Day 44):** System survives failure injection, handles concurrent sessions, zeroizes keys, dispatches notifications. Threat model and PCI compliance documentation complete.
	- PART VI — PRODUCTION READINESS (Days 45–50)
	  collapsed:: true
		- **Theme:** Hardening, documentation, final demonstration
		- Day 45: Full Lifecycle Test
		  collapsed:: true
			- **Rust Concepts:** State machine verification, complete flows
			- **RKI Component:** `tests/integration/lifecycle_tests.rs`
			- **Protocol Context:** New → rotate → revoke → decommission.
			- **Key Topics:**
				- Lifecycle state machine
				- Rotation flow testing
				- Revocation flow testing
				- Decommission flow testing
				- Complete lifecycle verification
			- **Exercise:** Write lifecycle tests. Verify all transitions.
		- Day 46: Performance Optimization
		  collapsed:: true
			- **Rust Concepts:** Profiling, benchmarking, `criterion`
			- **RKI Component:** `benches/protocol_bench.rs`
			- **Protocol Context:** Message parsing, TLS handshake, signature verification performance.
			- **Key Topics:**
				- Benchmarking with `criterion`
				- Profiling with `perf`/`flamegraph`
				- Hot path optimization
				- Memory allocation reduction
				- Performance measurement
			- **Exercise:** Write benchmarks. Optimize hot paths.
		- Day 47: Documentation — rustdoc and API Guide
		  collapsed:: true
			- **Rust Concepts:** Documentation design, examples, doctests
			- **RKI Component:** `docs/API.md`, rustdoc for all public items
			- **Protocol Context:** Public API documentation for TMS application developers.
			- **Key Topics:**
				- rustdoc comments and examples
				- Doctests as executable examples
				- API guide structure
				- Usage examples
				- Documentation completeness
			- **Exercise:** Complete API documentation. Ensure all doctests pass.
		- Day 48: Final Integration Suite
		  collapsed:: true
			- **Rust Concepts:** Test matrix, coverage, CI readiness
			- **RKI Component:** Complete test suite
			- **Protocol Context:** All tests passing — unit, integration, end-to-end.
			- **Key Topics:**
				- Test matrix design
				- Code coverage analysis
				- CI pipeline configuration
				- Test isolation
				- Deterministic testing
			- **Exercise:** Run complete test suite. Achieve all green.
		- Day 49: Demo Preparation
		  collapsed:: true
			- **Rust Concepts:** Scripting, runbooks, demonstration
			- **RKI Component:** `scripts/demo.sh`, `docs/OPERATIONS.md`
			- **Protocol Context:** End-to-end demonstration of certificate lifecycle and key delivery.
			- **Key Topics:**
				- Demo script creation
				- Operational runbook
				- Troubleshooting guide
				- Deployment documentation
				- Demonstration checklist
			- **Exercise:** Create demo script. Run complete demonstration.
		- Day 50: Final Milestone Demo and Retrospective
		  collapsed:: true
			- **Rust Concepts:** Architecture review, lessons learned
			- **RKI Component:** `docs/RETROSPECTIVE.md`, live demonstration
			- **Protocol Context:** Complete system demonstration.
			- **Key Topics:**
				- Final demonstration
				- Architecture review
				- Lessons learned
				- Future improvements
				- Celebration
			- **Exercise:** Deliver final demo. Complete retrospective.
		- **Milestone 6 (Day 50):** Complete, production-ready RKI core module. All tests pass. Documentation complete. Demo runs. You are now a Rust systems programmer with payment HSM integration experience.
		  
		  ---
- RKI CORE MODULE : A Journey