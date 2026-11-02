# Search protocol

This folder documents the database searches behind *Proactive, Robust Security at Scale: A Systematization of Intent-Driven Security Policy Automation*: the complete search strings, the fields and filters as executed, and the record counts per database.

| File | Content |
|---|---|
| `README.md` | Protocol, exact search strings, filters, and count tables |
| `queries.csv` | One row per executed search, with the decoded string, its filters, record count, and original URL |

Counts in this folder are the numbers shown by each database at search time. They are not deduplicated.

## Query families

Each database was searched with three query families. The term sets were identical in both collection rounds.

| Family | First concept group | Second concept group |
|---|---|---|
| Intent-oriented | intent-based network\*, intent-driven, intent extraction, intent recognition, intent expression, intent capture, intent translation, intent verification, intent refinement, intent orchestration | automation, autonomic, self-configur\*, network management, network control, configuration, system, network |
| Policy-automation | policy automation, policy synthesis, policy translation, policy refinement, policy enforcement, policy verification, policy management, policy conflict resolution, policy orchestration | Same as above |
| Intent x Policy (bridging) | intent-driven polic\*, intent-based polic\*, intent-based automation, intent-driven automation, intent-based configuration, intent-driven network automation | None |

Within a group, terms are joined with OR. The two groups are joined with AND.

## Collection rounds

| Round | Date | Publication years | Purpose |
|---|---|---|---|
| 1 | 31 October 2025 | 2020 to 2025 | Initial collection |
| 2 | 13 August 2026 **[TO CONFIRM: the paper states "through August 15, 2026"]** | 2020 to 2026 | Update covering late 2025 and 2026 |

## Search strings

### IEEE Xplore

