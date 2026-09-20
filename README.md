# Simple MCP

**Local civic data by radius — with the contact details to actually reach
someone.**

Every events API can tell an assistant what is on. Ask one *"which nonprofits
near me take volunteers, and who do I email"* and it has a host's display
name and nothing else.

Simple answers that question. `find_organizations` returns the
organization's own published email, phone, and website alongside its
location, so an assistant can say **here is who to contact** instead of
**here is a name to search for**.

Free. No API key, no account, no sign-up.

```
https://www.simple-hub.com/api/mcp
```

---

## Install

**Claude Code**

```bash
claude mcp add --transport http simple https://www.simple-hub.com/api/mcp
```

**Claude Desktop** — Settings → Connectors → Add custom connector, and paste
the URL above.

**Anything else that speaks MCP over HTTP** — copy [`.mcp.json`](.mcp.json)
into your project, or point your client at the URL. Gemini CLI users can
install this repo directly as an extension
([`gemini-extension.json`](gemini-extension.json)).

---

## Four questions to try

> **"Which nonprofits near Canoga Park take volunteers, and how do I contact
> them?"**
> Names, distances, and the email address for each.

> **"What's happening in 91367 this weekend?"**
> Community events, classes, and markets within a radius you choose — with
> the venue, the coordinates, the price, and how to sign up.

> **"Where can I volunteer this Saturday in the Valley?"**
> Volunteer shifts, filtered to 501(c)(3) organizations if you want.

> **"Tell me everything about these three events."**
> One call, up to 25 ids, mixing events and organizations freely.

---

## What it covers

Two layers, and they are **not** the same size:

| | Coverage |
|---|---|
| **Organizations** — nonprofits, businesses, libraries, community groups | California and Florida today, expanding |
| **Events** — activities, classes, markets, volunteer shifts, offers | Los Angeles metro |

Both numbers are **measured from live rows, never asserted**. Call
`list_areas` with a location and it will tell you exactly what is there right
now — including, plainly, when the answer is *nothing yet*.

That last part is the design. An assistant told "we have national coverage"
and then handed an empty list will report that a town has nothing going on.
That is a false statement about somebody's neighbourhood, and it is worse
than admitting a gap. So every search response carries a `coverage` object
saying which kind of empty you got.

---

## Tools

| Tool | For |
|---|---|
| `search_local` | Things *happening* near a place, across all of Simple in one call |
| `find_organizations` | Organizations rather than occasions — who they are and how to reach them |
| `get_details` | Full records for up to 25 ids at once, events and organizations mixed |
| `list_areas` | What Simple actually covers near a location |

A few behaviours worth knowing before you build on it — a weekly club is
**one** result with its dates attached rather than thirty; times are venue
local with no UTC offset, deliberately; some listings are always-on and have
open days instead of a date. [`skills/simple/SKILL.md`](skills/simple/SKILL.md)
is the full briefing, and is loaded automatically if you install this repo as
a plugin or extension.

---

## Limits

- **Read only.** No tool submits an event, an RSVP, or a registration. Every
  result carries a `simple_url` for a person to finish the job themselves.
- **60 calls an hour per IP.** Refusals name the time the window resets.
- **Public profiles only.** Nothing here reaches a user account, a private
  profile, or a personal email address. A business's published contact address
  is not the same field as its owner's.
- **Sources are cleared individually.** Content Simple cannot redistribute is
  excluded from every response, so some things visible on the website do not
  appear here.

---

## Data

The [MIT license](LICENSE) covers this repository — the manifests, the skill,
the docs. It does not grant rights to the data the server returns, which
belongs to the organizations that published it. Use it to answer someone's
question and link them onward, not to bulk-collect or rebuild the directory.

Something wrong or missing? Listings on
[simple-hub.com](https://www.simple-hub.com) can be corrected or claimed by
the organization they belong to.

Built by [Simple](https://www.simple-hub.com).
