## 💧 MoDOBR: Model for Optimized Design of Infiltration Basins (LID)

A fast, spreadsheet-friendly framework to **size infiltration basins and similar LID practices** by jointly optimizing **surface area** and **design height**, while accounting for **dynamic infiltration (Green–Ampt)**, **pre/post-urbanization hydrographs**, **underdrains**, and **climate-change freeboard**—with transparent diagnostics and open code.

![](https://img.shields.io/github/issues/marcusnobrega-eng/MoDOBR)
![](https://img.shields.io/github/forks/marcusnobrega-eng/MoDOBR)
![](https://img.shields.io/github/last-commit/marcusnobrega-eng/MoDOBR)

## 📘 Project Summary

**MoDOBR** is an open-source design and analysis toolbox that replaces ad-hoc height assumptions with a **coupled, optimization-based** sizing of infiltration LID (retention basins, rain gardens, permeable pavements). It links a **rational-method inflow** to a **time-varying infiltration capacity** and solves for the **unique, feasible design height** that satisfies storage, drainage-time, and safety constraints.

The method is documented in the preprint **“A Simple Method for Designing Infiltration Low Impact Development Techniques Considering Effects of Urbanization and Climate Change.”**

## 🔍 Model Capabilities

| Feature | Description |
| --- | --- |
| 🧮 Inflow hydrograph | Triangular hydrograph from the **Rational Method** with pre- vs post-urbanization $C,t_c$ and **Sherman IDF** parameters $K,a,b,c$ |
| 🌊 Dynamic infiltration | **Green–Ampt** capacity $$f(t)=\min\\big(C(t),h_p/\Delta t\big)$$ with evolving ponding depth $h_p$ and cumulative infiltration $F(t)$ |
| 🧩 Height–area coupling | Optimization of $(A,h)$: matches **minimum detention volume** from urbanization impact and enforces **max ponding $\le h$** and **drain-down $\le 24\text{h}$** |
| 🛠️ Drains & spillways | Optional **underdrains (orifices)** and **freeboard + weir** sizing; supports climate-change **intensity scaling** $\gamma\ \in\ [1.2,1.4]$ |
| 🧪 Sensitivity | Local sensitivity for $k_{\text{sat}},\Delta\theta,\psi$ on peak infiltration, max depth, and emptying time |
| 📊 Diagnostics | Time series for $Q(t),f(t),h_p(t),F(t)$ and storage; feasibility flags and constraint checks |
| 🧰 Implementation | **Excel/LibreOffice** (GRG solver) + scripts; simple inputs from site and soil data |

## 🌍 What Problems It Solves

- Eliminates **over/undersizing** from arbitrary height picks by enforcing **physics + constraints**.  
- Provides **climate-resilient** designs via explicit **freeboard** and overflow checks under intensified rainfall.  
- Offers a **transparent, reproducible** path for consultants and reviewers using familiar spreadsheet tools.

## 🧠 Method Highlights

- **Minimum required volume** from pre–post hydrograph difference is guaranteed even under **full clogging**.  
- A **unique compatible height** exists within user bounds; solved efficiently with **GRG** in Excel.  
- **Constant-rate shortcuts** ($f = k_{\text{sat}}$) tend to **overestimate** infiltration and **oversize** designs; **dynamic Green–Ampt** is recommended.

## 🚀 Getting Started

1. **Collect inputs:** IDF parameters $(K,a,b,c)$, pre/post $C$ and $t_c$, soil $k_{\text{sat}},\Delta \theta,\psi$, initial $F(0)$, event duration.  
2. **Choose constraints:** detention limit (e.g., **$24\,\text{h}$**), height bounds $[h_{\min},\,h_{\max}]$, optional **$\gamma$** for climate scaling.  
3. **Run MoDOBR:** compute pre/post hydrographs, solve optimization for $(A,\,h)$, check diagnostics.  
4. (Optional) **Add drains** or **spillway** to meet fixed-height site constraints and climate-adjusted freeboard.

## 📚 Documentation & Paper

- **Article:** *A Simple Method for Designing Infiltration Low Impact Development Techniques Considering Effects of Urbanization and Climate Change*.  
- Includes derivations for: **rational inflow**, **Green–Ampt dynamics**, **height–area optimization**, **freeboard under $\gamma$**, **drain/weir formulations**, and **sensitivity analyses**. In press.

## 🧪 Example Use Cases (from the paper)

- **Typical design:** 5-yr, 10-min storm over mixed land use; MoDOBR finds $h \approx 0.36\,\text{m}$ and $A \approx 2.9\%$ of catchment (sandy soil), avoiding overflow under dynamic infiltration.  
- **Soil contrast:** Feasible designs for **sandy $\rightarrow$ clayey** soils via optimized heights; **underdrains** unlock solutions for low saturated hydraulic conductivity cases.  
- **Fixed-height retrofit:** For imposed $h=0.8\,\text{m}$, a **set of underdrains** restores compliance; **freeboard** sized for $\gamma=1.2$.

## 🧩 Inputs & Outputs

**Inputs:** $K,a,b,c,C_{\text{pre}},C_{\text{post}},t_{c,\text{pre}},t_{c,\text{post}},k_{\text{sat}},\Delta\theta,\psi,\,F(0),\Delta t,t_f,\gamma$  
**Core Outputs:** $(A,h)$ (optimal), **max ponding**, **emptying time**, $f(t),F(t),h_p(t)$, storage time series, feasibility indicators.

| Variable | Description | Units / Dimensions |
|---|---|---|
| $K$ | Sherman IDF scale parameter (with $i = K\,(t+a)^{-b} + c$) | $(\text{mm h}^{-1})\cdot \text{min}^b$ (if $t$ in min, $i$ in mm/h) |
| $a$ | Sherman IDF offset parameter | min |
| $b$ | Sherman IDF exponent | – |
| $c$ | Sherman IDF additive term (baseline intensity) | - |
| $C_{\text{pre}}$ | Runoff coefficient (pre-urbanization) | – |
| $C_{\text{post}}$ | Runoff coefficient (post-urbanization) | – |
| $t_{c,\text{pre}}$ | Time of concentration (pre-urbanization) | min |
| $t_{c,\text{post}}$ | Time of concentration (post-urbanization) | min |
| $\gamma$ | Climate intensity scaling factor (e.g., 1.2–1.4) | – |
| $k_{\text{sat}}$ | Saturated hydraulic conductivity | mm/h |
| $\Delta\theta$ | Effective porosity difference ($\theta_s-\theta_i$) | – |
| $\psi$ | Wetting-front suction head (Green–Ampt) | mm |
| $F(0)$ | Initial cumulative infiltration | mm |
| $\Delta t$ | Computational time step | min |
| $t_f$ | Storm/event duration | min |
| $A$ | LID plan area (optimization variable) | m² |
| $h$ | Design ponding height (optimization variable) | m |
| $h_{\min},\,h_{\max}$ | Lower/upper bounds for design height | m |
| $Q(t)$ | Inflow hydrograph discharge to the LID | m³/s |
| $i(t)$ | Rainfall intensity time series (from IDF) | mm/h |
| $f(t)$ | Infiltration capacity (Green–Ampt) | mm/h |
| $F(t)$ | Cumulative infiltration | mm |
| $h_p(t)$ | Ponding depth inside the LID | mm |
| $S(t)$ | Storage volume within the LID | m³ |
| $t_d$ | Drain-down/emptying time (constraint) | h |
| $\text{FB}$ | Freeboard (additional allowable depth above $h$) | m |
| $Q_{\text{out}}(t)$ | Total outflow (underdrain + weir/spillway) | m³/s |
| $C_d$ | Orifice discharge coefficient (underdrain) | – |
| $A_o$ | Orifice/underdrain flow area | m² |
| $z_w$ | Weir crest elevation relative to basin bottom | m |
| $b_w$ | Weir width (rectangular weir) | m |
| $C_w$ | Weir coefficient | – |
| $g$ | Gravitational acceleration | m/s² |


## 👥 Authors & Contact

**Marcus N. Gomes Jr.**, **César A. F. do Lago**, **Eduardo M. Mendiondo**  
Questions, issues, or collaboration ideas? Please open a GitHub issue on the project repository.

## 👤 Developer
**Marcus N. Gomes Jr.**  
Postdoctoral Researcher II, University of Arizona  
📧 Email: [marcusnobrega.engcivil@gmail.com](mailto:marcusnobrega.engcivil@gmail.com)  
🌐 Website: [marcusnobrega-eng.github.io/profile](https://marcusnobrega-eng.github.io/profile)  
📄 CV: [Download PDF](https://marcusnobrega-eng.github.io/profile/files/CV___Marcus_N__Gomes_Jr_.pdf)  
🧪 ORCID: [0000-0002-8250-8195](https://orcid.org/0000-0002-8250-8195)  
🐙 GitHub: [@marcusnobrega-eng](https://github.com/marcusnobrega-eng)

