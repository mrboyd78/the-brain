# 07-segment-based-onboarding.md

## 1. Executive Thesis
In third-party app ecosystems (Shopify, Atlassian, HubSpot), a "one-size-fits-all" onboarding flow is a primary driver of churn. Adoption logic is fundamentally split between **"Outcome-Driven" Casual Users** (who need immediate, low-friction utility) and **"Architecture-Driven" Power Users** (who need control, documentation, and sandbox environments). Success in these ecosystems depends on the app's ability to "Branch" the onboarding experience within the first 60 seconds based on the user's technical confidence and business complexity.

## 2. What the Evidence Shows
*   **Shopify Segmentation:** Evidence from Shopify App Store reviews shows that "Mom-and-Pop" merchants (Casual) prioritize "Easy to use" and "Fast setup," while "Plus" (Enterprise) merchants prioritize "API flexibility" and "Customization." Apps that force an Enterprise merchant through a 5-step "Introduction to E-commerce" tutorial see significant drop-off.
*   **Atlassian Role-Based Flows:** In the Atlassian Marketplace, successful apps (e.g., Tempo) provide different entry points for "Admins" (Setup-heavy) vs. "End-Users" (Utility-heavy). Forcing a Jira Admin to watch a "How to log a ticket" video creates "Expert Friction."
*   **HubSpot Data-Confidence Gaps:** HubSpot’s own onboarding documentation identifies a "Confidence Gap" in low-tech users. These users are "Automation-Anxious" and require "Sample Data" to feel safe, whereas RevOps Power Users find sample data "clutter" and want an immediate "Mapping Interface."
*   **Branching Survey Success:** Top-performing apps in all three ecosystems (e.g., Klaviyo for Shopify, Insight for Jira) use a "1-Question Signup Survey" to segment users before the first dashboard view.

## 3. Onboarding and Adoption Differences by Segment

| Segment | First-Use Anxiety | Setup Tolerance | Guided Help Needs | First-Value Expectation |
| :--- | :--- | :--- | :--- | :--- |
| **Casual / Low-Tech** (e.g., Small Shopify Merchant) | **High:** Afraid of "breaking" the store or losing data. | **Low:** Will abandon if setup takes > 10 minutes or requires code. | **High:** Needs "Hand-holding" (Checklists, Videos, Live Chat). | **Immediate:** Wants a visible change on their store within minutes. |
| **Power / High-Tech** (e.g., Atlassian Admin, RevOps) | **Low:** Confident in technical tools; anxiety is about "Scalability" and "Security." | **High:** Willing to spend hours on "perfect" mapping/configuration. | **Low:** Prefers "Documentation" and "Searchable FAQs" over pop-ups. | **Deferred:** Understands that "Setting up the architecture" is the first win. |
| **The "Forced" User** (e.g., Employee using a tool an Admin bought) | **Neutral:** Just wants to "get the job done" with minimal extra work. | **Zero:** Any friction is seen as an "Annoyance" or "Work Impediment." | **Medium:** Needs "Just-in-Time" tips at the moment of action. | **Utility-Based:** Wants the tool to be "invisible" and automate their manual tasks. |

## 4. Which Segment Appears Easiest to Activate and Why
The **"Casual / Low-Tech"** segment is the easiest to *activate* but the hardest to *retain* long-term.
*   **Why they are easy to activate:** They are highly responsive to "Templates" and "Default Settings." If an app can offer a "1-Click Setup" (e.g., a Shopify 'Flash Sale' app), the activation happens instantly.
*   **The Trap:** Because their loyalty is tied to "Speed of Utility," they will churn just as quickly if a competitor offers a slightly faster or cheaper "1-Click" solution.
*   **Counter-Point:** The **"Power User"** segment is harder to activate (requires intense setup) but once "In-Production," they are virtually impossible to churn due to high switching costs and "Data Gravity."

## 5. Strategic Implications for a Founder
*   **Implement "Adaptive Friction":** Add more steps (Security, Mapping, Scaling) for Power Users while stripping them away for Casual Users.
*   **"Show, Don't Tell" for Casuals; "Document, Don't Show" for Experts:** Use interactive walkthroughs for the low-tech segment, but provide a "Skip to API Docs" button for the high-tech segment.
*   **Segment Your Support:** Casual users need "Customer Success" (How do I use this?); Power users need "Technical Support" (Why did this webhook fail?).

## 6. Risks, Counterarguments, and Uncertainty
*   **The "Middle" Segment:** Most research focuses on the extremes (Casual vs. Power). The "Growing Mid-Market" segment (who is outgrowing casual tools but isn't ready for Enterprise complexity) is often neglected and difficult to design for.
*   **Segment Migration:** A user may start "Casual" but become a "Power User" within 90 days. If the onboarding is "too simple," it may feel "limited" as the user grows.
*   **Self-Selection Bias:** Users often misidentify their own segment in surveys (e.g., an "Ambitious Beginner" claiming to be an "Expert"), leading to them being put in a flow that exhausts them.

## 7. Final Recommendations
1.  **Mandatory 60-Second Branching:** Ask "What is your primary goal?" and "How technical are you?" before showing the first screen.
2.  **Provide a "Safe Sandbox" for High-Stake Apps:** If your app handles Shopify Payments or Jira Workflow logic, give Power Users a "Test Mode" so they can "Adopt" the app without "Live Risk."
3.  **Build "Empty State" Templates:** For Casual users, never show a blank screen. Populate it with "Sample Data" that they can delete later once they feel confident.

## 8. Source List
*   **Shopify Polaris:** "Segmentation in Onboarding" case studies.
*   **Atlassian University:** "Admin vs. User" training tracks.
*   **HubSpot Partner Blog:** "RevOps Onboarding vs. SMB Onboarding" analysis.
*   **UserOnboard.com:** Teardowns of segmented onboarding (e.g., Canva, Slack, Trello).
*   **SaaS Industry Reports:** (e.g., OpenView Partners) on "Product-Led Growth for Different Segments."

---
**Internal Adversarial Review:**
1.  **Over-simplification of "Low-Tech":** Many "Small Business Owners" are actually quite tech-savvy. The assumption that "Small = Low-Tech" is a biased founder trap.
2.  **Ignoring "Organizational Friction":** In Atlassian/HubSpot, onboarding isn't just about the *user*, it's about the *team*. The research focuses heavily on the individual's technical level and misses the "Approval Workflow" friction.
3.  **Inference of "High Setup Tolerance":** Just because a Power User *can* tolerate a 2-hour setup doesn't mean they *want* to. High-tech users also value speed.
4.  **Static Segmentation:** The thesis assumes segments are fixed. In reality, a "Power User" might be "Casual" when trying a new experimental feature.
5.  **Platform Limitations:** Some ecosystems (like Shopify) have very rigid "Install" flows that make "Branching" difficult to implement before the user is already "In-App."

*Revised to focus on "Adaptive Friction" and "Outcome-Driven" vs. "Architecture-Driven" logic.*
