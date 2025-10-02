# How to Build an Empirical Speed Distribution for Dark Matter in the Solar Neighborhood
## Work published as *[link]* by Tal Shpigel, Dylan Folsom, Lina Necib, Mark Vogelsberger, Lars Hernquist, and Mariangela Lisanti.

For questions, please contact talshpigel at princeton dot edu.

---

### Abstract:

The dark matter flux in a direct detection experiment depends on its local speed distribution. This distribution
has been inferred from simulations of Milky Way–like galaxies, but such models serve only as proxies given that
no simulation directly captures the detailed evolution of our own Galaxy. This motivates alternative approaches
which obtain this distribution directly from data. In this work, we utilize 98 Milky Way analogues from the
IllustrisTNG50 simulation to develop and validate a procedure for inferring the dark matter speed distribution
using the kinematics of nearby stars. We find that the dark matter that originated from old mergers, plus that
from recent non-luminous accretions, is well described by a Maxwell–Boltzmann speed distribution centered at
the local standard-of-rest velocity. Meanwhile, recently-accreted dark matter from massive mergers has speeds
that can be traced from the associated stellar debris of these events. The stellar populations systematically
underestimate the velocity dispersion of their dark matter counterparts, but a simple kinematic boost brings the
two into good alignment. Using the TNG50 host galaxies, we demonstrate that combining these two contributions
provides an accurate reconstruction of the local DM speeds. As an application of the procedure to our own
Galaxy, we utilize stellar kinematic data from Gaia to quantify how the dark matter remnants from the Milky
Way’s last major merger impact its speed distribution in the Solar neighborhood.
---

### Datasets:

We provide four CSV files containing the normalized speed distributions in both the Galactocentric and Geocentric frames. All distributions are defined on a common velocity grid of 0–850 km/s with 1001 points. Each column is a single $f(v)$ curve.

#### 1. `total_galactocentric.csv`  
#### 2. `total_geocentric.csv`  

- Contain the **full reconstructed dark matter speed distributions** (“green band” in the paper).  
- **Structure:**
  - Column `v`: velocity grid in km/s.  
  - Columns `f_000`, `f_001`, … : reconstructed $f(v)$ curves.  
  - First two rows encode the reconstruction parameters for each column:  
    - Row 1: Δσ (stellar boost factor).  
    - Row 2: $w_{\rm tr}$ (tracer weight).  

#### 3. `stars_galactocentric.csv`  
#### 4. `stars_geocentric.csv`  

- Contain the **stellar speed distributions**, with and without boosting.  
- **Structure:**
  - Column `v`: velocity grid in km/s.  
  - Column `f_000`: unboosted stellar distribution.  
  - Columns `f_001`, `f_002`, … : boosted stellar distributions.  
  - First two rows encode parameters for each column:  
    - Row 1: Δσ (stellar boost factor; `0` for unboosted).  
    - Row 2: $w_{\rm tr}$ (blank for unboosted).  

---

### Example Script

We provide [`example.ipynb`](example.ipynb), a Jupyter notebook that demonstrates how to:  
1. Load the CSV files into Python.  
2. Access the reconstruction parameters (Δσ, $w_{\rm tr}$).  
3. Plot the families of $f(v)$ curves and their statistical envelopes.  

Minimal usage looks like:

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("total_geocentric.csv", header=2)  # header at 3rd row
v = df["v"].to_numpy(float)
f0 = df["f_000"].to_numpy(float)

plt.plot(v, f0)
plt.xlabel("v [km/s]")
plt.ylabel("f(v)")
plt.show()
```

See [`example.ipynb`](example.ipynb) for full plots of both totals and stellar distributions.  

---

### Notes

- All PDFs are normalized such that ∫ f(v) dv = 1.  
- The geocentric frame includes the Sun’s peculiar velocity and the Earth’s orbital motion.  
- The velocity grid and construction method match those used in *[paper link]*.  

---

### License:

- **Data files:** [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/)  
