# How to Build an Empirical Speed Distribution for Dark Matter in the Solar Neighborhood
**Work published as *[link]* by Tal Shpigel, Dylan Folsom, Lina Necib, Mark Vogelsberger, Lars Hernquist, and Mariangela Lisanti.**

For questions, please contact talshpigel at princeton dot edu.

## Abstract

> The dark matter flux in a direct detection experiment depends on its local speed distribution. This distribution
> has been inferred from simulations of Milky Way&ndash;like galaxies, but such models serve only as proxies given that
> no simulation directly captures the detailed evolution of our own Galaxy. This motivates alternative approaches
> which obtain this distribution directly from data. In this work, we utilize 98 Milky Way analogues from the
> IllustrisTNG50 simulation to develop and validate a procedure for inferring the dark matter speed distribution
> using the kinematics of nearby stars. We find that the dark matter that originated from old mergers, plus that
> from recent non-luminous accretions, is well described by a Maxwell&ndash;Boltzmann speed distribution centered at
> the local standard-of-rest velocity. Meanwhile, recently-accreted dark matter from massive mergers has speeds
> that can be traced from the associated stellar debris of these events. The stellar populations systematically
> underestimate the velocity dispersion of their dark matter counterparts, but a simple kinematic boost brings the
> two into good alignment. Using the TNG50 host galaxies, we demonstrate that combining these two contributions
> provides an accurate reconstruction of the local DM speeds. As an application of the procedure to our own
> Galaxy, we utilize stellar kinematic data from Gaia to quantify how the dark matter remnants from the Milky
> Way's last major merger impact its speed distribution in the Solar neighborhood.


## Datasets

The data provided here correspond directly to **Figure 8** of the paper, which presents the reconstruction of the
local dark matter speed distribution in the Milky Way. The reconstruction is the sum of a Maxwell&ndash;Boltzmann
distribution and a boosted stellar speed distribution. The full sum is given in the `total` files, while the stellar
component alone is given in the `stars` files. These are provided in both the galactocentric and geocentric frames. 

### `total_galactocentric.csv` and `total_geocentric.csv`
- contain the reconstructed total dark matter speed distributions in the galactocentric and geocentric frames, respectively.  
- The first column gives the speeds at which the distributions are evaluated, and each subsequent column correponds to a speed distribution
- The first row is a header, giving labels to the columns, which are:
  - `v` — velocity grid in km/s (from 0 to 850).  
  - `f_*` — reconstructed speed distributions for each sampled ($w_\mathrm{tr}$, $\Delta\sigma$).

### `stars_galactocentric.csv` and `stars_geocentric.csv`  
- contain the corresponding stellar speed distributions used in each reconstruction.  
- There are an additional two rows to these files, which record
  - Row 1: $\Delta\sigma$ values (km/s) describing the dispersion boost applied to stellar tracers, taken from the GSE-like mergers in TNG.  
  - Row 2: $w_\mathrm{tr}$ values indicating the DM fraction traced by each stellar population, taken from the single&ndash;traceable merger halos in TNG.  
- This is followed by a header, giving labels to the columns, which are:
  - `v` — velocity grid in km/s (from 0 to 850).  
  - `f_000` — unboosted stellar distribution.  
  - `f_001, f_002, ...` — boosted stellar distributions aligned with the total reconstructions (i.e., the `f_001` column in this file corresponds to the stellar speed distribution used in the first total reconstruction, and so on). 

Together, these datasets provide both the total reconstructed dark matter distributions and the stellar tracers used
to build them.

---

## Example Script

We provide [`example.ipynb`](example.ipynb), a Jupyter notebook that demonstrates how to:  
1. Load the CSV files into Python.  
2. Access the reconstruction parameters ($\Delta\sigma$, $w_\mathrm{tr}$).  
3. Plot the families of $f(v)$ curves and their statistical envelopes.  

Minimal usage looks like:

```python
import pandas as pd
import matplotlib.pyplot as plt

# For TOTALS (single header row)
df = pd.read_csv("total_geocentric.csv")
v = df["v"].to_numpy(float)
f0 = df["f_000"].to_numpy(float)

plt.plot(v, f0)
plt.xlabel("v [km/s]")
plt.ylabel("f(v)")
plt.show()
```

See [`example.ipynb`](example.ipynb) for full plots of both totals and stellar distributions.  

---

## Notes

- All PDFs are normalized to unity.  
- All velocity grids are sampled from 0&ndash;850 km/s for consistency between galactocentric and geocentric frames.  
- Naming convention:  
  - `f_000` is reserved for the unboosted stellar distribution.  
  - `f_001+` correspond to boosted stellar curves aligned with reconstructions.  
  - The names of the columns in the `stars` files indicate which columns of the `total` files they are used in.  
- The geocentric transformation assumes the following velocity values for $(v_r, v_\phi, v_\theta)$, in keeping with [Baxter et al. (2021)](https://arxiv.org/abs/2105.00599):  
  - Local Standard of Rest: $v_\mathrm{LSR} = (0, 238, 0)$ km/s  
  - Peculiar solar motion: $v_\mathrm{pec} = (11.1, 12.24, 7.25)$ km/s  
  - Sun's velocity relative to the MW: $v_\odot = v_\mathrm{LSR} + v_\mathrm{pec}$  
  - Earth's velocity relative to the Sun: $v_\oplus = (29.2, -0.1, 5.9)$ km/s  
- The Maxwell&ndash;Boltzmann used to construct the `total` curves has characteristic speed $v_0 = v_\mathrm{LSR} = 238$ km/s

## License
These data are licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/). These data are produced from the results of the `TNG50` simulation, which is publicly available at https://tng-project.org.
