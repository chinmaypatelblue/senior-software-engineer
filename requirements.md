# Requirements

## Objective

Build a small offline parser for the provided packet capture and extract top-of-book quote updates into a usable output format.

## Inputs Provided

- source documentation in `resources/`
- one session capture in `sessions/2026-01-21/pcaps/`

## Required Deliverable

Provide a solution that:

- reads the provided PCAP file
- identifies and decodes the quote update messages needed for top-of-book output
- extracts a stable instrument identifier and quote data correctly
- writes parsed top-of-book updates to a simple output format such as JSONL or CSV
- includes a short note describing approach, assumptions, and validation method

## Submission

Upload a private 5-8 minute demo video to YouTube, Dailymotion, or the video hosting service of your choice, and send us the link.

## Functional Requirements

- preserve message ordering from the capture
- focus on quote update messages needed for top-of-book output
- support filtering by available instrument key, such as symbol or `security_id`
- support filtering by optional event-time range using `start_time` and `end_time`
- handle malformed or truncated data defensively
- avoid loading the entire PCAP into memory if practical
- if any information is missing, you may make reasonable assumptions and document them


## Non-Goals

- live networking
- multicast handling
- production deployment concerns
- support for unrelated feed types


## Evaluation Notes

The goal is correctness, clarity, and judgment.

Trend analysis is out of scope, since prices may be randomized during sanitization.
Focus on correct parsing and extraction rather than interpreting market behavior.

A correct solution may key output by symbol or by `security_id`, depending on what is available in the provided data.

If a human-readable symbol is unavailable in the provided capture, using `security_id` as the instrument key is acceptable.

## Sample Commands And Output

The exact CLI is up to you, but a solution should support a simple offline parse flow.
The sample commands below are illustrative only; you do not need to implement this exact CLI.

Input Example 1:

```bash
python your_solution.py \
  --input sessions/2026-01-21/pcaps/20260121_MEMOIR_TopA.sanitized.pcap \
  --output outputs/quotes.jsonl
```

Input Example 2:

```bash
python your_solution.py \
  --input sessions/2026-01-21/pcaps/20260121_MEMOIR_TopA.sanitized.pcap \
  --symbol 6718 \
  --output outputs/quotes.6718.jsonl
```

Example output:

```json
{"instrument_id":"6718","bid_px":5.19,"bid_sz":117,"ask_px":130.4,"ask_sz":3302,"event_ts_ns":1768976090537034820}
{"instrument_id":"6718","bid_px":4.73,"bid_sz":150,"ask_px":130.4,"ask_sz":3302,"event_ts_ns":1768976130374385701}
{"instrument_id":"6718","bid_px":4.73,"bid_sz":150,"ask_px":134.97,"ask_sz":3193,"event_ts_ns":1768976133417931617}
```
