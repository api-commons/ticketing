# Help Desk and Ticketing

The common help desk vocabulary -- tickets, the replies on them, the people who raise and
work them -- distilled from the APIs that already exist. Part of
[Schema Commons](https://github.com/api-commons/schema-commons), published by the
[API Commons](https://apicommons.org).

Built from the providers tagged Customer Service, Help Desk, ITSM, Customer Support,
Service Desk or IT Support in [APIs.io](https://apis.io) that publish their own OpenAPI.
Only harvested contracts count -- nothing API Evangelist generated. Every number below is
a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| ticket | 33 | 25 | status 92%, id 88%, subject 76%, created_at 72%, assignee_id 64%, priority 60%, channel 56%, updated_at 56%, requester_id 52% | tickets 15, conversations 13, requests 6, issues 2 |
| ticket_message | 18 | 16 | id 88%, body 81%, attachments 75%, ticket_id 56%, author_id 50%, created_at 50% | messages 8, notes 7, comments 4, reply 2 |
| requester | 24 | 20 | id 85%, name 80%, email 75%, phone 70%, created_at 60% | contacts 13, customers 5, customer 2, contact 2 |
| agent | 14 | 10 | id 70%, name 70% | agents 12, agent 3, operators 1 |
| group | 27 | 22 | name 91%, id 86% | teams 17, groups 13, queues 4, team 3 |
| organization | 14 | 14 | id 100%, name 79%, created_at 71% | accounts 5, organizations 5, companies 4, organization 4 |

Cohort: 77 providers with harvested OpenAPI, 48 crosswalks. Of the providers exposing a ticket: 88% create, 82% list, 85% fetch, 70% update, 48% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
  Replies live where the market puts them, under the ticket:
  `/tickets/{ticket_id}/messages`.
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from. Freshdesk, for example: the assignee is
  `responder_id`, the channel is `source`, the organization is `company_id`.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments,
  aliases, reviewed exclusions and merged duplicates. Everything else is counted.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- **The "Ticketing" tag is not used.** In APIs.io it mixes support tickets with event and
  transit ticketing, whose `/tickets` are a seat or a fare. The help desk, customer service
  and ITSM tags define the cohort.
- **A support conversation is a ticket; a bot transcript is not.** Shared-inbox products
  (Intercom, Front, Gladly, Chatwoot, Kustomer, Hiver, Dixa) model the ticket as a
  conversation with a status and an assignee, and are counted as tickets. AI-assistant
  transcripts and conversation-analytics exports record a bot's session, not a request
  someone is working, and are excluded. Each exclusion and its reason is in
  [concepts.yml](concepts.yml).
- **Replies are `messages`.** Providers name them `messages` (8), `notes` (7), `comments`
  (4) and `replies` (2); the concept is `ticket_message` and the path
  `/tickets/{ticket_id}/messages`. Public replies and internal notes share one resource,
  told apart by `is_public`, as most providers do.
- **Replies only count under a ticket.** `comments`, `messages`, `notes` and `replies` are
  matched only beneath a ticket, case, incident, request or conversation path, so a forum's
  `/posts` or a CMS's `/notes` cannot pass for one.
- **`requester`, not `customer` or `contact`.** The person who raised the ticket is named
  for their role in this context; the crosswalk records each provider's own word
  (customers, contacts, end users, requesters).
- **SLA policies were measured and dropped.** Three providers expose them in their own
  OpenAPI. Revisit as the cohort grows.
- **One company counts once.** Lithium and Khoros are one specification under two
  listings, merged under `duplicates:`.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
