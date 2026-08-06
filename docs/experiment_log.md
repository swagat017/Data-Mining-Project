| Component | Paper | Our reproduction | Match/deviation explanation |
|---|---|---|---|
| Unit of analysis | Postcode | CustomerID | Postcode conflates multiple customers; CustomerID is a cleaner, standard unit for this dataset and is what the UCI page itself recommends |
| Eligible customers | 4,381 postcodes | 3,920 customers | Different unit + our explicit UK/known-customer/positive-qty-price/dedup rules |
| Recency unit | Months | Days | Days chosen for finer granularity; convertible to months if needed for direct comparison |
| Monetary range | [3.75, 88,125.38] | [3.75, 259,657.xx] | Scale differs due to unit-of-analysis difference above; shape (right-skew) is consistent |