Field: All Metadata. IEEE describes metadata as including "the abstract, index terms, and bibliographic citation data" ([Command Search help](https://ieeexplore.ieee.org/Xplorehelp/searching-ieee-xplore/command-search)).

```text
(("All Metadata":"intent-based network*" OR "All Metadata":"intent-driven" OR "All Metadata":"intent extraction" OR "All Metadata":"intent recognition" OR "All Metadata":"intent expression" OR "All Metadata":"intent capture" OR "All Metadata":"intent translation" OR "All Metadata":"intent verification" OR "All Metadata":"intent refinement" OR "All Metadata":"intent orchestration") AND ("All Metadata":"automation" OR "All Metadata":"autonomic" OR "All Metadata":"self-configur*" OR "All Metadata":"network management" OR "All Metadata":"network control" OR "All Metadata":"configuration" OR "All Metadata":"system" OR "All Metadata":"network"))
```

```text
(("All Metadata":"policy automation" OR "All Metadata":"policy synthesis" OR "All Metadata":"policy translation" OR "All Metadata":"policy refinement" OR "All Metadata":"policy enforcement" OR "All Metadata":"policy verification" OR "All Metadata":"policy management" OR "All Metadata":"policy conflict resolution" OR "All Metadata":"policy orchestration") AND ("All Metadata":"automation" OR "All Metadata":"autonomic" OR "All Metadata":"self-configur*" OR "All Metadata":"network management" OR "All Metadata":"network control" OR "All Metadata":"configuration" OR "All Metadata":"system" OR "All Metadata":"network"))
```

```text
(("All Metadata":"intent-driven polic*" OR "All Metadata":"intent-based polic*" OR "All Metadata":"intent-based automation" OR "All Metadata":"intent-driven automation" OR "All Metadata":"intent-based configuration" OR "All Metadata":"intent-driven network automation"))
```

### ACM Digital Library

Fields: Title, Abstract, Keyword.

```text
(Title:("intent-based network*" OR "intent-driven" OR "intent extraction" OR "intent recognition" OR "intent expression" OR "intent capture" OR "intent translation" OR "intent verification" OR "intent refinement" OR "intent orchestration") OR Abstract:("intent-based network*" OR "intent-driven" OR "intent extraction" OR "intent recognition" OR "intent expression" OR "intent capture" OR "intent translation" OR "intent verification" OR "intent refinement" OR "intent orchestration") OR Keyword:("intent-based network*" OR "intent-driven" OR "intent extraction" OR "intent recognition" OR "intent expression" OR "intent capture" OR "intent translation" OR "intent verification" OR "intent refinement" OR "intent orchestration")) AND (Title:("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network") OR Abstract:("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network") OR Keyword:("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network"))
```

```text
(Title:("policy automation" OR "policy synthesis" OR "policy translation" OR "policy refinement" OR "policy enforcement" OR "policy verification" OR "policy management" OR "policy conflict resolution" OR "policy orchestration") OR Abstract:("policy automation" OR "policy synthesis" OR "policy translation" OR "policy refinement" OR "policy enforcement" OR "policy verification" OR "policy management" OR "policy conflict resolution" OR "policy orchestration") OR Keyword:("policy automation" OR "policy synthesis" OR "policy translation" OR "policy refinement" OR "policy enforcement" OR "policy verification" OR "policy management" OR "policy conflict resolution" OR "policy orchestration")) AND (Title:("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network") OR Abstract:("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network") OR Keyword:("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network"))
```

```text
Title:("intent-driven polic*" OR "intent-based polic*" OR "intent-based automation" OR "intent-driven automation" OR "intent-based configuration" OR "intent-driven network automation") OR Keyword:("intent-driven polic*" OR "intent-based polic*" OR "intent-based automation" OR "intent-driven automation" OR "intent-based configuration" OR "intent-driven network automation") OR Abstract:("intent-driven polic*" OR "intent-based polic*" OR "intent-based automation" OR "intent-driven automation" OR "intent-based configuration" OR "intent-driven network automation")
```

### SpringerLink

The advanced-search URL carries the same string in both its `query` and `title` parameters, so these searches are title-scoped. They do not cover abstracts or keywords.

```text
(("intent-based network*" OR "intent-driven" OR "intent extraction" OR "intent recognition" OR "intent expression" OR "intent capture" OR "intent translation" OR "intent verification" OR "intent refinement" OR "intent orchestration") AND ("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network"))
```

```text
(("policy automation" OR "policy synthesis" OR "policy translation" OR "policy refinement" OR "policy enforcement" OR "policy verification" OR "policy management" OR "policy conflict resolution" OR "policy orchestration") AND ("automation" OR "autonomic" OR "self-configur*" OR "network management" OR "network control" OR "configuration" OR "system" OR "network"))
```

```text
("intent-driven polic*" OR "intent-based polic*" OR "intent-based automation" OR "intent-driven automation" OR "intent-based configuration" OR "intent-driven network automation")
```

### ScienceDirect

Field: Title, abstract or author-specified keywords (the `tak` URL parameter). Unlike the other databases, the intent-oriented and policy-automation families were run one first-group term at a time **[TO CONFIRM: reason, presumably the limit on Boolean connectors in the search form]**. Each string has the form:

```text
("<term>") AND ("automation" OR "autonomic" OR "self-configure" OR "network management" OR "network control" OR "configuration" OR "system" OR "network")
```

The ScienceDirect strings use no wildcards. They use `intent-based network`, `self-configure`, `intent-driven policy`, and `intent-based policy` where the other databases use `intent-based network*`, `self-configur*`, `intent-driven polic*`, and `intent-based polic*`. ScienceDirect phrase searches include plurals and spelling variants ([Advanced search help](https://www.elsevier.support/sciencedirect/answer/how-do-i-use-the-advanced-search)).

Bridging family, run as one string:

```text
"intent-driven policy" OR "intent-based policy" OR "intent-based automation" OR "intent-driven automation" OR "intent-based configuration" OR "intent-driven network automation"
```

## Fields and filters as executed

| Database | Fields | Year filter in URL | Document-type filter in URL | Language filter |
|---|---|---|---|---|
| IEEE Xplore | All Metadata | 2020-2025; 2020-2026 | Conferences, Journals | Not available |
| ACM Digital Library | Title, Abstract, Keyword | 2020-2025; 2020-2026 | None; document types were to be selected manually | Not applied |
| SpringerLink | Title-scoped (see above) | 2020-2025; 2026 in Round 2 | None in URL | None in URL |
| ScienceDirect | Title, abstract, keywords | Round 1: 2020-2025. Round 2: varies (see below) | Round 1: none in URL. Round 2: varies (see below) | None in URL |

**[TO CONFIRM: whether the SpringerLink and ScienceDirect counts were read before or after any filters set in the interface, and at which step non-English records and non-journal, non-conference records were removed.]**

## Record counts

### Round 1 (31 October 2025, 2020 to 2025)

| Database | Intent-oriented | Policy-automation | Bridging | Total |
|---|---:|---:|---:|---:|
| IEEE Xplore | 679 | 533 | 22 | 1,234 |
| ACM Digital Library | 55 | 59 | 1 | 115 |
| SpringerLink | 11 | 7 | 2 | 20 |
| ScienceDirect | 99 | 184 | 2 | 285 |
| **Total** | **844** | **783** | **27** | **1,654** |

### Round 2 (cumulative, 2020 to 2026)

| Database | Intent-oriented | Policy-automation | Bridging | Total |
|---|---:|---:|---:|---:|
| IEEE Xplore | 1,202 | 1,151 | 44 | 2,397 |
| ACM Digital Library | 90 | 83 | 2 | 175 |
| SpringerLink | 12 | 7 | 2 | 21 |
| ScienceDirect | 161 | 287 | 2 | 450 |
| **Total** | **1,465** | **1,528** | **50** | **3,043** |

Round 2 was recorded differently per database. IEEE Xplore, ACM, and the ScienceDirect intent-oriented and policy-automation families are cumulative counts. SpringerLink was recorded as new records only (1 in 2026, intent-oriented), and the ScienceDirect bridging family as no new records, so the table adds those figures to Round 1.

**[TO CONFIRM: the paper reports 2,928 identified records. The Round 2 table sums to 3,043. State how 2,928 was derived, for example the record count after merging and removing duplicates in Zotero.]**

### ScienceDirect, per term

| Family | Term | Round 1 records | Round 1 year filter | Round 2 records | Round 2 year filter | Round 2 article types |
|---|---|---:|---|---:|---|---|
| Intent-oriented | intent-based network | 29 | 2020-2025 | 37 | 2020-2026 | REV, FLA, SSU |
| Intent-oriented | intent-driven | 15 | 2020-2025 | 33 | 2020-2027 | REV, FLA, SSU |
| Intent-oriented | intent extraction | 5 | 2020-2025 | 8 | none in URL | none in URL |
| Intent-oriented | intent recognition | 42 | 2020-2025 | 68 | 2020-2026 | REV, FLA |
| Intent-oriented | intent expression | 1 | 2020-2025 | 2 | 2020-2026 | none in URL |
| Intent-oriented | intent capture | 0 | 2020-2025 | 1 | 2020-2026 | none in URL |
| Intent-oriented | intent translation | 5 | 2020-2025 | 9 | none in URL | FLA, REV |
| Intent-oriented | intent verification | 1 | 2020-2025 | 2 | none in URL | none in URL |
| Intent-oriented | intent refinement | 1 | 2020-2025 | 1 | none in URL | none in URL |
| Intent-oriented | intent orchestration | 0 | 2020-2025 | not recorded | n/a | n/a |
| Policy-automation | policy automation | 5 | 2020-2025 | 7 | 2020-2026 | none in URL |
| Policy-automation | policy synthesis | 12 | 2020-2025 | 14 | 2020-2026 | REV, FLA |
| Policy-automation | policy translation | 12 | 2020-2025 | 28 | 2020-2026 | REV, FLA |
| Policy-automation | policy refinement | 14 | 2020-2025 | 30 | 2020-2026 | REV, FLA, SSU |
| Policy-automation | policy enforcement | 83 | 2020-2025 | 135 | 2020-2026 | REV, FLA |
| Policy-automation | policy verification | 6 | 2020-2025 | 11 | 2020-2026 | none in URL |
| Policy-automation | policy management | 50 | 2020-2025 | 58 | 2020-2027 | REV, FLA |
| Policy-automation | policy conflict resolution | 0 | 2020-2025 | 1 | 2020-2026 | none in URL |
| Policy-automation | policy orchestration | 2 | 2020-2025 | 3 | none in URL | none in URL |

Sub-query counts can overlap, so the ScienceDirect family totals may count a record more than once. Round 2 URLs recorded for ScienceDirect do not carry uniform filters: `queries.csv` gives each one. Rows with `none in URL` may include records outside 2020-2026 or outside the listed article types, and two URLs use the year range 2020-2027.

## From search results to screening

Results were exported as RIS and merged in Zotero, where duplicates were removed. Round 1 produced a list of 1,580 records. Round 2 results were compared against that list to isolate new records, which then went through the same screening procedure. **[TO CONFIRM: this follows the working notes; confirm that it matches what was executed, and add the final counts.]**

## Reproducing the searches

These databases change over time, so re-running a URL will not return the recorded counts. For example, an IEEE Xplore intent-oriented search limited to 2020 returned 45 records on 31 October 2025 and 47 on 13 August 2026. The counts document the state at collection time.
