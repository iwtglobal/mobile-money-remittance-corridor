# Mobile Money Remittance Corridor

An educational guide to **mobile money remittance corridors** and cross-border mobile money. Written for fintech, telecom, and payment teams evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Is a Mobile Money Remittance Corridor?

A **mobile money remittance corridor** is a defined send-and-receive path between two markets—or two wallet schemes—that lets a customer move value across borders into a mobile money wallet. It is the operational and commercial packaging of FX, compliance, partner rails, and cash-out rules for a specific origin–destination pair.

Cross-border mobile money is not “P2P with a currency code.” Each corridor carries KYC thresholds, sanctioned-party screening, FX disclosure, partner SLAs, and payout methods (wallet credit, agent cash-out, bank). Weak corridor design creates stuck funds, opaque FX spreads, and regulatory exposure.

### Why remittance corridors matter

- **Diaspora utility** — families expect predictable delivery into local wallets  
- **Compliance** — AML, sanctions, and source-of-funds rules apply per corridor  
- **FX transparency** — customers need rate and fee clarity before they send  
- **Partner reliability** — payout partners and switches define real-world success rates  

A corridor is a product and control surface, not just an API flag.

---

## Architecture Overview: Corridor Pipeline

```
Sender (origin wallet / app) ──► Quote (FX + fees + ETA)
        │
        ▼
Compliance screening (KYC / AML / sanctions)
        │
        ▼
Debit origin ledger / funding rail
        │
        ▼
Corridor switch / partner ──► Credit destination mobile money
        │
        ▼
Receipts, tracking & exception handling
```

### Core layers

| Layer | Responsibility |
|-------|----------------|
| **Corridor catalog** | Lists live origin–destination pairs, limits, and payout options |
| **Quote engine** | Produces time-bound FX, fees, and receivable amount |
| **Compliance** | Screens parties and applies corridor-specific thresholds |
| **Funding & remittance** | Debits origin and instructs the partner/switch |
| **Payout & track** | Credits destination wallet or agent; exposes status to both ends |

Operators keep corridor events reconcilable so finance can match origin debit, partner instruction, and destination credit.

---

## How a Mobile Money Remittance Corridor Works

### 1. Select corridor and request a quote

The sender chooses destination country, payout method, and amount. The quote engine returns FX, fees, and the amount the receiver should get—valid for a short window.

### 2. Collect and screen party data

Sender and receiver identifiers, purpose codes (where required), and KYC tier checks run against corridor policy. Soft declines should explain what is missing.

### 3. Fund and create a remittance instruction

Origin funds are captured (wallet debit or linked funding). A remittance ID ties the debit to the outbound instruction so retries stay idempotent.

### 4. Partner delivery and destination credit

The corridor partner or switch pays into the destination mobile money wallet (or agent network). Status moves from instructed → accepted → credited (or failed).

### 5. Confirm, notify, and reconcile

Both parties get tracking and receipts. Exceptions (rejected payout, wrong MSISDN) enter ops queues with clear reverse or re-route rules.

---

## Patterns and Use Cases

1. **Wallet-to-wallet diaspora corridor** — Origin prepaid wallet to destination MNO wallet.  
2. **Bank-funded send to mobile money** — Card/bank funding in origin, wallet credit in destination.  
3. **Agent cash-out last mile** — Destination credit available for agent withdrawal where wallets are thin.  
4. **Operator partnership corridors** — Two MNOs interconnect for preferential FX and limits.  
5. **B2B payroll corridors** — Employer bulk sends into worker mobile money accounts.

Platforms such as EVD System / MoboGage sit beside telecom digital-value stacks where remittance corridors often coexist with airtime, vouchers, and domestic mobile money rails.

---

## Implementation Considerations

- **Quote expiry** — never settle on a stale FX quote without re-quote policy  
- **Idempotent remittance IDs** — partner retries must not double-pay  
- **Beneficiary validation** — verify MSISDN/wallet eligibility before irreversible debit when possible  
- **Partial failure** — distinguish “partner accepted” from “wallet credited”  
- **Limit ladders** — escalate KYC as corridor volume grows  
- **Disclosure** — show rate, fee, and receivable amount in one screen  

Corridor design should optimize for compliance clarity and delivery certainty, not only headline FX.

---

## FAQ

**Is a corridor the same as an FX rate?**  
No. The rate is one input. A corridor also includes partners, limits, compliance rules, and payout methods for a specific path.

**What happens if the destination MSISDN is wrong?**  
Best practice is early validation. If funds are already instructed, ops needs a documented reclaim or re-route process with the partner.

**Do all corridors support instant credit?**  
No. Some are near-real-time; others batch. Product UX should show expected ETA from the quote.

**Who owns AML for cross-border mobile money?**  
Typically both origin operator and corridor partners share obligations. Controls must be explicit in contracts and systems.

**Can one platform run many corridors?**  
Yes. A corridor catalog with per-path limits and partners is the usual model.

**How does this relate to MoboGage / EVD System?**  
EVD System is MoboGage's electronic voucher distribution and management platform family; remittance corridors often sit next to telecom wallet and digital product rails. See the [EVD System home](https://evdsystem.com/) and [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) pages when evaluating adjacent stack fit.

---

## Further Reading / Related Industry Resources

- [EVD System home](https://evdsystem.com/) — platform overview for digital value distribution  
- [Electronic voucher management system (EVD telecom)](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) — EVMS context in telecom programs  

See also [docs/glossary.md](./docs/glossary.md) for corridor terminology.

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not a substitute for vendor due diligence or regulatory advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
