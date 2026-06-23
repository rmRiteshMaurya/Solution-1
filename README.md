# Stale Lead Processing Engine

Nightly Salesforce batch job that scores inactive Leads, auto-converts qualifying ones, disqualifies the rest, and emails a summary report.

## Problem

50,000+ leads inactive for 90+ days, never converted. Need a nightly job to score, convert/disqualify, and report results.

## Lead Selection

- Not converted
- Created 90+ days ago
- No activity (`LastActivityDate = null`)

## Scoring

| Rule | Points |
| Corporate email domain | +10 |
| Industry in Custom Metadata | +20 |
| Company populated | +15 |

**Score >= 30** -> Convert Lead
**Score < 30** -> Status = "Disqualified"

## Components

StaleLeadProcessingBatch` - Batchable + Stateful, configurable batch size (default 200)
StaleLeadProcessingScheduler` - Schedulable, runs the batch
StaleLeadProcessingBatchTest` - test class

## Schedule (1 AM daily)

System.schedule('Stale Lead Job', '0 0 1 * * ?', new StaleLeadProcessingScheduler());
