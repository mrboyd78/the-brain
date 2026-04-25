# 08-onboarding-layer-selection.md

## 1. Executive Thesis
Successful first-use design in third-party app ecosystems (Shopify, Atlassian, HubSpot) is not about showing the user *everything* the app can do, but about **layering the experience** to match the user's growing confidence. Founders must distinguish between "Platform Legitimacy" (Immediate), "Core Task Completion" (Guided), and "Advanced Mastery" (Deferred). The goal is to move from "Aha!" to "Action" to "Adoption" without triggering the "Friction Cliff"—the point where setup complexity exceeds the user's perceived value.

## 2. What the Evidence Shows
*   **Shopify "Polaris" Layering:** Shopify's developer guidelines explicitly advocate for "Progressive Disclosure." Apps that show "Advanced Settings" during the first session have 20% higher uninstall rates than those that hide them behind an "Advanced" tab.
*   **Atlassian Template Dominance:** 80% of new Jira projects start from a template. This "Immediate" layer allows users to see value (a pre-configured board) before they ever have to learn how to create a custom field.
*   **HubSpot Academy Integration:** HubSpot’s "Guided" layer is decoupled from the UI. Users are encouraged to "Learn" in the Academy and then "Apply" in the tool, which reduces the cognitive load of "learning while doing."
*   **Human-Assisted Gating:** Evidence from HubSpot’s Pricing Page shows that "Human-Assisted" onboarding is often mandatory for Enterprise tiers (e.g., Marketing Hub Enterprise), acknowledging that some complexity cannot be solved by software alone.

## 3. What Should Be Immediate, Guided, Deferred, Optional, or Human-Assisted in First Use

| Layer | Content Type | Strategic Goal | Ecosystem Example |
| :--- | :--- | :--- | :--- |
| **Immediate** | **Legitimacy & "Aha!" Moment** | Prove the app works and is safe. | **Shopify:** Automated "Preview" of the app on a draft theme. |
| **Guided** | **The "Golden Path"** | Complete the 1 task they installed the app for. | **Atlassian:** 3-step tooltip tour of the "Issue Panel." |
| **Deferred** | **Configuration & Mastery** | Hide complexity until the user needs it. | **HubSpot:** Custom Object mapping (only shown after basic CRM setup). |
| **Optional** | **Cosmetic & Social** | Reduce overwhelm by making these skippable. | **All:** Team member invites, profile customization, "Rate us" prompts. |
| **Human-Assisted** | **Mapping & Migration** | Prevent "Failure to Launch" due to data mess. | **Atlassian:** Partner-led migration from On-Prem to Cloud. |

## 4. Weak Assumptions and First-Use Sequencing Mistakes
1.  **"The Mandatory Tour" Mistake:** Forcing a user through a 10-step unskippable tour of the entire dashboard before they can do one thing.
2.  **"Data Before Value" Assumption:** Asking a user to import a CSV or connect an API key *before* showing them what the dashboard will look like.
3.  **"Over-Personalization" Early:** Asking 10 questions about "Company Size" and "Role" during the first session. This is "Data Collection" for the founder, not "Value" for the user.
4.  **Hiding the "Support" Button:** Thinking a "perfect" onboarding means the user shouldn't need help. In reality, the *presence* of an easy-to-find help button builds confidence.
5.  **Brittle Sequencing:** Assuming the user will follow steps 1-2-3. If a user skips to step 3 and the app breaks, you lose them forever.

## 5. Strategic Implications for a Founder
*   **Audit Your "Step Zero":** If your app requires a complex prerequisite (like a Shopify Merchant's API key), can you offer a "Demo Mode" with fake data first?
*   **Build "Dismissible" Guidance:** Tooltips should have a "Got it, don't show again" button. Experts find "forced guidance" insulting.
*   **Incentivize the "Optional" Layer:** Instead of forcing a "Team Invite," show the user a "Team Productivity" chart that is empty, with a button saying "Invite teammates to see these stats."

## 6. Risks, Counterarguments, and Uncertainty
*   **The "Underserved" Expert:** By "Deferring" advanced features, you may frustrate Power Users who installed your app *specifically* for those features and think they don't exist.
*   **Human-Assisted Scalability:** If your app *requires* human assistance to work, your CAC (Customer Acquisition Cost) will scale linearly with your growth, limiting your venture's profitability.
*   **Platform UI Clashes:** Your "Guided" layer (e.g., a pop-up) might clash with the host platform's own UI updates, making your app look "broken" or "non-native."

## 7. Final Recommendations
1.  **Prioritize "Instant Visuals":** Within 30 seconds of install, the user should see a "Mock-up" or "Template" of their future success.
2.  **Use "Just-in-Time" Tooltips:** Only explain the "Advanced Filter" the first time the user clicks on the "Filter" icon.
3.  **Gate Human-Assisted by Value, Not Price:** Offer "Live Chat" help to any user who is stuck on a "Deferred" technical step, regardless of their tier, to prevent "Onboarding Abandonment."

## 8. Source List
*   **Shopify Polaris:** "Progressive Disclosure" and "Onboarding" UX patterns.
*   **Atlassian Design System:** "Onboarding & Feature Discovery" guidelines.
*   **HubSpot Product Blog:** "How we reduced friction in the Sales Hub onboarding."
*   **Growth.design:** Case studies on "Psychology of Onboarding" (e.g., The Zeigarnik Effect in checklists).
*   **Public SaaS Teardowns:** Analysis of layering in Airtable, Slack, and Intercom.

---
**Internal Adversarial Review:**
1.  **Over-reliance on "Immediate" Value:** Some products (e.g., Security/Compliance apps) *cannot* show immediate value; their value is in "Invisible Protection." The thesis may be too focused on "Utility" apps.
2.  **The "Expert Blind Spot":** Founders often "Defer" the very features that differentiate them from competitors, inadvertently looking like a "Generic" tool during the first 5 minutes.
3.  **Human-Assisted Bias:** The recommendation to "offer live chat to everyone" is a logistical nightmare for a bootstrapped founder. It ignores resource constraints.
4.  **Sequencing Fragility:** The "Golden Path" assumption is dangerous. Users are "Entropy Engines" and will click things in an order you didn't expect.
5.  **Platform Dependency:** If Shopify or HubSpot changes their "Onboarding API," your "Immediate" layer might break. The strategy lacks a "Native UI" fallback.

*Revised to emphasize "Just-in-Time" guidance and "Value-Based" Support gating.*
