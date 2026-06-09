# vps-migration-reality-recovery
Personal VPS infrastructure project involving Reality deployment, migration, troubleshooting, and service recovery.
# Reality Service Recovery After VPS Migration

A personal infrastructure project documenting the deployment, migration, troubleshooting, and recovery of a Reality-based VPS proxy service integrated with Surge and Xray.

---

## Architecture & Incident Timeline

![Architecture](images/architecture.png)

The diagram above summarizes the system architecture, VPS migration process, service failure, troubleshooting workflow, and final recovery.

---

## Project Overview

This project documents the recovery process of a self-hosted Reality-based VPS proxy service after migrating the server to a new VPS node.

The service originally worked through Clash-compatible subscriptions. However, my primary client platform was Surge on macOS and iOS, which did not directly support the same VLESS + Reality workflow.

To solve this problem, I deployed a local Xray client on macOS and built a complete proxy chain connecting Surge, Xray, Reality, and a Linux VPS.

The project later evolved into a real-world troubleshooting exercise involving infrastructure migration, client-server configuration synchronization, Reality handshake failures, and service recovery.

---

## Motivation

The original VPS service worked correctly through Clash.

However, because I primarily used Surge as my daily network management tool, I wanted to adapt the service to operate within a Surge-based workflow.

This required:

* Understanding the architecture differences between Clash and Surge.
* Deploying and configuring a local Xray client.
* Building a multi-layer proxy chain.
* Managing both client-side and server-side configurations.
* Troubleshooting protocol-level failures after infrastructure changes.

---

## Technical Environment

### Server Side

* Linux VPS
* Xray-core
* 3x-ui
* VLESS
* Reality
* TCP transport

### Client Side

* macOS
* Surge
* Local Xray Client
* sing-box (used during troubleshooting)

### Supporting Technologies

* SSH
* JSON configuration files
* TCP/IP networking
* SOCKS5
* Port diagnostics
* Remote server administration

---

## System Architecture

The final working architecture followed this flow:

```text
User Applications
        ↓
      Surge
        ↓
 Local Xray Client
        ↓
 VLESS + Reality
        ↓
    Linux VPS
        ↓
 Public Internet
```

Unlike Clash, Surge required a local proxy layer before forwarding traffic through Reality.

This project helped establish a clear understanding of how local proxy software interacts with remote infrastructure.

---

## Migration Event

The VPS was migrated from the original node to a new node using the provider's migration system.

Migration itself was straightforward.

However, after migration:

* Existing configurations no longer functioned correctly.
* Reality connections failed.
* Traffic could no longer reach the remote destination.
* The service became unavailable despite the VPS being online.

This transformed a simple migration task into a full troubleshooting and recovery project.

---

## Failure Symptoms

After migration, several symptoms appeared:

* Local Xray started normally.
* Surge successfully connected to the local SOCKS proxy.
* VPS ports appeared reachable.
* Public internet access through the proxy failed.
* Reality handshakes failed.
* Xray logs returned EOF and connection-related errors.

At this stage, basic network connectivity appeared functional, but protocol-level communication was not.

---

## Investigation Process

Several potential causes were investigated:

### Connectivity

* VPS reachability
* Port accessibility
* Firewall status
* Service status

### Client Configuration

* Local Xray configuration
* Surge proxy settings
* SOCKS5 listener configuration

### Reality Parameters

* UUID
* Public Key
* Short ID
* Server Name (SNI)
* SpiderX
* ML-DSA verification settings

### Service Validation

* Xray inbound configuration
* 3x-ui settings
* Client-server parameter synchronization

---

## Root Cause Analysis

The root cause was ultimately traced to configuration inconsistencies introduced during migration and configuration regeneration.

Although the server itself remained reachable, Reality requires strict matching between client-side and server-side parameters.

Several critical values were no longer synchronized between the local client and the VPS configuration.

As a result:

* TCP connectivity existed.
* The VPS was online.
* The service port was reachable.
* Reality handshakes failed.

This demonstrated that successful network connectivity does not necessarily imply successful protocol negotiation.

---

## Service Recovery

Recovery involved:

* Regenerating Reality-related parameters.
* Updating local client configurations.
* Synchronizing client and server settings.
* Restarting affected services.
* Verifying Reality handshake functionality.
* Testing end-to-end traffic forwarding.

After configuration synchronization was completed, the service returned to a fully operational state.

---

## Key Lessons Learned

### Deployment Is Easier Than Recovery

Installing Linux, deploying Xray, and configuring 3x-ui were relatively straightforward.

The majority of practical learning occurred during troubleshooting and recovery.

### Connectivity Does Not Guarantee Functionality

A reachable server and open port do not guarantee a successful Reality connection.

Protocol-level validation is equally important.

### Configuration Drift Is Dangerous

Infrastructure migrations can introduce subtle inconsistencies between client and server configurations.

Keeping configurations synchronized is critical.

### Layered Architectures Require Clear Mental Models

Understanding the complete traffic path proved essential:

```text
Surge
↓
Local Xray
↓
Reality
↓
Linux VPS
↓
Internet
```

Without understanding each layer, troubleshooting would have been significantly more difficult.

---

## Skills Demonstrated

### Linux Administration

* VPS deployment
* Remote server management
* Service monitoring
* Configuration management

### Networking

* TCP/IP troubleshooting
* Port diagnostics
* Proxy architecture analysis
* Connectivity verification

### Secure Proxy Infrastructure

* Xray
* Reality
* VLESS
* Surge integration
* SOCKS5
* sing-box

### Problem Solving

* Root-cause analysis
* Infrastructure migration recovery
* Configuration synchronization
* Multi-stage debugging

---

## Future Improvements

Potential future work includes:

* Automated configuration backup
* Configuration version control
* Migration validation scripts
* Additional monitoring and logging
* Multi-node deployment testing

---

## Disclaimer

All IP addresses, UUIDs, keys, domains, and sensitive configuration details have been removed or anonymized before publication.
