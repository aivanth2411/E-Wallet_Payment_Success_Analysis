## 📂 Project Background

The project analyzes the decline in bill payment success rate on an e-wallet app since August 2023 using transactional, session, event log, and error data. Power BI and the MECE framework were applied to identify failure points across the payment journey, including authentication, gateway redirects, and app version issues. Key insights highlighted correlations between failure rates and user segments, device types, and network quality. The analysis led to actionable recommendations to optimize payment flows and reduce error impact.

## 🔗 Data Structure & ERD (Entity Relationship Diagram)


## 📝 Executive Summary
### 1. Situation (Business Context)
- Transactions increased after the 3.9.6 update on July 28th .
- Billing payment transaction accounts for the greatest share (37%).
### 2. Complication (Business Problems)
- **Declining Success Rates**: Overall payment success rate dropped by 5% between July and August.
- **Authentication Bottlenecks**: High drop-off rates driven by 3DS and OTP timeouts, specifically during the authentication
phase.
- **Card Performance Issues**: Credit and Debit cards underperformed other methods with a peak 14% error rate.
- **Segment Risk**: Dormant users experienced disproportionately higher error rates compared to active users
### 3. Questions (Business Concerns)
- How can we stabilize the backend infrastructure and optimize the authentication flow to reduce the 18% drop-off?
- How can we successfully re-engage our dormant and user segments aged 35-44?
- How can we improve the customer support system with in-time communication and insightful information?
### 4. Answers (Business Solutions)
- Reduce authentication failures and backend-related drop-offs.
- Payment flow optimization & Root cause validation by performing A/B testing and comparison between versions.
- Improving real-time customer support and re-engaging with dormant users through marketing campaigns.

## 📝 Findings
1. The payment success rate declined despite an increase in total transactions

## ✨ Recommendations

## Deliverables
- `dashboard.pbix` – Power BI dashboard
- `report.pptx` – Business presentation and recommendations
- `dataset_dictionary.md` – Data documentation
