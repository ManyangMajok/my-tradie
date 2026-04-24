# Design → Spec mapping

Lookup table for every mobile screen. Use this to jump from a Stitch design (or the inventory count) to the corresponding behavioural spec section.

**Always read the spec section for behaviour, acceptance criteria, and edge cases.**
**Always look at the Stitch file for visual layout, spacing, colour use, and component structure.**

See `docs/11-mobile-apps-spec.md §13` for how to use these together when building.

---

## Tradie app (14 screens)

| # | Screen name | Spec section | Design file |
|---|---|---|---|
| 1 | Splash / Auth-Check | `11-mobile-apps-spec.md §9.1` | `docs/design/tradie/09.1-splash.tsx` |
| 2 | Login | `§9.2` | `tradie/09.2-login.tsx` |
| 3 | First-Time Onboarding (3-screen pager) | `§9.3` | `tradie/09.3-onboarding.tsx` |
| 4 | Leads Inbox | `§9.4` | `tradie/09.4-leads-inbox.tsx` |
| 5 | Lead Detail (Pre-Accept) | `§9.5` | `tradie/09.5-lead-detail.tsx` |
| 6 | Jobs Inbox | `§9.6` | `tradie/09.6-jobs-inbox.tsx` |
| 7 | Job Detail (Post-Accept) | `§9.7` | `tradie/09.7-job-detail.tsx` |
| 8 | Completion Form Wizard (5-step modal) | `§9.8` | `tradie/09.8-completion-form.tsx` |
| 9 | Performance Dashboard | `§9.9` | `tradie/09.9-performance.tsx` |
| 10 | Service Areas | `§9.10` | `tradie/09.10-service-areas.tsx` |
| 11 | Categories | `§9.11` | `tradie/09.11-categories.tsx` |
| 12 | Availability | `§9.12` | `tradie/09.12-availability.tsx` |
| 13 | Subscription Status | `§9.13` | `tradie/09.13-subscription.tsx` |
| 14 | Profile & Settings Root | `§9.14` | `tradie/09.14-profile-settings.tsx` |

## Member app (14 screens)

| # | Screen name | Spec section | Design file |
|---|---|---|---|
| 1 | Splash / Auth-Check | `§11.1` | `docs/design/member/11.1-splash.tsx` |
| 2 | Login | `§11.2` | `member/11.2-login.tsx` |
| 3 | Member Onboarding (2-step) | `§11.3` | `member/11.3-onboarding.tsx` |
| 4 | Home Dashboard | `§11.4` | `member/11.4-home.tsx` |
| 5 | Submit Request Wizard (7-step modal) | `§11.5` | `member/11.5-submit-request.tsx` |
| 6 | Properties List | `§11.6` | `member/11.6-properties-list.tsx` |
| 7 | Property Detail / Edit | `§11.7` | `member/11.7-property-detail.tsx` |
| 8 | Jobs List | `§11.8` | `member/11.8-jobs-list.tsx` |
| 9 | Job Detail (Member-Facing) | `§11.9` | `member/11.9-job-detail.tsx` |
| 10 | Review Form | `§11.10` | `member/11.10-review-form.tsx` |
| 11 | Saved Tradies | `§11.11` | `member/11.11-saved-tradies.tsx` |
| 12 | Membership Status | `§11.12` | `member/11.12-membership.tsx` |
| 13 | Profile & Settings | `§11.13` | `member/11.13-profile-settings.tsx` |
| 14 | Support | `§11.14` | `member/11.14-support.tsx` |

## Global system states (3 states)

Cross-cutting UI used across both apps. Rendered by shared `packages/ui/` components and composed into every screen.

| # | State name | Spec section | Design file |
|---|---|---|---|
| 1 | Offline Banner | `§9.15` (Part 1) | `docs/design/global/offline-banner.tsx` |
| 2 | Session Expired Modal | `§9.15` (Part 2) | `global/session-expired.tsx` |
| 3 | Maintenance Mode | `§9.15` (Part 3) | `global/maintenance-mode.tsx` |

---

## Submit Request Wizard — step-level mapping

The member's submit request wizard (§11.5) is a single multi-step modal. Stitch may export it as one file or as seven separate step files. If separate, add rows for each:

| Step | Purpose | Design file variants |
|---|---|---|
| 1 | Property select | `11.5-submit-request.tsx` or `11.5a-property-select.tsx` |
| 2 | Category | " or `11.5b-category.tsx` |
| 3 | Issue type | " or `11.5c-issue-type.tsx` |
| 4 | Urgency | " or `11.5d-urgency.tsx` |
| 5 | Description | " or `11.5e-description.tsx` |
| 6 | Photos | " or `11.5f-photos.tsx` |
| 7 | Review | " or `11.5g-review.tsx` |

## Completion Form — step-level mapping

Similar pattern for tradie completion (§9.8):

| Step | Purpose | Design file variants |
|---|---|---|
| 1 | Summary of work | `09.8-completion-form.tsx` or `09.8a-summary.tsx` |
| 2 | Invoice total | " or `09.8b-invoice.tsx` |
| 3 | Benefits honoured | " or `09.8c-benefits.tsx` |
| 4 | Photos + invoice upload | " or `09.8d-uploads.tsx` |
| 5 | Review & submit | " or `09.8e-review.tsx` |

---

## Check list for each screen build

When the agent is assigned a screen, it should:

- [ ] Read the spec section (e.g., `§9.4`)
- [ ] Open the matching Stitch file (e.g., `docs/design/tradie/09.4-leads-inbox.tsx`)
- [ ] Confirm all library imports in the Stitch file are in the whitelist (`09-development-guide.md §16`). Flag anything new.
- [ ] Identify all hardcoded colours in the Stitch file — map each to a token in `packages/ui/src/theme/tokens.ts`. Add tokens if genuinely new (rare).
- [ ] Identify all typography sizes in the Stitch file — map each to `typography.*` tokens.
- [ ] Build the screen using project primitives, hooks, and tokens — not by modifying the Stitch file.
- [ ] Run contrast check on every text element (WCAG AA, 4.5:1 body minimum).
- [ ] Verify no `BlurView` inside any `FlatList` item renderer (§12.4).
- [ ] Write acceptance tests matching the spec's acceptance criteria.
- [ ] Mark the PR "Design-matched" with a screenshot comparison if possible.

---

## When designs and spec diverge

Priority rules — spec vs design conflicts:

1. **Behaviour** (data, navigation, API call, state transition) → spec wins.
2. **Visual structure** (layout, spacing, colour choice, typography) → design wins.
3. **Copy** (labels, button text, error messages) → spec wins. Designs often use placeholder copy.
4. **Extra fields not in spec** → flag and ask. Either spec is incomplete or design added scope.
5. **Missing fields that are in spec** → build them per spec. Note "Added per spec, not in design" in PR.
6. **Icons** → prefer `lucide-react-native` equivalents for whatever Stitch used.

See `11-mobile-apps-spec.md §13.4` for more detail.

---

## Maintenance

This mapping document must be updated when:

- A new screen is added to the app (new row)
- A screen is renamed in the spec (update the section reference)
- Stitch exports are reorganised (update the file paths)

The agent must not add, remove, or rename rows in this table without explicit instruction. If a new screen is needed, it's a product decision, not a build one.
