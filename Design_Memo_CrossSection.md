# Design Memo — Intergenerational Background and the Incidence of the COVID-19 Crisis in Peru
## Repeated Cross-Section Approach using ENAHO 2018–2023

**Status:** Pre-analysis design memo (replacing earlier panel-based draft).
**Target journals:** *Journal of Development Economics*, *World Development*, *Review of Income and Wealth*, *Journal of Economic Inequality*.
**Authors:** Janice, Javier, Alexandra (roles per original outline).

---

## 1. Research Question

**Did the intergenerational gradient in household poverty widen during the COVID-19 economic crisis in Peru?** Specifically, was the incidence of monetary poverty during 2020–2021 disproportionately concentrated among households whose heads come from less-educated parental backgrounds, relative to the pre-crisis (2018–2019) and recovery (2022–2023) periods?

This is a **gradient-evolution question**, not a household-transition question. We do not track the same households over time; we estimate how the cross-sectional association between parental education and household poverty changes around the shock.

## 2. Motivation and Contribution

The literature on intergenerational mobility studies long-run transmission of education, income, and occupation (Solon 1999; Black & Devereux 2011; Torche 2015; Azevedo & Bouillon 2010). A small recent literature has begun to ask whether *macro shocks* differentially affect households along the intergenerational SES gradient:

- **Bailey et al. (2024)** — Great Depression, US: heterogeneous effects by gender/age in occupational and educational mobility.
- **Binder & Woodruff (2002)** — 1980s Mexico crisis: educational mobility stagnated for the affected cohort.
- **Jiang, Qi & Lin (2022)** — COVID, China: pandemic strengthened the parent-child income correlation; effects concentrated in poorer households.
- **Neidhöfer, Lustig & Tommasi (2021)** — COVID, Latin America: counterfactual projection that school closures will lower future educational mobility.

**Our contribution.** We provide direct, contemporaneous evidence on the heterogeneous *incidence* of a major macro shock along the intergenerational education gradient in a Latin American developing economy. Unlike Neidhöfer et al. (counterfactual projection on future mobility) or Jiang et al. (income correlation, panel), we use repeated cross-sections to estimate an event-study of the parental-education gradient in realized poverty across six consecutive years bracketing the shock. The design is transparent: parental education is a fixed individual characteristic, so its interaction with year identifies how the shock priced parental background.

Peru is a particularly informative case: one of the world's highest COVID mortality rates per capita, an 11% GDP contraction in 2020, and an employment collapse of 35% in Q2 2020 (one of the sharpest in Latin America). High informality (~70% of the labor force) makes the social-protection cushion thin and predicts large heterogeneity in the incidence of the shock.

## 3. Data

- **Source:** Encuesta Nacional de Hogares (ENAHO), INEI Peru. Six annual cross-sections, 2018–2023.
- **Sample size:** ~30,000–37,000 households per year (varies; the annual ENAHO has ~120,000 individuals).
- **Key variables:**
  - Official monetary poverty status (binary): consumption below the INEI poverty line.
  - Extreme poverty (binary): consumption below the extreme poverty line.
  - Real per-capita consumption (continuous).
  - Parental education: highest level attained by the household head's father and mother (categorical: none/early childhood, primary, secondary, tertiary non-university, tertiary university). Imputed to years using ENAHO conversion.
  - Household head's own education (`p301a`, `p301b`).
  - Demographics: head's age, sex; household size and composition.
  - Geography: department, urban/rural.
  - Employment: head's sector (formal/informal), industry.

## 4. Sample

- Household heads aged **25–65** at the time of survey (to ensure overlapping cohorts across years and avoid mechanical changes in parental availability with age).
- At least one parent's education observed.
- Drop collective households (institutions, dormitories).
- Apply ENAHO sampling weights throughout.

Robustness samples: 30–55 (narrower cohort); separate male and female heads; excluding the year of the head's internal migration.

## 5. Outcome Variables

**Primary:** Binary monetary poverty (INEI official, consumption-based).

**Secondary / robustness:**
- Extreme poverty (binary).
- Asset-based poverty index (PCA of durable goods, dwelling characteristics) — to bypass the 2021 INEI consumption-basket methodology revision.
- Vulnerable household (within 25% above the poverty line, binary).
- Log real per-capita consumption (continuous, for unconditional quantile regressions in robustness).

## 6. Treatment Variable: Parental Education

- **Main spec:** Maximum of father and mother education, in years, imputed from levels.
- **Alternatives reported:** Father only; mother only; both included separately; categorical (none / primary / secondary / tertiary).
- Note on imputation: the original ENAHO records only the categorical level for parents. We impute years using the population mean within each level (e.g., 6 years for "primary complete"). Robustness with the categorical specification only avoids imputation error.

## 7. Identification Strategy

### 7.1 Main specification — event study in repeated cross-sections

$$\Pr(\text{Poor}_{iht} = 1) = \alpha + \sum_{\tau \neq 2019} \gamma_\tau \mathbf{1}\{t=\tau\} + \sum_{\tau \neq 2019} \delta_\tau \big(\text{ParEduc}_i \times \mathbf{1}\{t=\tau\}\big) + X'_{ih} \theta + \varepsilon_{iht}$$

with 2019 as the omitted baseline. $\delta_\tau$ traces the parental-education gradient in poverty incidence over time.

