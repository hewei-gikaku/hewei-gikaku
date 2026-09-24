<h1 align="center">HE WEI（ギカク / Gikaku）</h1>

<p align="center">
  <strong>Red Teaming · Vulnerability Research · Cloud & Identity Security</strong>
</p>

<p align="center">
  <a href="https://gikaku.net">
    <img src="https://img.shields.io/badge/Blog-gikaku.net-21759B?style=flat-square&logo=wordpress&logoColor=white"/>
  </a>
  <img src="https://img.shields.io/badge/CVEs-50%2B-DD0000?style=flat-square"/>
  <img src="https://img.shields.io/badge/MSRC-2027_Special_Mention-0078D4?style=flat-square&logo=microsoft&logoColor=white"/>
  <img src="https://img.shields.io/badge/Google-Bug_Hunters_Leaderboard-4285F4?style=flat-square&logo=google&logoColor=white"/>
</p>

---

## About

I am a Japan-based **Red Team Analyst at Sophos**, working within the **Sophos Counter Threat Unit (CTU™) Adversary Group (SwAG)**.

My professional work includes threat-led penetration testing, adversary simulation, and full-scope red team engagements for enterprise clients in Japan and internationally.

I work across Active Directory, hybrid identity, Azure and Microsoft Entra ID, AWS, OCI, initial access, phishing simulation, C2 infrastructure, privilege escalation, lateral movement, EDR/XDR evasion, Web and API security, source code review, and exploit development.

Alongside client work, I conduct independent vulnerability research across **macOS, the Linux kernel, virtualization, identity systems, cloud security, and AI infrastructure**.

My research has contributed to **50+ assigned CVEs**, including vulnerabilities in Apple macOS, Linux, Google security projects, Keycloak, libvirt, Docker MCP Gateway, IBM ContextForge, and official Model Context Protocol SDKs.

I have also authored security fixes accepted upstream into the **Linux kernel** and **libvirt**.

My research tends to revolve around one question:

> **What does a system trust, and what happens when that trust crosses the wrong boundary?**

