# Marketing Analytics Metrics

## 1. Key Metrics

### Ad Spend:

### Revenue:

### Profit:
`= Revenue - Ad Spend`

### Impressions:
Number of times an ad/post/content was shown (screen displays).

### Reach:
Unique number of users who saw the ad at least once.

### Awareness:
User views of the ad; `impression`, `view`. It is top of the funnel.

### Engagement:
User interactions with the ad; `like`, `share`, `comment`, `click`

### Lead (Intent):
User purchase/subscription intent without conversion; `add_to_cart`, `signup`, `form_submit`

### Conversion:
User completion of the desired business action; `purchase`, `subscription`, `download`

### Return on Investment (ROI):
Measures ad profitability rate;	`= (Ads Revenue - Ad Spend) * 100 / Ad Spend`

### Return on Ad Spend (ROAS):
`= Revenue / Ad Spend`

### Cost per Click (CPC):
Measures how much is paid each time a user clicks on an ad in pay-per-click (PPC) campaigns;	`= Ad Spend / Clicks`

### Cost per Acquisition (CPA):
Measures how much is paid to acquire one conversion (e.g., purchase, signup, download); `= Ad Spend / Conversions`. A conversion is usually defined by the campaign’s objective.

### Click-through Rate (CTR):
Percentage of people who click on a link, ad, or call-to-action after seeing it; `= Clicks * 100 / Impressions`

### Engagement Rate:
Percentage of people who like, comment on, or shared a post after seeing it; `(Likes + Comments + Shares) * 100 / Impressions`

### Conversion Rate:
`= Conversions * 100 / Clicks`

### Customer Lifetime Value (CLV):
Total revenue expected from a customer during their entire relationship with the business; `= Average_Order_Value (AOV) * Purchase_Frequency (PF) * Customer_Lifespan (CL)`. Ideally, `CLV >> CAC`

### Average Order Value (AOV):
`= Total Revenue / Total Orders`

### Purchase Frequency (PF):
`= Total Orders / Total Customers`

### Customer Lifespan (CL):
Lifespan of a customer (e.g., years, months); `= (Max Order date - Min Order date)`

### Customer Acquisition Cost (CAC):
Average cost a business spends to acquire one new customer;	`= Marketing Spend / New Customers`. **Components of Costs:** Advertising spend (digital ads, TV, print, etc.), Sales team salaries and commissions, Marketing software/tools and technology, Creative & content production costs, Overheads directly tied to customer acquisition.

## Ads Event Hierarchy for Funnel Analysis

| Funnel Stage   | Event Example         | What it Indicates           |
| -------------- | --------------------- | --------------------------- |
| **Awareness**  | Impression            | Ad views                    |
| **Engagement** | Like, Comment, Share  | Ad interactions             |
| **Interest**   | Click                 | Link or landing page visits |
| **Conversion** | Purchase, Subscribe   | Intended action completions |

## Efficiency Metrics base on Goals
In analytics, you never use just one conversion rate — you calculate multiple rates to understand where `drop-offs` happen.

| Metric (Conversion Goal)       | Formula                                     | Insight                                |
| ------------------------------ | ------------------------------------------- | -------------------------------------- |
| **Click-through rate (CTR)**   | `Clicks ÷ Impressions`                      | Are users interested enough to click?  |
| **Engagement Rate**            | `(Likes + Comments + Shares) ÷ Impressions` | Is the ad resonating socially?         |
| **Post-Click Conversion Rate** | `Purchases ÷ Clicks`                        | Is the landing page convincing?        |
| **Overall Conversion Rate**    | `Purchases ÷ Impressions`                   | What’s the overall funnel efficiency?  |

## SQL example

```sql
SELECT
    campaign_id,
    SUM(CASE WHEN event_type = 'Impression' THEN 1 END) AS impressions,
    SUM(CASE WHEN event_type = 'Click' THEN 1 END) AS clicks,
    SUM(CASE WHEN event_type IN ('Like', 'Comment', 'Share') THEN 1 END) AS engagements,
    SUM(CASE WHEN event_type = 'Purchase' THEN 1 END) AS purchases,

    ROUND(SUM(CASE WHEN event_type = 'Click' THEN 1 END)::decimal 
          / NULLIF(SUM(CASE WHEN event_type = 'Impression' THEN 1 END), 0) * 100, 2) AS ctr_percent,

    ROUND(SUM(CASE WHEN event_type = 'Purchase' THEN 1 END)::decimal 
          / NULLIF(SUM(CASE WHEN event_type = 'Click' THEN 1 END), 0) * 100, 2) AS post_click_conversion,

    ROUND(SUM(CASE WHEN event_type = 'Purchase' THEN 1 END)::decimal 
          / NULLIF(SUM(CASE WHEN event_type = 'Impression' THEN 1 END), 0) * 100, 3) AS overall_conversion

FROM staging.ad_events
GROUP BY campaign_id;
```
