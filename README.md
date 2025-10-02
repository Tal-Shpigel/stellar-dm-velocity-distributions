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

The data provided here corresponds directly to **Figure 8** of the paper, which presents the reconstruction of the
local dark matter speed distribution in the Milky Way.

- **total_galactocentric.csv** and **total_geocentric.csv**  
  - Contain the reconstructed total dark matter speed distributions in the galactocentric and geocentric frames.  
  - Columns:  
    - `v` — velocity grid in km/s (0–850).  
    - `f_*` — reconstructed probability density curves for different parameter samples.  
  - These files do *not* contain parameter rows; they only include the velocity grid and the f(v) curves.

- **stars_galactocentric.csv** and **stars_geocentric.csv**  
  - Contain the corresponding stellar speed distributions used in the reconstruction.  
  - Structure:  
    - Two parameter rows at the top:  
      - Row 1: `delta_sigma` values (km/s) describing the dispersion boost applied to stellar tracers.  
      - Row 2: `w_tr` values indicating the DM fraction traced by each stellar population.  
    - Header row: `v, f_000, f_001, ...`  
    - Columns:  
      - `v` — velocity grid in km/s (0–850).  
      - `f_000` — unboosted stellar distribution.  
      - `f_001, f_002, ...` — boosted stellar distributions aligned with the total reconstructions.  

Together, these datasets provide both the total reconstructed dark matter distributions and the stellar tracers used
to build them.

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

- All PDFs are normalized such that $\int f(v)dv=1$.  
- All velocity grids are sampled from 0–850 km/s for consistency between galactocentric and geocentric frames.  
- Naming convention:  
  - `f_000` is reserved for the unboosted stellar distribution.  
  - `f_001+` correspond to boosted stellar curves aligned with reconstructions.  
  - In totals files, the column names match the reconstruction sample indices.  
- The geocentric transformation assumes the following velocity values (km/s):  
  - Local Standard of Rest: `v_LSR = (0, 238, 0)`  
  - Peculiar solar motion: `v_pec = (11.1, 12.24, 7.25)`  
  - Sun’s velocity: `v_sun = v_LSR + v_pec`  
  - Earth’s orbital motion: `v_earth = (29.2, -0.1, 5.9)`  

---

### License:

- **Data files:** [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/)  