Technical write-ups and research notes are published at [gikaku.net](https://gikaku.net).

---

## Professional Focus

| Red Team | Cloud & Identity | Vulnerability Research |
|---|---|---|
| TLPT / Threat-Led PT | Azure / Entra ID | Source code review |
| Adversary simulation | AWS | Native code |
| Active Directory | OCI | macOS |
| Initial access / phishing | GCP research | Linux kernel |
| C2 infrastructure | IAM privilege escalation | Virtualization / libvirt |
| Privilege escalation | Credential and token abuse | Identity systems |
| Lateral movement | Hybrid identity | MCP / AI infrastructure |
| EDR/XDR evasion | Cloud attack paths | PoC development |

---

## Featured Vulnerability Research

| CVE | Project | Research |
|---|---|---|
| **CVE-2026-64704** | **Apple macOS SMB** | Type confusion in the macOS SMB implementation. Apple credits me alongside Cisco Talos, Calif.io, and other researchers. CISA-ADP assigned CVSS v3.1 **9.8 Critical**. |
| **CVE-2026-65374** | **Apple macOS WebDAV** | Memory corruption reachable through a malicious WebDAV server. Apple's published impact states that exploitation may result in **code execution**. CISA-ADP assigned CVSS v3.1 **8.8 High**. |
| **CVE-2026-90048** | **Linux Kernel NTFS3** | Slab out-of-bounds write in `ni_create_attr_list()`. I discovered the issue, reproduced the heap corruption under KASAN, and authored the fix accepted upstream. |
| **CVE-2026-19202** | **Google mcp-toolbox-sdk-python** | Cross-audience Google ID token reuse caused by an audience-unaware token cache. Assigned CVSS v4.0 **9.1 Critical**. |
| **CVE-2026-53709** | **IBM ContextForge** | Authentication failure that could permit unauthenticated platform-administrator access in affected default configurations. CVSS v3.1 **9.8 Critical**. |
| **CVE-2026-76092** | **Docker MCP Gateway** | Authentication enforcement failure in the gateway execution path. CVSS v4.0 **9.2 Critical**. |
| **CVE-2026-63622** | **libvirt** | Host-side privilege boundary issue involving `swtpm` state handling and privileged filesystem ownership operations. CVSS v3.1 **7.8 High**. |
| **CVE-2026-92358** | **Keycloak** | Residual account-link proof could be reused to silently re-establish a removed external identity link. CVSS v3.1 **6.4 Medium**. |

---

## macOS Security Research

### CVE-2026-64704 · SMB Type Confusion

One of my primary macOS findings is a type confusion vulnerability in the SMB implementation.

Apple addressed the issue through improved memory handling and credits **He Wei（ギカク）** alongside researchers from Cisco Talos, Calif.io, and other independent researchers.

Apple's published impact states that an application may be able to cause unexpected system termination. CISA-ADP separately assigned the vulnerability a CVSS v3.1 score of **9.8 Critical**.

### CVE-2026-65374 · WebDAV Memory Corruption

I reported a memory-corruption vulnerability in macOS WebDAV where connecting to a malicious WebDAV server may result in **code execution**, according to Apple's published impact.

Apple credits me alongside Bruce Dang of Calif.io.

My other Apple-credited findings include:

- **CVE-2026-43677** · WebDAV heap out-of-bounds write
- **CVE-2026-43806** · `mDNSResponder` memory-safety issue

[Apple security advisory for CVE-2026-64704](https://support.apple.com/en-us/128067)

[CVE-2026-65374 technical write-up](https://gikaku.net/2026/09/15/cve-2026-65374/)

---

## Linux Kernel Research

### CVE-2026-90048 · NTFS3 Slab Out-of-Bounds Write

I discovered a slab out-of-bounds write in the Linux NTFS3 filesystem's `ni_create_attr_list()` path.

The vulnerable implementation allocated an attribute-list buffer according to the size of the original MFT record even though individual attributes could expand when converted into `ATTR_LIST_ENTRY` representations.

A crafted NTFS record could therefore cause the generated list to exceed the allocated heap object.

I reproduced the issue under KASAN and authored the fix accepted upstream into the Linux kernel.

Other Linux kernel findings for which I authored upstream fixes include:

- **CVE-2026-64339** · `usbio` out-of-bounds read
- **CVE-2026-68326** · `mwifiex` out-of-bounds read
- **CVE-2026-68402** · `cfg80211` out-of-bounds read

[CVE-2026-90048 technical write-up](https://gikaku.net/2026/09/17/cve-2026-90048/)

[Upstream Linux kernel fix](https://github.com/torvalds/linux/commit/7c4841e2a62794a3bab7c1ff0540580f387e377f)

---

## Cloud and Identity Research

### CVE-2026-19202 · Cross-Audience Google ID Token Reuse

I discovered a credential-boundary vulnerability in Google's `mcp-toolbox-sdk-python`.

The SDK cached Google ID tokens without including the requested audience in the cache key.

When one process authenticated to multiple services, a valid token created for one audience could therefore be returned for another audience and disclosed to the wrong service.

The vulnerability was assigned **CVSS v4.0 9.1 Critical**.

The interesting part of this class of issue is that the credential itself remains cryptographically valid. The security failure occurs because a valid credential crosses an audience boundary where it does not belong.

### Keycloak

My Keycloak research focuses on federated identity and account-linking proof lifecycles.

- **CVE-2026-82968** · Verification proof insufficiently bound to the external identity being linked
- **CVE-2026-92358** · Residual account-link proof remained usable after linking or manual unlinking

Both were assigned **CVSS v3.1 6.4** by Red Hat.

These findings examine whether an identity proof remains valid only for the identity, account, session, and authentication stage for which it was originally created.

---

## MCP and AI Infrastructure

I have conducted extensive vulnerability research into the Model Context Protocol ecosystem and the security boundaries created when AI systems interact with tools, credentials, local resources, and remote services.

### Google MCP Toolbox

Reported vulnerabilities include:

`CVE-2026-11719` · `CVE-2026-14537` · `CVE-2026-14538` · `CVE-2026-14539` · `CVE-2026-14541` · `CVE-2026-16481`

The findings covered authorization consistency, OAuth validation, BigQuery access controls, request handling, SSRF, and cloud credential exposure.

### Docker MCP Gateway

Reported four vulnerabilities:

`CVE-2026-76092` · `CVE-2026-76093` · `CVE-2026-76094` · `CVE-2026-76095`

The issues involved authentication enforcement, image signature verification, arbitrary host file access, and SSRF.

### IBM ContextForge

Reported:

- **CVE-2026-53709** · Authentication failure allowing platform-administrator access in affected default configurations
- **CVE-2026-53708** · DNS time-of-check/time-of-use weakness affecting SSRF protection

### MCP Ecosystem

Additional research includes vulnerabilities affecting:

- Official MCP Python SDK
- Official MCP Kotlin SDK
- Official MCP Ruby SDK
- Official MCP Rust SDK
- GitHub MCP Server
- HashiCorp Consul MCP Server
- Contentful MCP Server
- dbt Labs MCP Server

Representative findings include:

`CVE-2026-52869` · `CVE-2026-63658` · `CVE-2026-63118` · `CVE-2026-63128` · `CVE-2026-67430` · `CVE-2026-67432` · `CVE-2026-48529` · `CVE-2026-16328` · `CVE-2026-53957` · `CVE-2026-44968` · `CVE-2026-44969` · `CVE-2026-44970`

---

## Virtualization Research

I reported two vulnerabilities in **libvirt** and authored fixes accepted upstream.

### CVE-2026-63622

A confined `swtpm` process could influence filesystem ownership operations subsequently performed by the privileged libvirt daemon.

The issue crossed a host-side privilege boundary and could result in local privilege escalation.

Red Hat assigned **CVSS v3.1 7.8 High**.

### CVE-2026-63623

A temporary file-permission window during storage-volume cloning and conversion could expose guest disk contents to another local user.

Red Hat assigned **CVSS v3.1 5.5 Medium**.

---

## Recognition

| Program / Organization | Recognition |
|---|---|
| **Microsoft Security Response Center** | HE WEI · 2027 Special Mention |
| **Google Bug Hunters** | Leaderboard |
| **Apple Security** | CVE-2026-64704 · CVE-2026-65374 · CVE-2026-43677 · CVE-2026-43806 |
| **Red Hat / Keycloak** | CVE-2026-82968 · CVE-2026-92358 |
| **Red Hat / libvirt** | CVE-2026-63622 · CVE-2026-63623 |
| **Linux Kernel** | CVE-2026-90048 · CVE-2026-64339 · CVE-2026-68326 · CVE-2026-68402 |
| **IPA** | 7 vulnerability acknowledgements |

Other responsible disclosure acknowledgements include **Rakuten, Mercari, BANDAI NAMCO, Sky, Neo4j, MISP Project, SBI Holdings, Seven Bank, Yahoo Japan, Keysight, Lumen, Clarivate**, and others.

---

## Speaking and Writing

### Security Management Conference 2026 Summer

[**The Code We Trust Most Is the Code We Scrutinize Least: A Real-World CVE Exposes a Blind Spot in AI Agent Integration (MCP)**](https://www.sbbit.jp/eventinfo/security-management-online)

A talk on a disclosed MCP authorization vulnerability and the attack paths introduced when trusted AI integrations connect to enterprise systems.

### Sophos Research

[**In code we trust? Why the most trusted software receives the least scrutiny.**](https://www.sophos.com/en-us/blog/sophos-trusted-code)

Research on source review, variant hunting, patch-gap analysis, and coordinated vulnerability disclosure.

---

## Selected Certifications

**OSEP · OSWE · OSCP · CARTP · AWS Certified Security - Specialty**

Currently working toward **OSED** and the **OSCE3** certification path.

---

## Current Focus

| Area | Current Work |
|---|---|
| Red Team | TLPT · Active Directory · Initial access · C2 · EDR/XDR evasion |
| Cloud | GCP · Azure / Entra ID · AWS · OCI · IAM · Cloud attack paths |
| Identity | Authentication · Federation · Credentials · Token boundaries |
| Systems | macOS · Linux kernel · libvirt · Native code |
| MCP / AI | MCP servers · SDKs · Authentication · Authorization · Isolation |
| Research | Source code review · Dynamic analysis · Patch analysis · PoC development |
| Sharing | Vulnerability write-ups · Conference talks · Coordinated disclosure |

---

<p align="center">
  <strong>Technical write-ups and research notes</strong><br>
  <a href="https://gikaku.net">gikaku.net</a>
</p>
