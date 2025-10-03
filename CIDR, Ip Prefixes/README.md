#  CIDR and IP Prefixes: Defining Your Target's Network Scope
<img width="1280" height="720" alt="CIDR" src="https://github.com/user-attachments/assets/458d0ebc-5adb-4d50-b28f-bd25ac8c306e" />


Just as the internet is organized by Autonomous Systems (AS), the addresses inside those systems are organized using **Classless Inter-Domain Routing (CIDR)**, also known as **IP Prefixes**.

For anyone doing network security, especially **bug bounty hunters**, understanding CIDR is essential. It’s how you define the exact boundaries of a target's network.

---

## What is an IP Address?

An **IP address** is a unique number assigned to every device connected to the network. The older, class-based system of allocating these addresses was inefficient, leading to wasted IPs and routing complexity.

## What is CIDR? (Classless Inter-Domain Routing)

**CIDR** is the modern, flexible way to manage and allocate IP addresses.

* **Definition:** A method for grouping IP addresses together flexibly to reduce waste and improve routing efficiency.
* **Notation:** An IP address followed by a slash and a number.

    * **Example:** `192.168.1.0/24`
        * The `/24` is the **CIDR Mask** (or prefix length). It dictates the size of the network block.

##  What is an IP Prefix (or Subnet)?

The **IP Prefix** is the actual range of IP addresses defined by the CIDR notation.

* **Key Concept:** The number after the slash (`/`) is the most important part. A smaller number means a larger network block.
    * `/8`: Huge network (over 16 million IPs)
    * `/24`: Standard network (256 IPs)
    * `/32`: A single IP address

---

## CIDR and Reconnaissance for Bug Hunters

Mastering CIDR is key to effective network reconnaissance. It is the definitive map of the target's network space.

### The CIDR Recon Flow

1.  **Find the ASN:** Get the target organization's ASN.
2.  **Enumerate Prefixes:** Use tools to pull the full list of IP Prefixes (CIDR blocks) announced by that ASN. This is the company's full public network footprint.
3.  **Target Scope Definition:** Every prefix you find is a new set of IPs to investigate. This ensures you include all official infrastructure while also helping you avoid scanning out-of-scope targets (like third-party cloud services).

---

## Practical Tools for CIDR Management

Once you have the prefixes, use these tools to process them:

| Tool/Resource | Type | Simple Function |
| :--- | :--- | :--- |
| **`ipcalc`** | Command Line Tool (CLI) | Calculates the full details of a CIDR block (range, hosts, broadcast, etc.). |
| **`naabu`** (Project Discovery) | Command Line Tool (CLI) | Extremely fast port scanner that can efficiently scan thousands of IPs from a list of CIDR blocks. |
| **`masscan`** | Command Line Tool (CLI) | A super-fast scanner designed for rapidly checking massive IP ranges across the entire internet. |
| **CIDR to IP Converters** | Online Tool | Simple websites to quickly expand a CIDR (e.g., `/24`) into a full list of all 256 individual IP addresses. |

### Bug Bounty Command Example (Chaining Tools):

To scan all CIDR blocks listed in a file named `cidr.txt` for a specific open port (e.g., 8080):

```bash
# Pipes the list of CIDR blocks to the naabu port scanner
cat cidr.txt | naabu -p 8080 -silent
