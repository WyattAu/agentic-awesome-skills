| 7 days before expiry | Email: "Your card expires in 7 days; update now" |
**Email template; Card expiring:**
- [Product] Team
- **Stripe**: Automatic; enabled by default
- After a failed payment is recovered: "Add a backup to prevent future interruptions" (best timing; they felt the pain)
| `insufficient_funds` | Soft | Temporarily low balance | Yes; retry in 2-3 days |
| `processing_error` | Soft | Gateway/network issue | Yes; retry within 24h |
| `expired_card` | Hard | Card is expired | No; request new card |
| `stolen_card` | Hard | Card reported stolen | No; request new card |
**Stripe (Smart Retries; recommended):**
Subject: Action needed; your payment didn't go through
This happens sometimes; usually a quick card update fixes it.
- [Product] Team
Subject: Quick reminder; update your payment for [Product]
Just a heads-up; we still haven't been able to process your
- [Product] Team
Your data won't be deleted; you can reactivate anytime by
- [Product] Team
If you intended to cancel, no action needed; your account
- [Product] Team
| Visibility | In-app banner: "Payment past due; update to continue" |
- `invoice.payment_failed`; trigger dunning
- `invoice.paid`; cancel dunning, restore access
- `customer.subscription.updated`; status changes
- `customer.subscription.deleted`; final cancellation
- Don't block the product; let them continue using it