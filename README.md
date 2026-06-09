# vps-migration-reality-recovery
Personal VPS infrastructure project involving Reality deployment, migration, troubleshooting, and service recovery.
# Reality Service Recovery After VPS Migration

## Project Overview

This project documents the recovery process of a self-hosted Reality-based VPS proxy service after migrating the server to a new VPS node.

The initial service was already functional in Clash, but my main client was Surge, which did not directly support the original VLESS + Reality subscription format. To make the service work with Surge, I used a local Xray layer as a bridge:

```text
Surge
↓
Local Xray
↓
VLESS + Reality
↓
VPS
↓
Internet
```

The project became a practical troubleshooting case involving VPS migration, client-server configuration synchronization, Reality handshake failures, and final service recovery.

## Background

The original setup worked through a subscription link in Clash. However, because I had already purchased and used Surge as my primary network tool, I wanted to adapt the same VPS-based Reality service to work with Surge.

Since Surge could not directly consume the VLESS + Reality configuration in the same way, the solution was to run a local Xray client on macOS and let Surge connect to it through a local SOCKS proxy.

This created the following chain:

```text
macOS Surge
↓
127.0.0.1 local SOCKS proxy
↓
Local Xray client
↓
Reality connection
↓
Remote VPS
```

## Technical Environment

### Server Side

* Linux VPS
* 3x-ui panel
* Xray-core
* VLESS
* Reality
* TCP transport

### Client Side

* macOS
* Surge
* Local Xray
* sing-box was also tested during troubleshooting
* JSON configuration files

### Tools and Methods

* SSH
* Port testing
* Local proxy testing
* Xray logs
* Configuration comparison
* Service restart
* Client/server parameter synchronization

## Timeline

### Stage 1: Initial Working State

The VPS-based Reality service initially worked through Clash-compatible subscription links.

At this stage, the main issue was not whether the server could work, but how to adapt it into a Surge-based workflow.

### Stage 2: Surge Integration

Because Surge did not directly support the original VLESS + Reality link format, a local Xray bridge was introduced.

The role of local Xray was to receive traffic from Surge locally and forward it to the remote VPS through VLESS + Reality.

### Stage 3: VPS Migration

The VPS was later migrated to a new node.

After migration, the server IP changed. Local Xray and Surge-related configuration files had to be updated accordingly.

Basic network tests showed that the new server IP and port were reachable, but the proxy service still failed.

This indicated that the problem was not simply network reachability.

### Stage 4: Failure Symptoms

After migration, the service showed symptoms such as:

* Local Xray could start normally.
* Surge could connect to the local proxy port.
* The VPS IP was reachable.
* The target port was open.
* Public internet access through the proxy failed.
* Xray returned errors such as EOF or connection reset.

This suggested that the failure happened during the Reality/VLESS handshake stage.

### Stage 5: Troubleshooting

Several possible causes were checked:

* Wrong server IP
* Wrong port
* Local Xray not running
* Surge pointing to the wrong local port
* Reality public key mismatch
* Short ID mismatch
* Server Name / SNI mismatch
* SpiderX mismatch
* UUID mismatch
* ML-DSA / quantum verification mismatch
* Service-side configuration not matching the client-side configuration

The most important discovery was that successful TCP connection to the VPS did not mean the Reality connection was valid. The port could be open while the Reality handshake still failed.

### Stage 6: Root Cause

The main issue was configuration inconsistency after migration and repeated regeneration of Reality-related parameters.

The client and server were not using fully matched Reality parameters.

Relevant parameters included:

* UUID
* Public Key
* Short ID
* Server Name
* SpiderX
* ML-DSA verification parameters

### Stage 7: Final Recovery

The service was eventually restored by regenerating and synchronizing the required Reality parameters between the 3x-ui server-side inbound and the local Xray client configuration.

After the corrected configuration was applied, local Xray was restarted and Surge was able to route traffic through the local proxy successfully.

## Key Technical Lessons

### 1. Deployment is easier than recovery

Installing a VPS, Linux, and 3x-ui is relatively simple. The more valuable part of this project was diagnosing why a previously working service failed after migration.

### 2. Port reachability does not equal protocol success

A port can be reachable while the protocol handshake still fails.

In this case, TCP connectivity existed, but the Reality handshake failed because client and server parameters did not match.

### 3. Local proxy chains require clear mental models

The project helped clarify the actual traffic path:

```text
Surge
↓
Local Xray
↓
Reality
↓
VPS
```

This was different from assuming that Surge directly connected to the VPS.

### 4. Configuration drift is a real problem

After migration and parameter regeneration, different parts of the system may no longer match.

This project showed the importance of tracking which configuration is currently active on the server and which configuration is being used locally.

## Skills Demonstrated

* Linux VPS administration
* Remote service troubleshooting
* Xray and Reality configuration
* Surge integration through local proxy
* TCP port testing
* JSON configuration editing
* Client-server configuration synchronization
* Infrastructure migration recovery
* Root-cause analysis

## Screenshots to Add Later

Recommended screenshots:

1. Architecture diagram showing Surge → Local Xray → Reality → VPS.
2. Local Xray configuration with sensitive values redacted.
3. 3x-ui inbound configuration with sensitive values redacted.
4. Error screenshot showing EOF or connection reset.
5. Port test screenshot showing that the VPS port was reachable.
6. Final successful test showing the service restored.

Sensitive information should be removed before publishing:

* Real IP addresses
* Domain names
* UUIDs
* Private keys
* Public keys
* Short IDs
* SpiderX values
* Passwords
* API keys

## Project Outcome

The final result was a working Surge-compatible Reality proxy chain using local Xray as a bridge.

More importantly, this project provided hands-on experience in diagnosing real infrastructure failures after migration, understanding layered proxy architecture, and recovering a service through systematic troubleshooting.


## Architecture

![Architecture](images/architecture.png)

## Reality Handshake Failure

![Failure](images/reality-failure.png)

## Recovery Verification

![Recovery](images/recovery-success.png)
