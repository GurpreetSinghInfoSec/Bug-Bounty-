# ASN Reconnaissance: Mapping the Internet's Attack Surface

You use the internet every day, but have you ever thought about how data finds its way? It's not magic; it's a massive, organized system built on the **Autonomous System (AS)** and its unique ID, the **Autonomous System Number (ASN)**.

For anyone in cybersecurity, especially **bug bounty hunters**, understanding AS and ASN is like having a map to a company’s entire digital kingdom. It's a critical first step in finding new targets.

---

##  What is an Autonomous System (AS)?

Think of the internet as a collection of enormous, independent towns. Each of these "towns" is an **Autonomous System (AS)**.

* **Definition:** An AS is a large network or group of networks managed and controlled by a single entity (like an ISP or a large corporation).
* **Single Policy:** All networks within one AS follow a single, defined **routing policy**. They decide how data enters and leaves their space.
* **The Job:** An AS manages its own set of IP addresses and announces them to the rest of the internet.

##  What is an Autonomous System Number (ASN)?

If the AS is a town, the **ASN** is its unique postal code.

* **Definition:** The ASN is a **unique number** assigned to an Autonomous System. It's the unique identifier other networks use to communicate.
* **Routing:** The **Border Gateway Protocol (BGP)** uses the ASN to figure out the right path for data packets as they hop from one AS to the next across the globe.

---

##  Why ASNs are Gold for Bug Hunters

In **bug bounty hunting**, the initial step is **Reconnaissance** (or "recon"). ASNs are a massive shortcut in this phase.

### The ASN Recon Flow

1.  **Find the Target's ASN:** Start with the company's name or a known IP address.
2.  **Find the IP Space:** Use tools to look up **all** the IP address ranges (**CIDR blocks** or **prefixes**) registered to that ASN.
3.  **Expand the Attack Surface:** This step often reveals hidden targets not listed in the main scope:
    * Development or staging servers
    * Older, forgotten legacy systems
    * Infrastructure from recently acquired companies
    * Internal systems mistakenly exposed

The goal is to move beyond the main website and find a forgotten server on an obscure IP address within their ASN's range—that’s where the high-value bugs often live.

---

## 🛠️ Practical Tools and Resources for ASN Analysis

You don't need fancy paid software to start. Here are the go-to tools:

| Tool/Resource | Type | Simple Function |
| :--- | :--- | :--- |
| **BGPview** (`bgpview.io`) | Website/Online Tool | Look up ASN for an org/IP and see all owned IP ranges (CIDR blocks). |
| **Hurricane Electric BGP Toolkit** (`bgp.he.net`) | Website/Online Tool | Highly reliable source for ASN and associated IP prefixes. |
| **`asnmap`** (Project Discovery) | Command Line Tool (CLI) | Takes an organization name or ASN and quickly spits out all associated IP ranges. Essential for automation. |
| **`whois`** | Command Line Tool (CLI) | A fundamental utility to get basic registration and contact info for an IP or ASN. |

### Bug Bounty Command Example (Using `asnmap`):

To find all IP ranges for a target with ASN **AS12345** and then immediately scan those IPs for active web ports (80 and 443) using `naabu`:

```bash
# Finds IP ranges from ASN, then pipes (sends) the list to the naabu port scanner
asnmap -asn AS12345 | naabu -p 80,443 -silent
