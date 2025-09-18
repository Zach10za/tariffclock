# Tariff Clock

I wanted a simple way to show how much U.S. businesses and consumers have paid, and are continuing to pay, in tariff taxes since the April 2025 tariff hikes.

## Data Pipeline
- The page hits the U.S. Treasury FiscalData API for the Monthly Treasury Statement (table 4) filtered to `Customs Duties` records after March 2025.
- Each monthly release is collapsed into one number, sorted oldest to newest, and cached in `localStorage` so repeat visits work even if Treasury is slow.

## Rate Calculation
- Monthly customs duty totals are converted into a cumulative series starting April 2025.
- A monotone cubic spline is fit across those points so the running total and its slope stay smooth and non-negative.
- The derivative of that spline provides the live “per day” rate that drives the counter between official releases.
