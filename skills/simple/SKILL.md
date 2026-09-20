---
name: simple
description: Use when someone asks what is happening near a place, where they can volunteer, or which nonprofits, libraries, or community organizations are nearby and how to contact them. Covers community events, classes, volunteer shifts, markets, and local business offers, searchable by radius.
---

# Simple — local civic data by radius

Four tools. Use them in this order when you do not already know the answer.

## The one thing that will trip you up

**Coverage is not uniform, and an empty list does not mean an empty town.**

Simple's organization directory and its event listings grow at different
rates and in different places. A search that comes back with nothing means
one of two completely different things:

- *We cover this area and nothing matched* — "there are no volunteer shifts
  in Woodland Hills this Saturday" is a true, complete answer. Say it.
- *We do not reach this area* — you have learned nothing about the town. Say
  **that**, and point the person elsewhere.

Every search response carries a `coverage` object telling you which one it
is. **Read it before you write your answer.** Reporting the second case as
the first tells someone their neighbourhood has nothing going on when really
you just could not see it.

When in doubt about a town, call `list_areas` with the location first. It is
cheap and it answers exactly this.

## The tools

### `list_areas`
Coverage, in two layers, for a place. Organizations and events are reported
separately because they differ. Call it first for an unfamiliar town, or when
a search returns nothing and you want to know why.

### `search_local`
Things *happening* near a place — events, classes, markets, volunteer shifts,
local offers — across all of Simple in one call. Takes a location, an
optional radius (default 10 miles, max 50), a freeform query, a date range in
plain language ("this weekend", "next 2 weeks"), and platform filters.

### `find_organizations`
Organizations rather than occasions: "which nonprofits are near me", "who do
I email". **This is the tool nothing else has.** Results carry the
organization's own published email, phone, and website — so you can answer
"here is who to contact" rather than "here is a name to search for".

Use it, not `search_local`, when the question is about *who* rather than
*when*.

### `get_details`
Full records for ids you already hold, up to 25 in one call. Takes listing
ids and organization ids mixed together — no need to sort them. Returns what
search leaves out: the whole description, every upcoming date rather than the
ones inside your search window, spots left, and an organization's hours and
current programme.

Ids that come back in `missing` were cancelled, unpublished, or cannot be
redistributed. Do not present the rest as the whole answer without saying so.

## Things worth knowing

**A repeating thing is one result, not thirty.** A weekly run club comes back
once with `other_dates` and `dates_in_range` attached. Do not list the same
activity once per occurrence.

**Times have no UTC offset.** `timezone` is `null` on purpose. The time given
is the venue's local clock time, which is the answer to "when is it". Do not
convert it; do not assume it is UTC.

**Some things are always on.** `always_on: true` means a market or a standing
offer with no single date — read `open_days` (0 = Sunday), `opens_at`, and
`closes_at` instead of looking for a date that is not there.

**Free is stated, not inferred.** `participate.price` says `"Free"` outright
when it is. `where_to_sign_up` tells you whether people sign up on Simple, on
someone else's site (`signup_url`), or by simply turning up.

**Link to the listing.** Every result carries a `simple_url`. Use it — it is
where someone completes the thing you just told them about.

**Read only.** Nothing here submits an event, an RSVP, or a registration.
Point people at `simple_url` to do that themselves.

**Anonymous, 60 calls an hour.** No key, no account. If you hit the limit the
error names the time it resets; wait for it rather than retrying.