- **Pre-period placebo:** $\delta_{2018}$ should be statistically indistinguishable from zero under parallel trends in the gradient.
- **Crisis effect:** $\delta_{2020}, \delta_{2021}$. The sign of interest is negative — meaning the gradient becomes *steeper* (more parental education → more protection from crisis-induced poverty).
- **Recovery:** $\delta_{2022}, \delta_{2023}$. Returns toward zero indicate transient shock; persistence indicates lasting amplification of intergenerational inequality.

### 7.2 Identifying assumption

**Parallel trends in the gradient**, not in levels. Specifically: absent the COVID shock, the relationship between parental education and poverty would have evolved smoothly (linearly or constantly) over 2018–2023. The 2018 coefficient is a direct (though weak — only one pre-year) test.

### 7.3 Threats and mitigations

| Threat | Mechanism | Mitigation |
|---|---|---|
| Sample composition | Year-to-year shifts in age/region distribution of heads correlate with parental education | Cohort restriction (25–65), region × year FE, balance tables across years |
| Reverse urban-rural migration during COVID | Heads with rural-educated parents may have re-located | Robustness with originally-urban subsample (if region of birth available); control for migration history |
| Selection in parental-education availability | Missing when parents deceased; correlates with head's age and SES | Report missingness by year; MICE imputation; Lee (2009) bounds |
| Poverty line discontinuity | INEI revised the consumption basket around 2021 | Asset-index poverty as robustness; expenditure-quartile poverty; show results separately for original vs. revised basket years |
| Own education as "bad control" | Endogenous mediator of parental education | Report both: with own education (residual effect) and without (total reduced-form effect) |
| Selection into headship | During crisis, household formation/dissolution may differ by parental SES | Robustness: predict pre-crisis-modeled headship and weight |
| Conglomerate-level confounders | Local COVID severity correlates with population SES | Department × year FE in main spec; explicit triple-diff in heterogeneity |

## 8. Heterogeneity

Pre-specified subgroups:
- Sex of household head (male / female)
- Region (Lima Metropolitana / urban Costa-Sierra-Selva / rural)
- Sector of head's main job (formal / informal)
- Head's own education (low: ≤ secondary; high: > secondary) — tests whether parental matters conditional on own
- Age cohort (25–40 / 41–65)

Triple-difference: interact $\delta_\tau$ with provincial COVID mortality (CONCYTEC data) — provinces with worse shocks should show stronger amplification of the gradient if parental background matters more when the shock is bigger.

## 9. Empirical Methods

- **Main:** Linear Probability Model with sampling weights, standard errors clustered at the primary sampling unit (conglomerate).
- **Robustness:** Probit and Logit with average marginal effects; results should align.
- **Visualization:** Event-study plots of $\delta_\tau$ with 95% CIs; binned scatterplots of poverty rate by parental education years, separately by year.
- **Software:** R (`fixest`, `marginaleffects`) and/or Stata (`reghdfe`, `margins`).

## 10. Causal Interpretation and Scope

**What we estimate:** The change in the cross-sectional association between parental education and household poverty during the COVID crisis, relative to the pre-shock baseline.

**What we do NOT claim:**
- That parental education *causes* lower poverty in levels. Assortative mating, wealth transfers, social networks, and geography are all bundled into the level association.
- That panel-style flows ("falling into poverty") behave the same way as cross-sectional stocks.
- That observed differences reflect within-household resilience — they may also reflect compositional shifts in who is observed.

**What we can claim** under parallel trends in the gradient:
- The crisis was *differentially incident* by parental background.
- The intergenerational SES gradient in living standards *widened/narrowed* during the shock.
- Mechanism candidates (informal-insurance networks, asset transfers, formal-sector access via parental connections) can be probed with heterogeneity, but not point-identified.

## 11. Pre-specified analysis vs. exploratory

**Pre-specified (confirmatory):** Sections 7, 8, 9 above. Event-study coefficients $\delta_\tau$ on max parental education, with controls listed, in the 25–65 cohort.

**Exploratory:** Mechanisms — informal labor markets, intra-family transfers (ENAHO Module 9), inter-household transfers, education investment in children during school closures.

## 12. Why Repeated Cross-Sections (not Panels)

- Larger and more nationally representative annual samples than the ENAHO 2- or 3-year panels.
- Extends to 2023, capturing the recovery; the ENAHO panel is typically released with longer lag and smaller sample.
- Avoids panel attrition, which is non-random and likely correlated with poverty transitions (the most affected households are most likely to drop out).
- Estimand is a population-level gradient, not a household-level transition probability — matches the policy question about *who* the crisis hit.
- Trade-off acknowledged: we cannot identify intra-household flows; results are about distributional incidence.

## 13. Working Title (candidates)

- "Parental Background and the Incidence of the COVID-19 Crisis: Evidence from Peru"
- "Did Parental Education Buffer Peruvian Households from the Pandemic Recession?"
- "The Intergenerational Gradient of a Crisis: COVID-19 and Household Poverty in Peru"

## 14. Open issues to resolve before submission

1. Verify ENAHO module containing parental education is collected identically in all years 2018–2023.
2. Confirm INEI poverty methodology revisions (years and magnitude) and design robustness accordingly.
3. Decide whether to include 2024 if released (one additional recovery year).
4. Decide pre-registration venue (OSF given non-RCT design).
