# Delivery Routes with DBSCAN

Clustering a day's delivery addresses into routes with DBSCAN, and comparing the result with K-means.

## Data

`livraisons_jour.csv`: 356 delivery addresses with 5 columns.

| Column | Description |
|---|---|
| `adresse_id` | Address ID |
| `est_ouest_km` | East–west position (km) |
| `nord_sud_km` | North–south position (km) |
| `nb_colis` | Number of parcels |
| `poids_kg` | Total weight (kg) |

## Method

1. **Visual exploration**: the scatter plot shows 3 delivery axes and a few isolated addresses.
2. **K-means baseline**: with k = 3 or 4, K-means cuts the long axes and forces isolated addresses into clusters.
3. **Tuning DBSCAN**
   - `minPts = 5` (2 × number of features, plus one for noise)
   - ε read on the k-distance curve (elbow ≈ 0.3 km)
   - ε sweep from 0.20 to 0.80: stable plateau of 3 clusters from 0.45 → **ε = 0.55**
4. **Feature choice**: test with `poids_kg` added, with and without standardization.

## Results

| Setup | Clusters | Noise |
|---|---|---|
| Positions only, ε = 0.55 | **3** | **8** |
| Positions + weight | 1 | 351 |
| Positions + weight, standardized | 2 | 17 |

With positions only, DBSCAN finds the 3 delivery routes (92, 103 and 153 addresses) and isolates 8 addresses.

## Key takeaways

- DBSCAN follows dense areas of any shape and labels isolated points as noise, while K-means builds compact, round groups around centers.
- The k-distance curve gives a first estimate of ε; the sweep confirms or corrects it.
- Both positions are in km, so they should **not** be standardized: distances are already real distances.
- Parcel weight has no place in a distance used to build routes: a route depends on where addresses are, not on what is delivered.

## Tools

Python, pandas, NumPy, scikit-learn, Matplotlib
