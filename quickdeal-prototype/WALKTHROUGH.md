# QuickDeal — Stakeholder Walkthrough Script

A 20-minute live walkthrough designed for product, sales leadership, and engineering. Use the structure below as a talk-track; the flagged demo paths are tested and produce clean outputs.

---

## 0. Before the meeting (2 min setup)

Open the hosted URL in a fullscreen browser tab. Pre-pick **Driver A17** (LIC, has an active package deal) so you're one click from the swap demo if conversation goes there. Have this doc open on a second screen.

---

## 1. Frame the problem (2 min)

Open with the current state, briefly:

> Today, creating a deal in NetSuite means navigating to the Vehicle record, clicking "Quick Deal," and then editing fields on a 600+ field record. The form is long, the order is inverted from how a salesperson actually thinks about a deal, and a meaningful fraction of every deal is the rep manually re-typing values that should be defaults. We have **13 sales channels** and **5 locations** today, and the current UI doesn't gate any of them — a rep can pick an OPLS deal at a location that doesn't offer OPLS, or send an LA-only RTO product to a New York customer.

Set up the proposal:

> What I'm going to show you is a clickable prototype of a new flow we're proposing — same data, same NetSuite back-end, but a single-page wizard that inverts the flow to start from the customer, gates options to only what's possible, and pulls all defaults from existing records.

---

## 2. The five sections — happy path (5 min)

**Demo Path A — happy path, no swap.**

Click through with narration:

1. **Customer** → search "A1" → pick **Driver A1**.
   Talking point: *"Search is fuzzy across name, license, phone, email. The eligible-only toggle hides anyone whose underwriting status isn't Re-Approve, Approved, or Submitted to Broker — so the rep can never accidentally start a deal with someone who isn't cleared."*

2. **Location** → click **LIC — Dealership**.
   Talking point: *"Five real Tower locations. Picking the location is what gates everything below."*

3. **Deal type** → pick **Lease**, then sales channel **LTOWPC**.
   Talking point: *"Notice we only see Lease, Finance, Rental/OPLS, Other — that's because LIC Dealership has channels in all four families. If we were at Yankee Lot, only Lease and Rental would be enabled because that's what Yankee Lot actually offers. The sales channel dropdown is in volume order — LTOWPC at the top because it's 49% of all our active deals."*

4. **Deal terms** → leave start date as default → click **Confirm terms & continue**.
   Talking point: *"Three decisions, not thirty. Insurance surcharge is off by default but pre-populated for customers whose NetSuite profile has a value. Deposit shows for rental deals, frequency for finance deals — the form adapts to the channel."*

5. **Vehicle** → pick any row.
   Talking point: *"Each row is a vehicle × bucket × sales channel combo — exactly the shape your saved searches return today. The same Stock # can show up multiple times under different packages because the same vehicle can be sold as either a 3.5-year lease or a 4-year extended lease."*

6. **Review & submit** → highlight the auto-filled fields, click **Submit**.
   Talking point: *"Twelve fields auto-filled from the package, customer, and terms. The rep only had to type a start date and click Confirm. In the current flow this same deal takes 5–7 minutes; this took us under 30 seconds."*

---

## 3. Post-submit lifecycle (3 min)

After the submit, the section transforms into a five-stage timeline:

`Submitted → Manager review → Approved → Contract & payment → Active`

Click **Approve as manager** (the demo button in the purple panel) to advance.

Talking point: *"In production this is the manager's UI. They get a notification, open the deal, see exactly what we just submitted, and approve or reject. Reject sends a comment back to the rep. Approve unlocks the next two parallel steps."*

Once approved, point to the two cards:

- **Contract** → click "Send for e-signature."
  Talking point: *"This is where Adobe Sign comes in. Customer gets the contract on their phone or email."*

- **Payment** → click "Start payment" → choose "Take payment in person" → "Charge card."
  Talking point: *"Two paths. POS card entry at the counter, or send a payment link to the customer's phone. Both feed the same payment-received signal."*

Once both cards are done, point to the active stage:

> *"Vehicle is delivered. Deal is active. The rep never left this page, and the entire flow took the time it would have taken just to navigate the current NetSuite UI."*

---

## 4. The swap path (3 min)

**Demo Path B — customer with active deal.**

Reload to start fresh. Search for **A17** → pick **Driver A17**.

> *"This customer already has an active deal with us — a $580/week package rental on a Honda Accord. The system pulls that context from NetSuite the moment we identify the customer."*

Read the active deal banner aloud, then point to the swap prompt at the bottom.

Click **Yes — swap this deal**. Type a reason ("Customer upgrading to newer vehicle"). Click **Continue with swap →**.

> *"Reps used to manually flag swaps in a free-text status field. Here it's a structured event — the old deal stays linked, the reason is captured, the new deal is parented."*

Continue to **LIC Dealership → Lease → LTOWPC**.

In **Deal Terms**, the section now shows the old deal's terms at the top, with two radio cards: **Inherit from active deal** vs **Set new terms**.

