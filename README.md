# Totogi (totogi)

Totogi LLC is an Austin, Texas based vertical AI and BSS software vendor that builds telecom operator software natively on the public cloud (AWS). Its two products are the **Totogi Ontology** (formerly BSS Magic), a machine-readable semantic layer that sits above a carrier's existing BSS, OSS, core, and network systems so AI agents can reason and act across them, and **Totogi Charging-as-a-Service**, a multi-tenant, serverless 5G Standalone converged charging system with built-in policy control that integrates over the 4G/5G NSA Sy (PCRF) and 5G SA N28 (PCF) interfaces. Totogi sells to communications service providers, MNOs, MVNOs, and MVNEs — it does not own spectrum and it does not sell to developers directly.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/totogi/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/totogi/refs/heads/main/apis.yml)

## Tags

- Telecommunications
- United States
- BSS
- OSS
- Charging
- Messaging
- SMS
- A2P
- 5G
- TM Forum
- Standards
- Network Vendor
- Vertical AI

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

### Whoosh Programmable Messaging API

Whoosh! is Totogi's Application-to-Person (A2P) messaging API, launched in September 2023 and positioned as a drop-in replacement for Twilio's A2P APIs that network operators can resell to keep enterprise messaging traffic and revenue on their own network. The REST surface deliberately mirrors Twilio's `2010-04-01` account/resource shape, authenticates with HTTP Basic using an Account SID and Auth Token, and pushes delivery status back through a `StatusCallback` webhook. Quickstart documentation is public and un-gated; the console that issues credentials is not, and there is no self-serve signup.

- **Human URL:** [https://docs.whoosh.totogi.solutions](https://docs.whoosh.totogi.solutions)
- **Base URL:** `https://api.whoosh.totogi.solutions/2010-04-01/Accounts/`

#### Tags

- Messaging
- SMS
- A2P
- CPaaS
- Webhooks

#### Properties

- [Documentation](https://docs.whoosh.totogi.solutions)
- [API Reference](https://docs.whoosh.totogi.solutions/send_message)
- [Webhook / status callback docs](https://docs.whoosh.totogi.solutions/monitor_message_status)
- [Console](https://console.whoosh.totogi.solutions)
- [Node.js SDK — `whoosh-sms` on npm](https://www.npmjs.com/package/whoosh-sms)
- [Python SDK — `WhooshSms` on PyPI](https://pypi.org/project/WhooshSms/)
- [Ruby SDK — `whoosh-ruby` on RubyGems](https://rubygems.org/gems/whoosh-ruby)
- [Java SDK — GitHub releases](https://github.com/totogi/whoosh-java-releases)
- [Launch announcement](https://totogi.com/newsroom/press-releases/replacement-for-twilio-a2p-apis-for-network-operators/)

### Totogi Charging-as-a-Service

A serverless, multi-tenant 5G Standalone and 5G Advanced converged charging system delivered as SaaS on AWS, with built-in policy control and a single account management API across tenants. Totogi's BSS surface is built on TM Forum Open APIs, for which Totogi holds Platinum Conformance Certification across 31 certified Open APIs. The product's API reference is published on a Redocly-hosted portal that returns a "Log in with Redocly" wall to anonymous visitors — no specification, endpoint list, or base URL is publicly retrievable.

- **Human URL:** [https://totogi.com/ai-native-charging/](https://totogi.com/ai-native-charging/)

#### Tags

- Charging
- BSS
- 5G
- TM Forum
- Policy Control

#### Properties

- [Documentation](https://totogi.com/ai-native-charging/)
- [API Reference (login wall)](https://docs.totogi.solutions)
- [Product updates](https://totogi.com/ai-native-charging/caas-product-updates/)
- [TM Forum Open API Conformance](https://www.tmforum.org/conformance-certification/open-api-conformance/)
- [AWS Marketplace listing](https://aws.amazon.com/marketplace/pp/prodview-hnaq62hzpxane)

## API posture, honestly

Totogi's developer surface is split, and both halves are findings.

**The platform documentation is a login wall.** The first-party API reference at `https://docs.totogi.solutions` is a Redocly-hosted portal that returns HTTP 200 with a "Log in with Redocly" page to anonymous visitors. Every spec-shaped path probed on that host returns the same login HTML. `developer.`, `developers.`, `docs.`, `api.`, `console.`, `dev.`, and `sandbox.` under `totogi.com` do not resolve at all, and `totogi.com/developer`, `/api`, and `/docs` return 404. There is no self-serve signup and no downloadable OpenAPI anywhere.

**Whoosh is the one open surface.** The Whoosh docs are public, the API host is live, four helper libraries are published to npm, PyPI, RubyGems, and GitHub Releases, and the webhook contract is documented. But Whoosh is sold *through operators* to their enterprise customers — Totogi's own launch language is that the telco handles connectivity and sales while Totogi delivers the APIs. Developers reach it via a carrier, not via a signup form.

**No CAMARA.** There is zero mention of CAMARA, GSMA Open Gateway, or Aduna in Totogi's own canonical AI index (`llms.txt`, `llms-full.txt`, `facts.json`, `evidence.json`, `glossary.json`, `case-studies.json`) or anywhere on the site. This is not a press-release-only posture — there is no press release either. Totogi's standards alignment is TM Forum and 3GPP. `glossary.json` defines Network Exposure Function as a term, but Totogi makes no claim to operate or expose one.

**TM Forum is where the certification is.** Totogi announced Platinum Open API Conformance Certification and the #1 spot on TM Forum's Open API Certification Leaderboard in September 2021, reaching Platinum in four months and finishing the month with 31 certified Open APIs. The individual TMF numbers are not published by Totogi, and the TM Forum conformance directory returns HTTP 403 to anonymous fetch, so the itemized list could not be enumerated. Totogi also sells certification acceleration as an outcome: a published case study describes CloudSense certifying 13 TM Forum Open APIs in 30 days with the Totogi Ontology.

**Machine-readable about itself, gated about its APIs.** Totogi publishes `llms.txt`, `llms-full.txt`, and four canonical "AI Index" JSON documents linked from its footer, and `robots.txt` explicitly welcomes GPTBot, ClaudeBot, PerplexityBot, CCBot, and friends. A telecom vendor that machine-readably publishes its own facts and evidence while keeping its API reference behind a Redocly login is a precise illustration of the sector split.

## Links

- [Website](https://totogi.com)
- [About](https://totogi.com/company/)
- [FAQs](https://totogi.com/company/faqs/)
- [Blog](https://totogi.com/blog/)
- [Newsroom](https://totogi.com/newsroom/press-releases/)
- [Case studies](https://totogi.com/resources/case-studies/)
- [GitHub](https://github.com/totogi)
- [LinkedIn](https://www.linkedin.com/company/totogi/)
- [X](https://x.com/totogi)
- [YouTube](https://www.youtube.com/@Totogi)
