# Glossary — Mobile Money Remittance Corridor

Educational terminology for **mobile money remittance corridors** and cross-border mobile money. Definitions are industry-oriented; vendor labels vary.

## Core Terms

| Term | Definition |
|------|------------|
| **Corridor** | Defined origin–destination path with partners, limits, FX, and payout rules |
| **Quote** | Time-bound offer of FX rate, fees, and receivable amount |
| **Receivable amount** | What the destination party should receive after fees/FX |
| **Funding** | Origin debit or linked payment that backs the remittance |
| **Remittance ID** | Durable instruction identifier used for idempotency and tracking |
| **Payout method** | Wallet credit, agent cash-out, or bank deposit at destination |
| **Switch / partner** | Intermediary that delivers value into the destination scheme |
| **Screening** | Sanctions / AML / PEP checks applied to parties and corridors |
| **ETA** | Expected time to destination credit or availability |
| **Reclaim / re-route** | Ops process when beneficiary data is wrong after instruction |
| **Limit ladder** | Increasing send/receive caps tied to stronger KYC |
| **Purpose code** | Classification of remittance reason where regulation requires it |

## Flow States (Typical)

```
quoted → screened → funded → instructed → partner-accepted → credited
                                              └→ failed / reclaim / ops review
```

## Roles

| Role | Concern |
|------|---------|
| **Sender** | Funds the transfer; needs FX and fee clarity |
| **Receiver** | Receives wallet credit or cash-out |
| **Origin operator** | Owns customer UX, funding, and origin compliance |
| **Corridor partner** | Delivers into destination mobile money or agent network |
| **Regulator / compliance** | Sets thresholds, reporting, and screening expectations |

## Design Notes

- Treat quotes as **time-bound**; re-quote when policy requires before debit.  
- Correlate **origin debit**, **remittance ID**, and **destination credit** on every success path.  
- Prefer explicit **pending** when partner ACK is delayed.  
- Publish receivable amount before authorization so disputes stay rare.

## Related Reading

Live product reading: [evdsystem.com](https://evdsystem.com/), [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/).

MoboGage / EVD System materials on evdsystem.com describe adjacent digital value distribution capabilities that often sit beside mobile money remittance programs in telecom markets.
