<!--
Draft for the SynthCrop GitHub organization profile.

Move this file to `SynthCrop/.github/profile/README.md` (a separate `.github`
repository inside the SynthCrop organization) so GitHub renders it on
https://github.com/SynthCrop. It is not used from within synth-crop-web.
-->

# SynthCrop

> Synthetic data + remote sensing for tropical crop classification.

We build open tooling for **per-pixel crop classification of tropical
agriculture** using Sentinel-2 imagery and government land-use vectors, with a
focus on **synthetic-data augmentation for minority crops**. Initial area of
interest: Rayong Province, Thailand.

---

## What we work on

- **Earth observation** — Sentinel-2 L2A composites windowed to a Rayong AOI
  (10 m grid, EPSG:32647). Multi-year (2018, 2020, 2024).
- **Reference labels** — Thailand Land Development Department (LDD) landuse
  shapefiles, mapped to 15 classes (14 target crops + reservoirs / other).
- **Generative augmentation** — TabSyn (latent diffusion over a VAE), SMOTE,
  and VAE + Bayesian Gaussian Mixture, applied **only to the train split** to
  lift heavily-underrepresented minority crops (Langsat, Longan, Rambutan,
  Mangosteen, Coconut, Mango).
- **Classifier** — 2-stage Random Forest cascade: a 15-class generalist plus a
  tropical-orchard specialist that re-classifies the orchard prediction band.
- **Web delivery** — Streamlit dashboards that read pre-baked artifacts only,
  so the served app stays light while training stays in notebooks.

---

## Featured project

### [synth-crop-web](https://github.com/SynthCrop/synth-crop-web)

End-to-end Streamlit app for the Rayong AOI:

- **Dataset** — class distribution (source vs sampled vs synth), per-feature
  statistics, real-vs-synth correlation.
- **Temporal Change** — LDD landuse polygons across years over a folium
  basemap, colored by broad category.
- **Segmentation** — Sentinel-2 imagery with the RF cascade prediction
  overlaid; click any parcel to compare the model's per-pixel predictions to
  the ground-truth label.
- **Synth Lab** — TabSyn / SMOTE quality probes: class lift, marginal
  distributions, PCA projection, per-feature 1-Wasserstein distance,
  correlation drift.
- **Model Card** — variant selector for baseline / SMOTE-augmented /
  TabSyn-augmented cascades, with a side-by-side metrics + per-class F1
  comparison.

---

## Pipeline

```
Sentinel-2 SAFE.zip ──┐
LDD landuse shapefile ─┼─► preprocessing & feature extraction
                                     │  (NDVI, EVI, NDWI, MTCI, SWIR — Oct / Nov / Dec)
                                     ▼
                       2-stage Random Forest cascade
                          ▲                ▲
                          │                │
            real samples ─┘   minority augmentation (TabSyn / SMOTE / VAE+BGM)
                                     │
                                     ▼
                       per-pixel + per-parcel predictions
                                     │
                                     ▼
                           deploy/artifacts/  ──►  Streamlit web demo
```

---

## Class palette

15 dense classes (LDD-sorted) — `lib/palette.py` in `synth-crop-web`:

| idx | LDD code | name | | idx | LDD code | name |
|---|---|---|---|---|---|---|
| 0 | 2101 | Rice         | | 8  | 2407 | Mango       |
| 1 | 2204 | Cassava      | | 9  | 2413 | Longan      |
| 2 | 2205 | Pineapple    | | 10 | 2416 | Jackfruit   |
| 3 | 2302 | Para rubber  | | 11 | 2419 | Mangosteen  |
| 4 | 2303 | Oil palm     | | 12 | 2420 | Langsat     |
| 5 | 2403 | Durian       | | 13 | 4201 | Reservoir   |
| 6 | 2404 | Rambutan     | | 14 | 9999 | Others      |
| 7 | 2405 | Coconut      | |    |      |             |

---

## Stack

`Streamlit` · `scikit-learn` · `imbalanced-learn` · `TabSyn` · `Sentinel-2 L2A`
· `rasterio` · `geopandas` · `shapely` · `pyogrio` · `folium` · `plotly`
· `pyproj`

---

## Repositories (current)

| Repo | What |
|---|---|
| [`synth-crop-web`](https://github.com/SynthCrop/synth-crop-web) | Streamlit dashboards + artifact builders + RF / SMOTE / TabSyn notebooks |

> More repositories will be added as the organization grows. To follow new
> work, use **Watch → Custom → Releases** on the project page above.

---

## Contributing

Issues and pull requests are welcome on individual project repositories.
For dataset and modeling questions, please open a discussion on the relevant
repo rather than emailing maintainers directly so the answer benefits the next
person who has the same question.

---

## Acknowledgements

- Sentinel-2 imagery © European Space Agency / Copernicus.
- Land-use polygons from Thailand's Land Development Department.
- Synthetic-data baselines built on top of public TabSyn and
  imbalanced-learn implementations.
