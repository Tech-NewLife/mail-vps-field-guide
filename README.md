# Mail VPS Field Guide

A practical shortlist of infrastructure providers for people who intend to run
their own SMTP server.

This guide does not rank hosts by affiliate payout, and none of the entries is
an endorsement. It asks a narrower question: **what should a mail operator
verify before placing a mail server on a provider's network?**

> [!IMPORTANT]
> A VPS plan does not guarantee successful mail delivery. Account approval,
> outbound SMTP access, reverse DNS, IP reputation, and responsible sending
> practices matter more than headline specifications.

Information was last reviewed on **29 July 2026**. Confirm policies and prices
directly with a provider before ordering.


## Read This First

There are three common deployment paths:

| Path | How mail leaves the VPS | When it makes sense |
| --- | --- | --- |
| Direct delivery | Your MTA connects to destination servers on TCP 25 | You can obtain SMTP approval, manage reputation, and monitor abuse |
| Smarthost relay | Your MTA submits mail to a relay on TCP 465 or 587 | Port 25 is unavailable or deliverability is more important than independence |
| Receive only | The VPS accepts inbound mail but relays outbound mail elsewhere | You want local mailboxes without operating a sending reputation |

If direct delivery is required, obtain written confirmation about outbound port
25 before migrating. An open firewall rule alone does not prove that the
provider permits SMTP traffic at the network level.


## Provider Map

The matrix deliberately avoids a single numerical score. A provider that works
for a personal mailbox may be unsuitable for bulk or transactional traffic.

