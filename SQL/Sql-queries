
-- ============================================
-- Query 1: Segment Performance Variance
-- Purpose: Calculate average margin per segment 
-- and variance from overall business average
-- ============================================

WITH segment_performance AS (
    SELECT 
        Segment,
        COUNT(*) AS Transactions,
        ROUND(SUM(Sales), 2) AS Total_Revenue,
        ROUND(SUM(Profit), 2) AS Total_Profit,
        ROUND(AVG("Profit Margin %") * 100, 2) AS Avg_Margin_Pct,
        ROUND(SUM(Profit) * 100.0 / SUM(Sales), 2) AS Profit_Revenue_Ratio
    FROM financial
	WHERE segment IS NOT NULL
    GROUP BY Segment
),
overall AS (
    SELECT ROUND(AVG("Profit Margin %") * 100, 2) AS Overall_Avg_Margin
    FROM financial
	WHERE segment IS NOT NULL
)
SELECT 
    s.*,
    o.Overall_Avg_Margin,
    ROUND(s.Avg_Margin_Pct - o.Overall_Avg_Margin, 2) AS Variance_From_Average
FROM segment_performance s
CROSS JOIN overall o
ORDER BY s.Avg_Margin_Pct DESC;


-- ============================================
-- Query 2: Discount Impact by Segment
-- Purpose: Analyse how discount bands affect 
-- margin within each customer segment
-- ============================================

WITH segment_discount AS (
    SELECT 
        Segment,
        "Discount Band",
        COUNT(*) AS Transactions,
        ROUND(AVG("Profit Margin %") * 100, 2) AS Avg_Margin
    FROM financial
    WHERE Segment IS NOT NULL
    AND "Discount Band" IS NOT NULL
    GROUP BY Segment, "Discount Band"
),
segment_total AS (
    SELECT 
        Segment,
        ROUND(AVG("Profit Margin %") * 100, 2) AS Overall_Segment_Margin
    FROM financial
    WHERE Segment IS NOT NULL
    GROUP BY Segment
)
SELECT 
    sd.Segment,
    sd."Discount Band",
    sd.Transactions,
    sd.Avg_Margin,
    st.Overall_Segment_Margin,
    ROUND(sd.Avg_Margin - st.Overall_Segment_Margin, 2) AS Discount_Impact
FROM segment_discount sd
JOIN segment_total st ON sd.Segment = st.Segment
ORDER BY sd.Segment, sd.Avg_Margin DESC;

-- ============================================
-- Query 3: Monthly Profit by Segment
-- Purpose: Track monthly revenue and profit 
-- trends broken down by customer segment
-- ============================================

WITH monthly_segment AS (
    SELECT 
        Year,
        "Month Name",
        "Month Number",
        Segment,
        ROUND(SUM(Sales), 2) AS Revenue,
        ROUND(SUM(Profit), 2) AS Profit,
        ROUND(AVG("Profit Margin %") * 100, 2) AS Avg_Margin
    FROM financial
    WHERE Segment IS NOT NULL
    GROUP BY Year, "Month Number", "Month Name", Segment
)
SELECT 
    Year,
    "Month Name",
    Segment,
    Revenue,
    Profit,
    Avg_Margin
FROM monthly_segment
ORDER BY Year, "Month Number", Segment;
