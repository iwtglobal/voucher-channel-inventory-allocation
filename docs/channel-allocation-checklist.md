# Channel Inventory Allocation Checklist

Practical checklist for allocating **EVD / gift-card voucher stock** across POS, USSD, app, and API without double-sell.

## 1. Ledger rules

| Rule | Required | Notes |
|------|----------|-------|
| Single serial ownership | Yes | Available → reserved → sold |
| Atomic sell | Yes | One commit wins under concurrency |
| Idempotency on sell | Yes | Channel + client key |
| Reservation TTL | Yes | Auto-release on expiry |
| Channel stamp on sale | Yes | For reporting and disputes |
| Rebalance audit | Yes | Who moved what, when |

## 2. Quota setup checklist

- [ ] Define quotas per channel and denomination
- [ ] Document overflow policy (borrow global / decline / sibling)
- [ ] Set partner API minimum buckets if contracted
- [ ] Publish stock-out alerts per channel
- [ ] Schedule rebalance windows (e.g. off-peak)

## 3. Cutover / go-live

- [ ] Drain old channel-local caches
- [ ] Load-test concurrent sells at low remaining counts
- [ ] Verify USSD abandon releases reservations
- [ ] Verify POS retry does not double-commit
- [ ] Finance sign-off on reserved-vs-sold reporting

## 4. Daily ops

- Watch sell-through vs quota by channel
- Investigate aged reservations
- Rebalance hot channels before marketing spikes
- Reconcile sold serials to settlement files

## Related

See the main [README](../README.md). Product context: [electronic voucher distribution software](https://evdsystem.com/evd-software/electronic-voucher-distribution-software/).

## 5. Partner API notes

- Contracted buckets should show remaining vs reserved in the partner portal
- Under-used buckets: warn before reclaim; reclaim with notice per contract
- Rate-limit sell bursts that would empty a shared overflow unfairly
