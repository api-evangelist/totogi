# Totogi (totogi)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
