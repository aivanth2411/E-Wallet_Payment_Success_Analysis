View full report here.

## 📂 Project Background

The project analyzes the decline in bill payment success rate on an e-wallet app since August 2023 using transactional, session, event log, and error data. The MECE framework was applied to identify failure points across the payment journey, including authentication, gateway redirects, and app version issues. Key insights highlighted correlations between failure rates and user segments, device types, and network quality. The analysis led to actionable recommendations to optimize payment flows and reduce error impact.

## 📊 Dataset Structure
This dataset simulates transaction data, user behavior, and system errors of an e-wallet over a two-month period (July 1, 2023 – August 31, 2023).

**Dataset sources:**
- `transactions.csv`: Detailed transaction history
- `sessions.csv`: Session information
- `event_logs.csv`: Step-by-step behavior logs
- `dim_user.csv`: User information
- `dim_event.csv`: Event categories
- `dim_error_categories.csv`: Error classification by three levels

## 🎯 Objectives
- Identify the main drivers behind the decline in payment success rates.
- Analyze user journey drop-off points.
- Investigate system and authentication errors.
- Provide actionable recommendations to improve conversion.
## 🔍 Key Findings
- Payment success rate declined from **92% to 87%**.
- The largest drop-off occurred **after authentication (18%)**.
- **OTP timeout and authentication issues** contributed over 46% of failures.
- **Dormant users and age group 35–44** recorded the highest error rates.
- **Credit and Debit Cards** showed the weakest performance.

## ✨ Recommendations
- Optimize OTP timeout and authentication flow.
- Fix payment gateway and app version issues.
- Run A/B testing on the updated payment flow.
- Improve real-time support and re-engagement campaigns for dormant users.

## Deliverables
- `data/dataset_dictionary.md`: Data documentation
- `docs/release_note.md`: Documents of released versions
- `report/Ewallet_Analysis_Report.pdf`: Product analysis and report
- `scripts/funnel_analysis.md`: Analysis of total users through each stage
- `scripts/funnel_metrics.md`: Calculations of key funnel metrics 

