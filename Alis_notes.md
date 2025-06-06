# QP Code Notes

## 0. Simulation Output

* **Weights are corrected**

  * Season correction
  * *Normalized to injected*

    * For example, if doing $-2 < \log(m_p) < 1$, $-2 < \log(a) < 1$
    * Simulations must integrate to normalize:

      ```
      ∫∫ dlog(m_p) dlog(a) = Event
      ```
* **Compute whether in season**
* **Compute insolation**

## 1. Filter Simulation Output

* **Two dataframes needed:**

  * All injected events
  * All detected events
* **Common cuts:**

  * In season
* **For detected events:**

  * `flake Δχ² = χ²_PSPL - χ²_flat > 500`
  * `binary Δχ² = χ²_PSPL - χ²_binary > 160`

## 2. Set-Up

* **Grid over mass & semimajor axis**

  * (≈ 20–30 bins each)
  * $-2 < \log(m_p) < 1$
  * $-2 < \log(a) < 1$
* **Compute total # of events**

  * `N_events = Σ_det_weights [Δχ²_flake > 500] ≈ 40,000?`
* **Compute detection efficiency**

  * 2D histogram of (m, a) grid
  * ```
    det_eff = N_det / N_dat = (Σ_det_weights) / (Σ_dat_weights)
    ```

---

## QP Code (cont.)

### Survey Sensitivity

* **# of planets detected if 1 per star exists**

  * $S = \rm{det_{eff}} \times N_{\rm{events}}$  *(Suzuki 2016)*

---

## 3. Fitting

### Maximum Likelihood Estimate (MLE) + MCMC

#### For MLE:

* Need function for occurrence rates (ultimately want modified Cassan from Penny 2019)
* *Currently fitting:*

  * ```
    d²N / dlog(m_p) dlog(a) = C * (m_p / M_break)^n, for m_p > M_break
    ```
* To change injected rates from log-uniform, need to weight by individual occurrence rate (evaluated at $m_{p,i}$ for event)

#### Maximum Likelihood

* Eqn 6 from Suzuki et al. 2016
* Need to compute N\_exp and S(m, a)
* Need sample of planets to fit, draw Poisson realization from detected events

  * $λ = N_\text{det} = Σ \text{det weights}$
* Minimize negative log L, varying occurrence rate params (C, M\_break, n, m)

  *     N_exp(C, M_break, n, m) = ∫∫ S * f dmdα
  *  \#versions: $N_\text{exp} = N_\text{det}$, $N_\text{exp} = Σ S Δm Δa$, $N_\text{exp} = ∫∫ S f dmdα$

---

## QP Code (cont.)

### Maximum Likelihood (cont.)

* MLE gives candidate parameters

#### For MCMC:

* Map out posteriors of candidate params from MLE

  * Using uniform priors right now (could use Kepler posteriors, e.g., from Bryson)
* Gives posteriors of occurrence rate params: (C, M\_break, n, m)

```
[Earthlike (box in grid sketch)]
```

* insolation
* now median flux

```
η⊕ = ∫∫ d²N / dmdS
```

---

## Things To Do First

* Decide on m, a limits
* Build new observatory, weather, etc. files
* Test run of notebook on existing data
* (Optional) Parse extra axes

---
