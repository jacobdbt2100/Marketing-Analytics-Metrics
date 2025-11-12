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

## 2. Ads Event Hierarchy for Funnel Analysis

| Funnel Stage   | Event Example         | What it Indicates           |
| -------------- | --------------------- | --------------------------- |
| **Awareness**  | Impression            | Ad views                    |
| **Engagement** | Like, Comment, Share  | Ad interactions             |
| **Interest**   | Click                 | Link or landing page visits |
| **Conversion** | Purchase, Subscribe   | Intended action completions |

## 3. Efficiency Metrics base on Goals

In analytics, you never use just one conversion rate — you calculate multiple rates to understand where `drop-offs` happen.

| Metric (Conversion Goal)       | Formula                                     | Insight                                |
| ------------------------------ | ------------------------------------------- | -------------------------------------- |
| **Engagement Rate**            | `(Likes + Comments + Shares) ÷ Impressions` | Is the ad resonating socially?         |
| **Click-through rate (CTR)**   | `Clicks ÷ Impressions`                      | Are users interested enough to click?  |
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
## 4. Marketing Ads Effectiveness Analysis Approach

**A) Steps:**

- **Understand the Funnel**

    - `Impression` > `Click` > `Engagement` (like, share, comment) > `Purchase`

- **Define Key Metrics**

    - Start from `reach` and `engagement`, then `conversion`
    - Use `ratios`, not just raw counts (e.g. `Click-through rate` instead of `total clicks`)

- **Segment Data**

    - By `platform`, `ad type`, `campaign`, `audience` (age, gender, country), etc.

- **Compare Performance**

    - Across `campaigns` or `periods` to find what drives effectiveness

**B) Key Questions:**

| **Category**                                 | **Question**                                                                  | **Purpose**                            |
| -------------------------------------------- | ----------------------------------------------------------------------------- | -------------------------------------- |
| **Reach & Exposure**                         | How many people saw the ad? Which platforms deliver the highest reach?        | To identify visibility                 |
| **Engagement**                               | Which ads have the most interactions (likes, shares, comments)?               | To measure how engaging the content is |
| **Click Performance**                        | What percentage of impressions result in clicks (CTR)?                        | To gauge how compelling the ad is      |
| **Conversion**                               | What percentage of clicks lead to purchases (Conversion Rate)?                | To assess business impact              |
| **Cost Effectiveness** (if cost data exists) | How much does each click or conversion cost (CPC, CPA)?                         | To evaluate ROI                        |
| **Audience Insights**                        | Which age group or gender segment converts best?                              | To refine targeting                    |
| **Content Analysis**                         | Do video ads perform better than image ads?                                   | To guide creative strategy             |

**C) Most Important Metrics:**

| **Metric**                     | **Formula**                                         | **Meaning**                   |
| ------------------------------ | --------------------------------------------------- | ----------------------------- |
| **Impressions**                | Count of times ad shown                             | Reach/visibility              |
| **Clicks**                     | Count of link clicks                                | Interest generated            |
| **Click-Through Rate (CTR)**   | `(Clicks / Impressions) × 100`                      | Ad appeal effectiveness       |
| **Engagement Rate (ER)**       | `((Likes + Shares + Comments) / Impressions) × 100` | Audience interaction strength |
| **Conversion Rate (CR)**       | `(Purchases / Clicks) × 100`                        | Sales effectiveness           |
| **Cost per Click (CPC)**       | `Total Spend / Clicks`                              | Click efficiency              |
| **Cost per Acquisition (CPA)** | `Total Spend / Purchases`                           | Cost per conversion           |
| **Return on Ad Spend (ROAS)**  | `Revenue / Spend`                                   | Overall profitability         |

**D) Example Analysis Output (Simplified):**

| Campaign  | Impressions | Clicks | CTR (%) | Engagement Rate (%) | Purchases | Conversion Rate (%) | ROAS |
| --------- | ----------- | ------ | ------- | ------------------- | --------- | ------------------- | ---- |
| A (Video) | 10,000      | 800    | 8.0     | 5.5                 | 40        | 5.0                 | 2.5  |
| B (Image) | 12,000      | 480    | 4.0     | 7.2                 | 30        | 6.3                 | 1.8  |
| C (Text)  | 8,000       | 320    | 4.0     | 3.0                 | 12        | 3.8                 | 1.2  |

**E) Insights Example:**

1. **Video ads (Campaign A)** have the **highest CTR (8%) and ROAS (2.5)**, meaning they are both engaging and profitable.

> **Recommendation:** Allocate more budget to video creatives.

2. **Image ads (Campaign B)** show higher **engagement (7.2%)** but lower **conversion efficiency (6.3%)**.

> **Recommendation:** Use image ads earlier in the funnel (awareness), not for direct sales.

3. **Text-based ads (Campaign C)** underperform on all fronts.

 > **Recommendation:** Phase out or rework text content.

4. **Next step:** Segment by audience — e.g., 18–24 might engage more, but 25–34 may purchase more.

**F) Summary Framework for Future Use:**

| **Stage**  | **Goal**                   | **Key Metrics**            | **Improvement Focus**       |
| ---------- | -------------------------- | -------------------------- | --------------------------- |
| Awareness  | Increase visibility        | Impressions                | Targeting & ad placement    |
| Engagement | Drive interaction          | Engagement Rate            | Content & creativity        |
| Conversion | Turn clicks into purchases | CTR, Conversion Rate, ROAS | Landing page, offer, timing |