| Provider | Outbound SMTP starting point | PTR / reverse DNS | Footprint | Operator note |
| --- | --- | --- | --- | --- |
| [InterServer](https://www.interserver.net/) | Blocked;unblocked upon request period | Support ticket | United States | Approval considers the intended email usage, anti-abuse measures, hostname, and reverse DNS configuration.|
| [Vultr](https://www.vultr.com/) | Blocked; support request reviewed case by case | Control panel or CLI | Global | Suitable only after approval; authenticated relay remains the fallback |
| [DigitalOcean](https://www.digitalocean.com/) | Restricted for new accounts | Control panel | Global | Confirm eligibility before choosing it for direct delivery |
| [Akamai Cloud / Linode](https://www.linode.com/) | Account-dependent controls | Control panel | Global | Check current outbound-mail restrictions for the account and region |
| [Hetzner](https://www.hetzner.com/) | Restricted; limit request may be available after account history | Control panel | Europe, US, Asia | Attractive infrastructure pricing, but approval timing must be planned |
| [RackNerd](https://www.racknerd.com/) | Confirm before purchase | Control panel or support | Primarily North America | Annual promotions vary; ask about SMTP policy for the exact product and location |
| [IONOS](https://www.ionos.com/servers/vps) | Blocked; support can review an unblock request | Cloud Panel | US and Europe | IONOS documents FQDN, PTR, and SPF prerequisites |
| [UpCloud](https://upcloud.com/) | Blocked; support request with verification | Control panel or API | Global | Identity or payment verification and a legitimate use case may be required |
| [Scaleway](https://www.scaleway.com/en/virtual-instances/) | Disabled initially; verified accounts can enable SMTP in security groups | Console/API | Europe | KYC must be completed before SMTP can be enabled |
| [Amazon Lightsail](https://aws.amazon.com/lightsail/) | Limited; request removal of sending restrictions | AWS Support for a static IP | Global | Reverse DNS and the sending-limit request are linked workflows |
| [Contabo](https://contabo.com/) | Policy varies by product and account | Customer control panel | Multiple regions | Verify limits and IP reputation for the allocated region |
| [Hostinger](https://www.hostinger.com/vps-hosting) | Product limits apply | Control panel | Multiple regions | Review hourly or daily message limits rather than assuming unrestricted SMTP |
| [UltaHost](https://ultahost.com/vps-hosting) | Confirm plan-specific limits | Control panel or support | Multiple regions | Obtain the sending allowance in writing for the selected plan |
| [OVHcloud](https://www.ovhcloud.com/en/vps/) | Anti-spam controls apply | Control panel | Global | Automated abuse controls can temporarily restrict suspicious traffic |

“Confirm” is intentional: an undocumented or historically open port should not
be represented as a contractual guarantee.


## Price and Plan Snapshot

The figures below are representative Linux plans checked on **29 July 2026**.
They are useful for creating a shortlist, not estimating the complete cost of a
mail service. Taxes, backups, snapshots, extra addresses, control panels, and
regional transfer charges may be additional.

| Provider and plan | Monthly price | Compute | Storage | Included network | Mail-specific catch |
| --- | ---: | --- | --- | --- | --- |
| [InterServer 1 slice](https://www.interserver.net/vps/) | $3 | 1 vCPU, 2 GB RAM | 40 GB SSD | 2 TB transfer | Port 25 request normally begins after providing the intended email usage and confirming the anti-abuse measures in place. |
| [IONOS VPS S+](https://www.ionos.com/servers/vps) | $5 standard price | 2 vCores, 2 GB RAM | 90 GB NVMe | Unlimited traffic | Advertised introductory price requires a term; SMTP unblock is separate |
| [UpCloud Starter](https://upcloud.com/global/pricing/) | $7 | 1 core, 2 GB RAM | 20 GB SSD | 250 Mbit/s, zero-cost egress under fair-use terms | Port 25 requires review and verification |
| [Scaleway DEV1-S](https://www.scaleway.com/en/pricing/virtual-instances/) | About €6.55 | 2 vCPUs, 2 GB RAM | Storage priced separately | 200 Mbit/s; egress and IPv6 included | Public IPv4 is extra and SMTP requires completed KYC |
| [Hetzner CX23](https://docs.hetzner.com/general/infrastructure-and-availability/price-adjustment/) | From €5.49 after the June 2026 adjustment | Shared vCPU plan; confirm regional specification | Confirm selected region | Large included transfer varies by region | New accounts cannot assume immediate port 25 approval |
| [Amazon Lightsail Micro](https://docs.aws.amazon.com/lightsail/latest/userguide/amazon-lightsail-bundles.html) | $7 with public IPv4 | 2 vCPUs, 1 GB RAM | 40 GB SSD | 2 TB in most listed regions | Static IP, reverse DNS, and removal of sending limits require planning |

### Reading the Price Correctly

- **Promotional price is not normal price.** Compare the renewal amount and
  required contract length.
- **A public IPv4 address may be extra.** A directly delivering mail server
  usually needs one, so IPv6-only headline pricing can be misleading.
- **Storage is not backup.** Budget for an independent backup and periodic
  restore tests.
- **Approval risk has a cost.** Do not migrate mailboxes until SMTP access and
  PTR control have been confirmed for the actual account.
- **Two gigabytes is a practical floor.** A very small personal relay may run
  with less, but spam filtering, antivirus scanning, and webmail can quickly
  exhaust 1 GB of RAM.


## Recommendations by Use Case

These recommendations describe which providers are worth investigating first;
they do not promise SMTP approval or inbox placement.

### Small Personal Server

Consider **InterServer** when a US location is suitable and the account-age requirement is acceptable.. Its slice includes comparatively generous memory and storage at the listed entry price. **IONOS** is the
alternative when a longer-term plan and phone-assisted SMTP approval are
acceptable.

### European Deployment

Consider **Scaleway** when KYC and console-controlled SMTP enablement fit the
project. Consider **Hetzner** when network footprint and resource pricing are
the priority, but establish account history and obtain SMTP approval before
migration. **UpCloud** is a useful third option when predictable pricing,
included egress, and API-managed PTR records matter.

### Relay-Based Mail

Choose **Amazon Lightsail**, **UpCloud**, or **Vultr** based on region and
operational tooling, then submit outbound mail through an authenticated relay
on port 465 or 587. This avoids making deployment dependent on port 25 approval,
although inbound SMTP and DNS still need correct configuration.

### Business-Critical Mail

Do not choose solely from the entry-price table. Prefer a provider that gives
written confirmation of the sending policy, a dedicated and replaceable IP,
responsive abuse handling, snapshots, external backups, and a tested recovery
path. Use two independent systems if an outage would stop business operations.

### Not Recommended

Avoid any plan whose seller will not confirm PTR control, outbound SMTP policy,
and acceptable message types. Also avoid annual promotional plans until the
assigned IP reputation and provider support have been tested.


## Short Notes on the Approval-Based Options

### InterServer

InterServer is included as one candidate, not highlighted as a preferred host.
Its published policy says outbound port 25 is blocked by default on VPS
instances. A request may be made after the server has been active for one month
without outstanding invoices. The review expects a valid hostname, matching
reverse DNS, and an explanation of the controls used to prevent abuse.
Dedicated mailing-only systems are not permitted under that policy.

Useful reading:

- [SMTP support on InterServer VPS instances](https://www.interserver.net/tips/kb/smtp-support-on-interserver-vps-instances/)
- [InterServer reverse DNS policy](https://www.interserver.net/tips/kb/reverse-dns-policies-at-interserver/)
- [InterServer VPS locations](https://www.interserver.net/vps/locations.html)

### IONOS

IONOS blocks outbound port 25 by default but documents a support-assisted
unblocking process. Before contacting support, the server should have a valid
FQDN, a matching PTR record, and an SPF record. Public IP reverse DNS can be
managed in the Cloud Panel.

Useful reading:

- [IONOS port 25 requirements](https://www.ionos.com/help/server-cloud-infrastructure/firewall-policies/unblocking-port-25-for-sending-emails/)
- [IONOS public IP and reverse DNS](https://www.ionos.com/help/server-cloud-infrastructure/ip-addresses-vps/creating-a-public-ip-address-vps-and-migrated-cloud-servers/)

### UpCloud

UpCloud blocks outbound port 25 for new accounts. Support can review a request
after the customer provides a use case and completes identity or payment
verification. PTR records for public addresses can be changed in the control
panel or through the API.

Useful reading:

- [UpCloud SMTP policy and opening port 25](https://upcloud.com/docs/guides/sending-email-smtp-best-practices/)
- [UpCloud DNS and PTR records](https://upcloud.com/docs/products/networking/dns/)

### Scaleway

Scaleway is an additional provider not present in the original shortlist.
Remote SMTP ports are disabled initially. Once account identity verification
is complete, an authorized user can enable SMTP for Instances through the
security-group setting. Scaleway also documents reverse DNS for Instance IPs.

Useful reading:

- [Sending email from a Scaleway Instance](https://www.scaleway.com/en/docs/instances/how-to/send-emails-from-your-instance/)
- [Scaleway reverse DNS concepts](https://www.scaleway.com/en/docs/instances/concepts/#reverse-dns)

### Amazon Lightsail

Lightsail is another added option. AWS limits outbound messages over port 25 by
default. Direct sending requires a static IP, forward DNS that points to that
address, and an AWS Support request covering both the sending restriction and
reverse DNS.

Useful reading:

- [Lightsail reverse DNS and email sending limits](https://docs.aws.amazon.com/lightsail/latest/userguide/amazon-lightsail-configuring-reverse-dns.html)
- [Lightsail firewall reference](https://docs.aws.amazon.com/lightsail/latest/userguide/amazon-lightsail-firewall-rules-reference.html)


## Preflight Test

Run this review before installing a mail stack:

- Ask whether outbound TCP 25 is open, blocked, or approval-based.
- Confirm that both IPv4 and IPv6 PTR records can be changed.
- Make the `A` or `AAAA` record and PTR record resolve back to each other.
- Check the assigned IP against major reputation and block-list services.
- Verify whether the address is dedicated, replaceable, or recycled.
- Read the acceptable-use policy for message type and volume restrictions.
- Decide whether a relay on ports 465 or 587 is an acceptable fallback.
- Budget for monitoring, backups, abuse handling, and security updates.


## DNS Baseline

A minimal production configuration normally includes:

```text
mail.example.net.  A      192.0.2.10
example.net.       MX 10  mail.example.net.
example.net.       TXT    "v=spf1 ip4:192.0.2.10 -all"
```

The provider must separately configure the PTR for `192.0.2.10` so it returns
`mail.example.net`. Add DKIM and DMARC before sending real traffic. IPv6 should
not be advertised until its forward DNS, PTR, routing, firewall, and reputation
have also been checked.


## How Entries Are Maintained

A provider belongs in this guide when its mail-related network controls can be
described without guessing. Pull requests should link to first-party pricing,
SMTP, reverse-DNS, and acceptable-use documentation. Promotional claims,
affiliate links, and unsourced statements such as “unlimited email” should not
be added.

Changes to SMTP access are more important than small price changes. When a
policy cannot be verified, mark it as “confirm with provider” and include the
date and region tested in the pull request.

## Contributions Welcome

Pull requests are welcome. If you notice outdated information, missing providers, or incorrect details, please submit an update with links to official sources.

￼
￼
￼