> *"The default is Inherit — most swaps keep the same surcharge, deposit, and frequency. Notice the small 'Inherited from #10412-22' badges next to each affected field. The rep can override any of them, but the burden is on overriding, not setting."*

Pick a vehicle, submit, and let stakeholders see the swap context surface in the manager-approval banner.

---

## 5. Eligibility gating in action (2 min)

**Demo Path C — show the system saying no.**

Reload. Pick **Driver A1** → click **Yankee Lot (06)**.

Show the deal-type cards. Only **Lease** and **Rental / OPLS** are enabled.

> *"Yankee Lot is a satellite — only LTOWPC and OPLS are available there. We're not letting the rep accidentally book a Finance deal that the location can't fulfill. Same logic for LA — pick LA Mission and you'll see only Lease is enabled, because RTO-LA is the only LA-eligible channel."*

Try **LA — Mission** to demonstrate the LA gating.

Then go back to **LIC Dealership** and show the rejected-customer path: turn off "Eligible only" in the search → search **A11** (Rejected status) → row renders disabled and unclickable.

> *"We can't accidentally take an underwriting-rejected customer through to a deal. The system enforces the boundary."*

---

## 6. Implementation phasing (3 min)

Switch to talking, no demo:

> *"What you're seeing is a prototype — synthetic data, mocked manager approval. The implementation has three phases."*

**Phase 1: Data & APIs (~1–2 weeks)**
- Suitelet host page, deployed to NetSuite via SDF
- 5 RESTlets: searchPreLeads, listSalesChannels, searchAvailableVehicles, createDeal, approve/reject
- A "Deal Type Defaults" record (or augmentation of existing Sales Channel record) wired to a config layer
- Auth via NetSuite cookie session — no separate login

**Phase 2: UI shell & validation (~1–2 weeks)**
- Bundle this prototype as the SPA shell, hosted from the File Cabinet
- Wire the wizard steps to live RESTlets
- Sandbox testing with three real reps from each location

**Phase 3: Lifecycle integrations (~2–3 weeks)**
- Adobe Sign integration for contracts
- POS card-take + SMS payment-link integration
- Manager approval routing by location/role
- Deal-status state machine with audit log

**Total: ~5–7 weeks of focused engineering, plus parallel UAT.**

---

## 7. Decisions to surface (3 min)

End the meeting with explicit asks. Don't bury these:

1. **Where do default values live?** Today they're scattered across reps' heads. We need either a "Deal Type Defaults" custom record, or to augment the existing Sales Channel record with default fields. Either is fine, but we need a single source of truth.

2. **Manager approval routing.** Today the prototype hard-codes one approver. Production needs routing by location, deal value, or sales channel — what's the rule?

3. **Adobe Sign template.** Do we have one template per deal type, or one template with merged fields? Affects what the contract-send button does.

4. **Customer app payment integration.** What's the current API for "send payment link to customer phone"? If it doesn't exist, that becomes a sub-project.

5. **Pilot scope.** Roll out to one location first (recommend LIC Dealership since it's the highest-volume), all reps; or one rep per location to validate cross-location patterns?

6. **Sunset of current flow.** Run in parallel for 30 days, or hard-cutover? Recommend parallel — gives us a fallback and a comparison set.

---

## Anticipated questions

**Q: Does this replace the existing Quick Deal button?**
For Phase 1, no — we add a parallel "New Deal (QD)" link in the menu. After validation we deprecate the current flow.

**Q: What happens if NetSuite is slow or down?**
Same as today — the wizard is a thin layer on RESTlets. If RESTlets time out, the user sees a clear error and the deal isn't half-created.

**Q: How do we handle multi-customer deals or cosigners?**
Out of scope for v1. Currently <2% of deals have a cosigner. We can add a "Cosigner" step in v2.

**Q: Mobile?**
Desktop-first. Most reps use desktops at the dealership counters. We'd add responsive layout in v2 if mobile usage emerges.

**Q: How does this affect the underwriting flow?**
It doesn't. Underwriting still happens upstream in the pre-lead pipeline; this flow only consumes the result. Reps can't initiate a deal for an ineligible pre-lead.

**Q: Why this specific section ordering?**
Customer-first matches how reps actually think and removes wasted effort if the customer isn't eligible. Location next because it gates everything downstream. Deal type before vehicle because the channel determines which vehicles even appear. Terms before vehicle because terms-mode (inherit vs new) affects the package the rep should pick.

**Q: How accurate is the synthetic data?**
Distributions match the active-deals export — 13 sales channels with real volumes, 5 real locations, real subsidiary mix, real vehicle bucket types. Names and DLs are placeholders.

---

## After the meeting

1. Send the recording (if recorded) and the hosted link to attendees who couldn't make it.
2. Capture decisions surfaced in section 7 above and assign owners.
3. Schedule a 30-min follow-up with engineering to scope the SDF project (the SDF skeleton is already built — see `QuickDealPOC/` in our outputs folder).
4. Identify two pilot reps for Phase 2 UAT.
