---
name: create-danzeira-event
description: Set up a complete Danzeira event through the Danzeira MCP, from draft to published. Use when an organiser wants to create a dance social, class, workshop, festival or recurring series, add tickets, promo codes or a schedule, and publish it.
---

# Create a Danzeira event

Walk the organiser through building a full event with the Danzeira MCP tools. Everything is created as a draft first. Nothing is public until `events_publish` runs, and that only happens after the organiser confirms.

## When to use

- "Create an event / social / workshop / festival on Danzeira"
- "Set up tickets for my class" or "make this a weekly series"
- Any request that ends with a new event being listed on Danzeira

## Before you start: collect the details

Ask for anything missing. Do not guess prices, capacities, dates, times or the venue.

- Title and description
- Venue name and address
- Start and end date and time, in the venue's local time
- Ticket types: name, price (0 for a free RSVP), and capacity for each
- Optional: promo codes, a schedule of sessions, whether it repeats

## Step 1: Create the draft event with `events_create`

Required: `title`, `start_time`, `end_time`, `location`, `latitude`, `longitude`.

- `start_time` and `end_time` are venue-local, format `YYYY-MM-DDTHH:MM` (for example `2026-11-20T19:30`). The time zone comes from the venue location. There is no time zone parameter, so never convert times into your own or the user's zone. If the organiser gives times in another zone, ask which local time they mean.
- `location` is the full street address. `venue_name` is the display name of the venue. Also pass `city` and `region` when known, and `place_id` if you have a Google place id.
- `latitude` and `longitude` must match the address. If you cannot resolve them reliably, ask the organiser rather than inventing coordinates.
- `description`: plain text with plain newlines (`\n`) between paragraphs and list lines. Do not use HTML or escaped `\\n` sequences.
- Cover image: pass `cover_image_url`, an https URL to a png, jpeg or webp file up to 5MB. The current tool schema does not accept base64 image data, so if the organiser only has a local file, ask them for a hosted https link, or tell them to upload the cover on danzeira.com after the draft exists.
- Optional fields: `slug` (custom link), `sales_end_at`, `purchase_message` (shown after purchase), `dance_style_ids`, `public_attendee_list_enabled`, `ticket_transfers_enabled`, `attendee_cancellations_enabled`, `save_venue`, `venue_id` (reuse a saved venue).
- Pass an `idempotency_key` (any unique string) so a retry returns the first event instead of creating a duplicate.

Keep the returned event `slug`. Every later call uses it.

## Step 2: Add ticket types with `ticket_types_create`

Required: `slug`, `name`, `price`.

- Free RSVP: `price: 0`.
- Paid: `price` is in the event's own currency, as the organiser stated it. Do not convert currencies. If the currency is unclear, check `events_show` or ask.
- `capacity`: the number the organiser gave. If they gave none, ask. Do not make one up.
- Optional: `description`, `sales_start_at`, `sales_end_at`, `online_sales_paused`, and lead/follow balancing with `role_ratio_type` and `role_ratio_value`.
- Repeat for each ticket type (for example Early bird, Full pass, Door).

## Step 3 (optional): Promo codes with `promo_codes_create`

Required: `slug`, `code`, `discount_type` (`percent` or `fixed`), `discount_value`.
Optional: `ticket_type_id` (limit to one ticket type, id from `events_show`), `max_redemptions`, `expires_at`, `active`.

## Step 4 (optional): Schedule with `sessions_create`

Required: `slug`, `title`, `starts_at`, `ends_at`. Optional: `description`, `apply_to_series`.

- Sessions must sit inside the event start and end. Times without a zone are read in the venue time zone.
- For a series, set `apply_to_series: true` to copy the session onto later dates.
- The schedule may be hidden by default. Use `sessions_visibility` with `schedule_public: true` to show it, after the organiser agrees.

## Step 5 (optional): Recurring series

1. Turn the event into a series with `dates_update`: `slug`, `frequency` (`weekly`, `fortnightly`, `monthly_by_weekday` or `monthly_by_day`), plus `weekdays`, `monthly_week_ordinal`, and an end with `ends_type` (`count` with `occurrence_limit`, or `date` with `repeat_until`).
2. Extend an existing series later with `dates_add` (`slug`, optional `count`, default 4).
3. Check the result with `dates_show`.

Set up tickets and sessions before adding dates where possible, so new dates inherit them.

## Step 6: Review, confirm, then publish

1. Call `events_show` and summarise for the organiser: title, venue and address, local start and end with the time zone, each ticket type with price, currency and capacity, promo codes, schedule, and series dates.
2. Ask explicitly: "Publish this now?" Wait for a clear yes.
3. Publish with `events_publish` (one event) or `events_publish_series` (every draft date in a series).
4. Share the public event link.

If anything looks wrong, fix it with `events_update`, `ticket_types_update`, `promo_codes_update` or `sessions_update` before publishing.
