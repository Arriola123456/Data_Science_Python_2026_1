# Pre-Analysis Plan Review

**Study**: The Intergenerational Education Gradient in Poverty Incidence During the COVID-19 Crisis in Peru (working title: "Intergenerational Buffers / Parental Background and the Incidence of COVID-19")
**PI(s)/Team**: Janice (Introduction & Literature), Javier (Data & Methodology), Alexandra (Results & Conclusions)
**Date**: 2026-05-14
**Review Standard**: OSF — Open Science Framework Registry (observational, non-RCT). Likely submission journals: Journal of Development Economics, World Development, Review of Income and Wealth, Journal of Economic Inequality.

---

## File Inventory

**Main PAP file:**
- `C:\Users\manue\OneDrive\Escritorio\CIUP\15_Paper_Alexandra\Idea\Design_Memo_CrossSection.md`

**Supporting context (predecessor draft, superseded):**
- `C:\Users\manue\OneDrive\Escritorio\CIUP\15_Paper_Alexandra\Idea\Janice -  Movilidad Intergeneracional - Poverty - Crisis.docx` (read via python-docx; one paragraph affected by cp1252 encoding error — non-substantive limitation)

**Expected supporting documents NOT FOUND:**
- Power / MDE calculation worksheet
- Mock tables / mock figures (Table 1 shell, Figure 1 event-study shell)
- Code skeleton (.do / .R)
- Variable crosswalk / data dictionary mapping ENAHO variables to PAP concepts
- Auxiliary data documentation for provincial COVID mortality (SINADEF / CONCYTEC)
- IRB / ethics exemption letter
- Replication / data-sharing plan
- Bibliography / references file

---

## Overall Assessment

The PAP represents a **substantial and correct conceptual repositioning** from the original panel-based draft: it abandons the within-household "falling-into-poverty" framing (which a cross-section design cannot identify) and reframes the question as the evolution of the cross-sectional parental-education–poverty gradient around the COVID shock. The design is honest about its limitations (Section 10) and pre-specifies a reasonable set of outcomes, subgroups, and robustness checks.

**However**, the single most damaging weakness is identification: with 2019 as the omitted baseline, only ONE pre-period year (2018) remains to test parallel-trends-in-gradient. A single pre-period coefficient cannot distinguish parallelism from low power, and modern DiD literature (Roth 2022; Rambachan-Roth 2023) demands at least 3 pre-periods. Combined with (i) no formal power calculation, (ii) the 2021 INEI poverty-line methodology revision contaminating the recovery coefficients, (iii) endogenous composition of household-headship during the shock, and (iv) an unresolved ENAHO parental-education variable name, the PAP is **not yet registerable**.

**Preliminary Recommendation**: **Substantial revision required before registering**. The conceptual core is sound; the empirical scaffolding needs significant strengthening over ~4–6 weeks of work before posting to OSF.

---

## Priority Action Items

Ranking applies the triage hierarchy: identification & causal credibility (Agents 3, 6) > statistical plan, power & multiple testing (Agent 4) > internal inconsistencies & outcome coverage (Agent 2) > data, sample & implementation (Agent 5) > clarity & pre-specification completeness (Agent 1). Within each agent, Critical > Major > Minor.

### CRITICAL (must fix — these could invalidate the pre-registration or attract fatal referee criticism)

1. **Extend the pre-period to ≥3 years (2015–2017) or formally weaken causal language.** With 2019 as baseline and only 2018 as "pre-trends test", parallel-trends-in-gradient is not testable in any meaningful sense. The fix is to either (a) verify that the ENAHO parental-education question is comparable in 2015–2017 and extend the window, or (b) reposition the paper as descriptive cross-sectional evolution rather than event-study causal. (Agents 3, 6 — both flag as the #1 fatal risk.)

2. **Resolve composition endogeneity of household headship during COVID.** Reverse urban-rural migration, multigenerational consolidation, and differential COVID mortality changed *who is observed as head* between 2019 and 2021 in ways correlated with parental education. The proposed "re-weighting on pre-crisis-modeled headship" is not operationalized. Required fixes: (i) restrict to fixed birth cohorts observed in all years (e.g., born 1965–1990) instead of "aged 25–65 each year", (ii) report balance tables of head characteristics across years × tertiles of parental education, (iii) implement Lee or Horowitz-Manski bounds for selection on unobservables. (Agents 3, 6.)

3. **Pre-specify the analytic specification fully.** The control vector $X_{ih}$ in Section 7.1 is not enumerated; functional form of `ParEduc` is not committed (years vs. categorical); SE clustering ignores ENAHO's stratified two-stage design (needs Taylor linearization via `svy`/`survey`, not plain conglomerate clustering); imputation of years from levels uses a deterministic mean rather than multiple imputation. Fix: lock down the regression columns (col 1: no controls; col 2: demographics; col 3: demographics + region × year FE [primary]; col 4: + sector; col 5: + own education [reported, not primary]). Make categorical parental education the primary specification, not robustness. (Agents 1, 2, 4.)

4. **Compute a formal power / MDE calculation.** The PAP claims sample-size adequacy from ~30k–37k households/year but never computes the MDE for $\delta_\tau$ accounting for conglomerate clustering, design effect, and ICC. Sub-sample MDE (female × rural × informal) likely below policy-relevant magnitudes. OSF observational standard requires this. (Agents 4, 5, 6.)

5. **Address the 2021 INEI poverty-line methodology revision.** The official monetary-poverty indicator changes definition mid-sample. $\delta_{2022}, \delta_{2023}$ are mechanically not comparable to $\delta_{2019}$. Use INEI's back-cast harmonized series as the primary outcome; the asset-index PCA should be a co-primary check, not a robustness afterthought. Pre-commit a decision rule when the two series disagree. (Agents 3, 5, 6.)

6. **Identify the exact ENAHO parental-education variable, year by year.** The PAP never names the variable code (likely `p106` family or a parent-education roster in Module 100/300). Parental education is collected only for the household head in a different module than `p301a`/`p301b`. Before locking the PAP: (i) name the variable, (ii) verify identical wording across 2018–2023, (iii) tabulate non-missing rates per year, (iv) document any mode-of-interview change (telephone vs. in-person in 2020). (Agent 5.)

7. **Enumerate hypotheses, primary test, and decision rule.** The PAP poses a research question but never lists numbered hypotheses with predicted signs, a designated primary test statistic, an effect-size threshold that counts as confirmation, or a rule for interpreting a null. Without these, the PAP is not binding. Required: H1 (primary, directional): joint F-test that $\delta_{2020} = \delta_{2021} = 0$ is rejected with negative signs; H2 (recovery): $\delta_{2023}$ statistically closer to zero than $\delta_{2020}$; H3 (heterogeneity, directional): widening larger for informal-sector, female, rural heads. State explicit equivalence bound for the placebo $\delta_{2018}$. (Agents 1, 2, 6.)

8. **Implement multiple-testing correction.** Six event-time coefficients × 4 outcomes × 5 heterogeneity dimensions ≈ several hundred tests. Apply Romano-Wolf step-down (bootstrap) for the family of $\delta_\tau$ in the primary outcome; Anderson (2008) sharpened q-values for heterogeneity. Designate primary outcome (monetary poverty), secondary (extreme poverty + asset index), exploratory (vulnerability, log consumption). (Agents 1, 2, 4.)

9. **Replace deterministic mean-imputation of parental years with categorical-as-primary specification.** Imputing years from levels using population means introduces non-classical measurement error in the interaction variable that attenuates $\delta_\tau$ and varies across years if category composition shifts. Make the 5-level categorical specification (none / incomplete primary / complete primary / incomplete secondary / complete secondary / any tertiary, harmonized across years) the main spec. Continuous years moves to robustness. (Agents 2, 3, 4, 5.)

### MAJOR (should fix — these are likely to weaken the study's credibility or competitiveness)

10. **Decide what counts as a "positive null".** Pre-commit whether a tight-CI null on $\delta_{2020}$ is *evidence of resilience of the gradient* (publishable positive finding) or "no effect found" (uninformative). This decision must be made before seeing data. (Agent 6.)

11. **Re-specify the triple-difference with provincial COVID mortality.** Provincial mortality is endogenous to pre-existing poverty/informality/health-infrastructure conditions correlated with parental-education composition. Use a Bartik/shift-share exposure to pre-COVID contracting sectors (tourism, retail) instead of realized mortality, or instrument mortality with hospital capacity / latitude / altitude. Specify SINADEF (not CONCYTEC) as the data source, the geographic level (province, ~196 units), and the merge key. Use wild-cluster bootstrap SE given few clusters. (Agents 3, 5.)

12. **Sharpen the contribution claim vs. Jiang-Qi-Lin (2022) and Neidhöfer-Lustig-Tommasi (2021).** The defensible niche is: *realized outcome (not projection) × Latin America × extreme informality (70%) × extreme COVID mortality*. State explicitly what hypothesis on Peru is *not* tested by Jiang (China, formal economy) or Neidhöfer (LatAm, projection on schooling not poverty). The informality angle (channel: parental networks in informal labor markets) is the most defensible differentiator. (Agents 3, 6.)

13. **Handle the rotating-panel sub-sample of ENAHO.** ENAHO has ~20–25% household overlap between consecutive years. Standard errors clustered only at PSU understate uncertainty. Either (a) drop overlap households from non-base years or (b) two-way cluster at PSU and household ID. (Agent 5.)

14. **Treat own education as a "bad control" rigorously.** Specifications conditional on own education identify a residual that does not have a clean causal interpretation. Make the unconditional specification (total reduced-form effect of parental background) the primary; the conditional-on-own version is descriptive/exploratory, not causal. Consider Oster (2019) bounds for the total effect. (Agents 1, 3.)

15. **Add a deviations-from-PAP policy and resolve the "open issues" (Section 14) before posting.** OSF expects a clause committing the team to a "Deviations from PAP" subsection in the eventual paper. Open issues 1–3 (verify ENAHO module continuity; confirm INEI methodology revisions; decide on 2024 inclusion) must be resolved before the PAP is binding. (Agents 1, 2.)

16. **Build mock tables and a code skeleton before posting to OSF.** Run the full analysis on 2019 data only and produce the empty templates of Table 1, Table 2, Figure 1. This surfaces variable-coding issues the PAP currently hides. Deposit the code skeleton on OSF as part of registration. (Agent 5.)

### MINOR (polish — improves reviewer confidence and pre-specification quality)

17. Choose a single analysis software (R or Stata) for the confirmatory specification; the other for cross-check. Specify versions, seeds for MICE, and the OSF code repository link. (Agents 1, 4.)

18. Adopt simultaneous 95% confidence bands (sup-t, Montiel Olea-Plagborg-Møller 2019) for the event-study coefficient plot, not pointwise. (Agent 4.)

19. Replace the working titles using "Buffer" / "Did... widen" with a more descriptive title that matches Section 10's scope (e.g., "The Intergenerational Education Gradient in Poverty Incidence During the COVID-19 Crisis in Peru: A Repeated Cross-Section Event-Study"). (Agents 1, 3, 6.)

20. Add a CIUP IRB exemption note and an INEI data-use citation/redistribution statement. (Agent 5.)

21. Standardize terminology: "household head" (not "head"); single label for the estimator ("event-study with continuous treatment in repeated cross-sections"); a single, citation-anchored name for the "intergenerational education gradient" (not "SES gradient" — SES is broader than what is measured). (Agents 1, 2.)

22. Add a formal Ethics, Funding, and Conflict-of-Interest section. (Agent 5.)

23. Document the asset-index PCA construction completely (items, pooled-vs-yearly fitting, signed direction, threshold definition). (Agents 1, 4.)

24. Replace the Lee bounds proposal — which assumes monotonicity unlikely to hold under COVID-induced parental mortality — with Horowitz-Manski bounds. (Agent 4.)

---

## Adversarial Referee Review & Registration Recommendation

### Part 1 — Core Research Case

**Pregunta del PAP en una frase:** El PAP propone estimar, vía un *event study* en cross-secciones repetidas de la ENAHO 2018–2023, si el gradiente intergeneracional (educación parental → pobreza monetaria del hogar) se ensanchó durante el shock COVID-19 en Perú, identificando el efecto bajo "parallel trends in the gradient" con 2019 como año base.

**Evaluación:**
- *Importancia*: Sí. La pregunta es de primer orden para América Latina, complementa Jiang et al. (2022) con un caso de alta informalidad y mortalidad COVID extrema, y se diferencia conceptualmente de Neidhöfer et al. (2021) al medir incidencia realizada en lugar de proyecciones contrafactuales.
- *Identificación causal*: Marginalmente creíble pero frágil. Una sola observación pre-shock relativa a la base vuelve el "placebo de tendencias paralelas" prácticamente nominal. El lenguaje "differentially incident" está al borde de un *causal slip*.
- *Pre-especificación vinculante*: Razonablemente vinculante en outcome primario, muestra y especificación principal, pero deja zonas grises críticas (umbral de decisión, criterio de éxito, regla de elección entre LPM vs. probit, lista cerrada de controles, manejo de comparaciones múltiples).

**Rating: Borderline — substantial revision needed**

### Part 2 — Major Strengths

1. **[MAJOR]** Reposicionamiento conceptual honesto: renunciar al panel y declarar el estimando como gradiente poblacional, no transición intra-hogar.
2. **[MAJOR]** Outcome primario bien definido y verificable: pobreza monetaria binaria oficial INEI.
3. **[MAJOR]** Tabla de amenazas explícita (Sección 7.3), reconociendo discontinuidad de canasta INEI, bad-control de educación propia, selección en headship.
4. **[MINOR]** Heterogeneidad pre-especificada y disciplina de subgrupos (5 dimensiones + triple-diff).
5. **[MINOR]** Sección 10 "what we do NOT claim" — protege de overclaiming y es la disciplina que OSF y JDE quieren ver.

### Part 3 — Major Weaknesses

1. **[CRITICAL]** Un solo año pre-shock para tendencias paralelas. Es **la** debilidad fatal.
2. **[CRITICAL]** Composición del jefe de hogar endógena al shock — retorno rural, reconfiguración intergeneracional, mortalidad COVID — y el re-weighting propuesto no la resuelve.
3. **[MAJOR]** Imputación de años parentales con media de nivel introduce error de medida no-clásico en la variable de interacción.
4. **[MAJOR]** Indeterminación de la regla de decisión y del significado de un null.
5. **[MAJOR]** Power analysis ausente.
6. **[MINOR pero importante]** Contribución sobre Jiang et al. y Neidhöfer et al. débilmente argumentada.

### Part 4 — Required Revisions Before Registration

Ver lista CRITICAL/MAJOR consolidada arriba (items 1–16).

### Part 5 — Registration Fit and Strategic Positioning

**OSF es el registro correcto.** No hay alternativa razonable: AEA RCT Registry no aplica, AsPredicted es muy compacto. OSF Registries con template "Secondary Data Pre-registration". **Considerar Registered Report** si la revista lo acepta (RIW y JEI están abiertas; JDE menos) — compromete a la revista *antes* de ver resultados, lo cual maximiza el valor del PAP cuando el null es una posibilidad real.

**Ambición vs. lo que el diseño puede entregar:** Mismatch moderado. El PAP aspira a hablar de "amplificación de desigualdad intergeneracional" — afirmación distribucional fuerte — pero el diseño identifica un cambio en una asociación cross-sectional. Título recomendado: *"The Intergenerational Education Gradient in Poverty Incidence During the COVID-19 Crisis in Peru"*.

**Revista objetivo (orden recomendado):**
- **World Development** (primer envío). Acepta diseños descriptivos rigurosos con relevancia de política; el ángulo informalidad + Bono Yanapay encaja.
- **Review of Income and Wealth** (plan B). Si el énfasis se desplaza a descomposición distribucional.
- **Journal of Economic Inequality** (plan C). Fit temáticamente natural; menor selectividad relativa.
- **JDE solo si** se resuelven todas las debilidades CRITICAL — de lo contrario es desk reject.

**Posicionamiento estratégico:** Opción **(c)** — *América Latina como evidencia complementaria a Jiang-Qi-Lin (China) y Neidhöfer-Lustig-Tommasi (proyecciones)*. Posiciona a Perú como complemento empírico necesario: China midió correlación de ingreso parent-child con panel; Neidhöfer proyectó movilidad educativa futura; **este paper mide incidencia realizada de pobreza en una economía de altísima informalidad y mortalidad COVID extrema** — el caso polar respecto al chino.

### Part 6 — Adversarial Questions to the Research Team

1. **Identificación:** Con solo 2018 como pre-shock, ¿cómo distinguen "ensanchamiento causado por COVID" de una tendencia lineal pre-existente? ¿Pueden extender a ENAHO 2015–2017?
2. **Composición:** Durante 2020 hubo retorno masivo a hogares rurales. La población de "jefes 25–65 con padre observable" cambia. ¿Cómo aíslan el cambio en el gradiente del cambio en *quién es jefe*? Si la selección depende del shock mismo, el re-weighting no identifica el parámetro.
3. **Poder:** ¿MDE para $\delta_{2020}$? ¿Para subgrupos (rural, informal, mujer-jefe)? Si MDE > 2–3 p.p., un null no es informativo.
4. **Null como hallazgo positivo:** Si los $\delta_\tau$ son cero con CI estrecho, ¿es resiliencia inesperada (positivo) o no hallazgo (negativo)? Pre-comprometan *antes* de ver datos.
5. **Contribución sobre Jiang et al. (2022):** ¿Qué hipótesis sobre Perú — que no aplica a China — distingue su predicción? Sin esto, es "Jiang aplicado a Perú", publicable en JEI pero no en JDE/WD.
6. **Imputación:** La imputación de años usando media de nivel introduce error de medida no clásico. ¿Por qué no categórica como *main*? ¿Han testeado con simulación cuánto sesgo de atenuación introduce?
7. **LPM vs. probit (Ai & Norton 2003):** Para probit/logit la interacción no es interpretable directamente como coeficiente. ¿Reportarán AME of the interaction? ¿Regla pre-especificada si LPM y AME-probit divergen?
8. **Canasta INEI 2021:** La revisión metodológica de canasta coincide temporalmente con la recuperación. ¿Cómo aseguran que $\delta_{2022}, \delta_{2023}$ no capturen la revisión en lugar de la dinámica del gradiente?
9. **Modo de entrevista:** ¿Han verificado que la pregunta de educación parental se hace idénticamente en 2018, 2019, 2020 (telefónico parcial), 2021, 2022, 2023? Cambio de modo durante COVID puede generar variación sistemática que se confunde con el efecto.

---

## Internal Consistency, Hypotheses & Outcomes

### Critical Inconsistencies

1. **[CRITICAL] §1 (Research Question) ↔ §7.1 (Specification) — Hypotheses never formally enumerated.** Single research question but no numbered hypotheses with predicted signs. No parallel statement of recovery hypothesis, pre-trend null, or triple-diff hypothesis. **Fix:** Enumerate H1–H3 with directional predictions and decision rules.

2. **[CRITICAL] §5 ↔ §7.1 ↔ §9 — Only primary outcome hooked to estimating equation.** Five outcomes listed but only Pr(Poor=1) has a specified regression. **Fix:** Specify the regression for each outcome explicitly.

3. **[CRITICAL] §5 ↔ §7.3 — Asset-based poverty index has conflicting roles.** Simultaneously a "robustness outcome" AND a mitigation for the 2021 INEI revision. **Fix:** Pre-specify decision rule when consumption-based and asset-based diverge.

4. **[CRITICAL] §7.1 ↔ §6 ↔ §11 — Treatment functional form not pre-committed.** Four equally defensible parametrizations (max, father-only, mother-only, both, categorical) without one designated as primary. **Fix:** Lock to categorical as primary, continuous as robustness.

5. **[CRITICAL] §3 ↔ Original draft — Major design evolution (panels → cross-sections, 2018–2021 → 2018–2023).** Memo flags "replacing earlier panel-based draft" but does not disclose whether panel exploration informed cross-section design choices (cohort window, baseline year). **Fix:** Disclose design-evolution history transparently.

### Hypothesis or Outcome Coverage Gaps

Pre-trends placebo (no formal decision rule); recovery hypothesis (no outcome variable / test); triple-diff data source unverified; "head's own education conditional on parental" relies on a spec the PAP itself flags as bad control; sector formal/informal endogenous; vulnerability outcome conflicts with §7.3 basket-revision threat; UQR mentioned but not specified; multiple-testing correction absent; final analytic-N never given; sampling-weight × IPW × clustered SE interaction not specified.

### Terminology Drift

"Parental education" / "ParEduc" / "max parental education" / "parental SES" / "intergenerational gradient" used interchangeably (SES ≠ education); "gradient widens" / "becomes steeper" / "amplification" / "differentially incident" / "buffers" — sign ambiguity; "event-study" vs. "DiD with continuous treatment" vs. "saturated interaction model" — three names for one estimator; "conglomerate" / "PSU" / "cluster" hierarchy not specified; cohort restriction 25–65 vs. robustness 30–55 vs. heterogeneity 25–40/41–65 inconsistent.

### Minor Inconsistencies

Institutional-household exclusion only in §4; two different imputation rules in §3 and §6; conditional "if region of birth available"; basket-split robustness collinear with crisis-vs-recovery; "and/or" software; intra-family transfers (flow) listed as exploratory despite §10 explicitly disclaiming flows; §12 references panel attrition that this design doesn't have; three working-title candidates use three different verbs; parental income from original draft dropped without acknowledgment; "verify ENAHO module" should be resolved before registration; "decide whether to include 2024" needs ex-ante rule; no bibliography in PAP.

---

## Identification Strategy, Causal Claims & Contribution

### Major Identification or Design Problems

1. **[CRITICAL] Parallel-trends-in-gradient with a single pre-period year.** Acknowledged in §7.2 as "weak — only one pre-year" but treated as a footnote, not a first-order identification constraint. Cannot distinguish parallel trends from a transitory pre-shock. **Fix:** Extend to ENAHO 2015–2017; implement Rambachan-Roth (2023) sensitivity bounds.

2. **[CRITICAL] Mismatch between research question and estimand.** Question posed as "gradient widened" (population-level cross-sectional); literature anchor (Jiang et al.) asks "who fell into poverty" (within-household flow). The reader will expect the latter; the design delivers the former. **Fix:** Rewrite §1 to align estimand with question, or reintroduce panel as primary.

3. **[MAJOR] Identifying variation is across cohorts, not individuals.** In repeated cross-sections, individuals in 2018 and 2023 are different households. The interaction ParEduc × year identifies the shock effect only if the joint distribution (ParEduc, unobservables) is stable across years. Age 25–65 restriction does not achieve this. **Fix:** Use fixed birth-cohort restriction (e.g., 1965–1990), not age-window each year.

4. **[MAJOR] Triple-diff with provincial COVID mortality is endogenous.** Provincial mortality correlates with pre-existing poverty, informality, health infrastructure, parental-education composition. **Fix:** Use Bartik shift-share exposure to contracting sectors; or instrument mortality with hospital capacity / altitude / pre-COVID density.

5. **[MAJOR] Cohort 25–65 is problematic.** Lower bound mixes heads whose own education is still forming; upper bound includes heads whose parents are mostly deceased (selective missingness). **Fix:** Birth-cohort restriction; report gradient by 5-year age groups.

6. **[MAJOR] "Bad control" treatment is insufficient.** Reporting both with-and-without own education is the minimum. Own education is a mediator; the residual coefficient has no causal interpretation. **Fix:** Make the unconditional spec primary; conditional spec is descriptive only. Add Oster (2019) bounds.

7. **[MAJOR] LPM with binary outcome and tail-heterogeneity.** Probit/Logit as robustness is insufficient. The policy-relevant question is about the tail (falling into poverty). **Fix:** Pre-specify Unconditional Quantile Regressions (Firpo-Fortin-Lemieux) on log consumption at percentiles 10, 25, 50.

8. **[MAJOR] 2021 INEI basket revision is identification, not robustness.** $\delta_{2022}, \delta_{2023}$ are mechanically biased. **Fix:** Use INEI's harmonized back-cast series as primary; year-by-year official indicator moves to appendix.

### Overclaiming

1. "The design is transparent: parental education is a fixed individual characteristic, so its interaction with year identifies how the shock priced parental background" (§2) — "priced" is causal language exceeding what is identified.
2. "The crisis was *differentially incident* by parental background" (§10) — inconsistent with the simultaneous acknowledgment that compositional shifts may explain observed differences.
3. "Peru is a particularly informative case... predicts large heterogeneity in the incidence" (§2) — "predicts" overreaches; the methodology does not test this directly.
4. "Matches the policy question about *who* the crisis hit" (§12) — answering "who" requires individual-level identification; cross-sections answer "how the population distribution changed".
5. "Protection from crisis-induced poverty" (§7.1) — implies an individual insurance mechanism the design cannot point-identify.

### Underused Strengths

1. Honest §10 disclaimer should be elevated to abstract/introduction.
2. ~210,000 household-year observations across the 25–65 cohort (vs. ~3,000 in the 2-year panel) — should be quantified, not just claimed as "larger and more representative".
3. Extensive pre-specified robustness battery — should be presented as an integrative robustness map (threat → spec → expected bias direction → check), not as scattered mitigations.
4. Informal/formal heterogeneity is precisely the lever differentiating Peru from China (Jiang et al.) — should anchor the contribution claim in §2.

### Minor Positioning Issues

Literature gap on Jiang/Neidhöfer; ENAHO variable codes undefined; mean-imputation introduces SES-correlated error; pre-spec/exploratory frontier vague; title candidates use causal verbs in tension with §10; OSF requires explicit decision rules; "if region of birth available" needs pre-verification; missing CEQ/Lustig benchmark on distributional incidence in LatAm.

---

## Statistical Analysis Plan, Power & Multiple Testing

### Major Statistical or Power Problems

1. **[CRITICAL] No formal power calculation.** MDE for $\delta_\tau$ under conglomerate clustering, ICC 0.10–0.25, deff 2–4 is roughly 0.012–0.018 per parental year — plausible for the average effect but not for fine heterogeneity bins or for the 2018 placebo. **Fix:** Compute MDE for primary, heterogeneity subgroups, and placebo; report under realistic ICCs from ENAHO 2018–2019.

2. **[CRITICAL] Placebo 2018 underpowered by construction.** A single coefficient cannot distinguish $\delta_{2018}=0$ from low power. **Fix:** Report power of the placebo test against alternative pre-trend (e.g., 0.005 pp/year); add ENAHO 2016–2017; implement Rambachan-Roth (2023).

3. **[CRITICAL] Controls $X_{ih}$ not pre-specified.** Leaves room for specification search. **Fix:** Lock the regression columns and primary specification.

4. **[CRITICAL] Clustering ignores ENAHO stratified design.** Plain conglomerate clustering misses strata variation. **Fix:** `svy: regress` (Stata) with `svyset conglome [pweight=factor07], strata(estrato)` or `survey::svyglm` (R) for confirmatory SE.

5. **[MAJOR] Single mean-imputation of parental years.** Subestimates SE; ignores within-level variance. **Fix:** Multiple imputation (m=20, Rubin's rules); make categorical the primary spec.

6. **[MAJOR] MICE for missing parental education without characterizing the mechanism.** Probably NMAR (parents deceased, correlates with SES). **Fix:** Report missingness rate by year; pattern-mixture sensitivity; Horowitz-Manski bounds (not Lee — monotonicity unlikely under COVID parental mortality).

7. **[MAJOR] Multi-dimensional heterogeneity without power structure.** Five dimensions × 6 years × 4 outcomes ≈ several hundred tests. **Fix:** Declare two heterogeneity dimensions as confirmatory (sector, sex); rest exploratory; Romano-Wolf or List-Shaikh-Xu (2019).

### Multiple Testing or Specification Gaps

1. **[CRITICAL]** 5 $\delta_\tau$ × 4 outcomes = 20 primary tests without correction. **Fix:** Designate monetary poverty as the only confirmatory outcome; Romano-Wolf for the 5 event-time coefficients; Anderson sharpened q-values for the heterogeneity table.
2. **[MAJOR]** Confirmatory vs. exploratory boundary fuzzy in §11.
3. **[MAJOR]** Triple-diff equation not formalized; mortality variable unspecified.
4. **[CRITICAL]** Asset index PCA construction not pre-specified (items, pooling rule, sign, threshold).

### Missing or Inadequate Robustness Checks

1. **[MAJOR]** No placebo gradient on pre-determined outcomes (head age, sex, # siblings) — would catch compositional shifts.
2. **[MAJOR]** Categorical specification of parental education should be co-primary, not robustness.
3. **[MINOR]** Cohort window robustness limited to one alternative (30–55); test ≥3 windows.
4. **[MAJOR]** Sample-composition balance test absent.
5. **[MAJOR]** Triple-diff with ~196 provinces needs wild-cluster bootstrap (Cameron-Gelbach-Miller 2008).
6. **[MINOR]** Overlap diagnostic on ParEduc distribution across years absent.

### Minor Statistical Issues

Ai & Norton (2003): probit/logit interaction coefficients are not effects; report AME via `margins`/`marginaleffects`. `fixest` vs. `reghdfe` may differ in small-sample SE correction (CR1 vs CR2). Weights × FE interaction in software-specific. Use simultaneous 95% bands (sup-t) for event-study plots. Register seeds for MICE and bootstrap. Lee bounds: specify direction and applicability to binary outcomes. Pre-commit decision rule for 2024 inclusion before INEI release.

---

## Data, Sample, Implementation & Operational Plan

### Sample or Data Access Concerns

1. **[CRITICAL] Parental-education ENAHO variable not identified, not module-located.** Probably `p106` family or a parent-education roster in Module 100/300, asked only of the head, not of all members. Wording or skip pattern may have changed in 2020 (telephone interviews). **Fix:** Name the variable, verify across 2018–2023, tabulate non-missing rates.
2. **[CRITICAL] No harmonization for 2021 INEI poverty-line revision.** Asset-PCA robustness is insufficient. **Fix:** Use INEI's back-cast series; methodology-break placebo; replicate published rates within ±0.5 pp.
3. **[CRITICAL] 2017 Census sampling frame discontinuity not addressed.** ENAHO weights changed. **Fix:** Specify exact weight variable; reproduce INEI's published poverty rates.
4. **[MAJOR] Headship not invariant across years.** Selection into headship during COVID is endogenous. **Fix:** Robustness sample of adults 25–65 regardless of headship; "stable headship" subsample.
5. **[MAJOR] Cohort 25–65 conflicts with parental-education availability.** **Fix:** Pre-specify max missingness; MICE variable list; bounds.
6. **[MAJOR] Imputed years depends on changing ENAHO categorical codes.** **Fix:** Harmonized 5-level categorical as primary.
7. **[MAJOR] Panel-rotation contamination (~20–25% household overlap).** **Fix:** Drop overlaps or two-way cluster.
8. **[MAJOR] Heterogeneity multiple-testing problem unaddressed.**
9. **[MINOR] CONCYTEC mortality citation vague; should be SINADEF.**

### Implementation or Feasibility Risks

1. **[CRITICAL]** No code skeleton, no replication archive, no analyst assignment for data cleaning. Even basic descriptive Ns are not yet computed. **Fix:** "Shell paper" on 2019 only with all tables/figures populated with mock estimates before PAP registration.
2. **[MAJOR]** No internal timeline; realistic implementation is 4–6 months.
3. **[MAJOR]** R/Stata dual-software plan increases reconciliation burden.
4. **[MAJOR]** Mechanisms language risks turning exploratory into confirmatory ex post.
5. **[MINOR]** 2024 inclusion creates dependency on INEI release calendar.

### Ethical or Regulatory Gaps

1. **[MINOR]** No IRB exemption statement (likely needed from CIUP Comité de Ética).
2. **[MINOR]** No INEI data-use / citation acknowledgment.
3. **[MINOR]** Re-identification risk via PSU × provincial mortality merge should be acknowledged.

### Missing or Weak Supporting Documents

1. **[CRITICAL]** No data dictionary / variable crosswalk (highest-priority missing artifact).
2. **[CRITICAL]** No power / MDE calculation.
3. **[MAJOR]** No mock tables or figures.
4. **[MAJOR]** No code skeleton on OSF.
5. **[MAJOR]** No replication / data-sharing plan (required by JDE AEA policy).
6. **[MAJOR]** No SINADEF mortality auxiliary file documentation.
7. **[MINOR]** No deviations-log plan.
8. **[MINOR]** No conflict-of-interest / funding statement.

---

## Clarity, Writing Quality & Pre-specification Completeness

### Critical Vagueness or Specification Gaps

1. **[CRITICAL] §7.1 — Controls vector $X_{ih}$ undefined in main spec.**
2. **[CRITICAL] §7.1 — Functional form of ParEduc not pinned (linear? quadratic? standardized?).**
3. **[CRITICAL] §5 — No designated primary test among the five-outcome × six-event-time family.**
4. **[CRITICAL] §8 — Heterogeneity subgroups not operationalized as triple-interaction or split-sample; no directional hypotheses.**
5. **[CRITICAL] §7.3 — Mitigations stated conditionally ("if available"), undermining binding.**
6. **[CRITICAL] §4 — "Excluding the year of the head's internal migration" not operationalized; missingness rule not tied to mitigations.**
7. **[CRITICAL] §10 — "Widened/narrowed" both on the table contradicts §7.1's directional sign of interest.**
8. **[MAJOR] §7.2 — Placebo failure has no decision rule.**
9. **[MAJOR] §6 — Imputation rule (mean of level) differs from the original draft's deterministic step-rule.**
10. **[MAJOR] §8 — CONCYTEC provincial mortality undocumented (source, year, unit).**
11. **[MAJOR] §9 — "Results should align" between LPM and probit/logit has no tolerance.**
12. **[MAJOR] §11 — Pre-specified vs. exploratory boundary fuzzy.**

### Minor Writing Issues

Citation style consistency; "shock priced parental background" idiom; "~70% of labor force" needs year/source; "ENAHO conversion" ambiguous (no canonical conversion for parental education); collective households may already be excluded by ENAHO frame; PCA item list missing; LPM equation should be footnoted; Lee bounds citation; "residual effect" non-standard terminology; pick R or Stata; "panel released with longer lag" — verify; resolve "open issues" before posting; heading inconsistencies (Lima Metropolitana, Costa-Sierra-Selva); rewrite awkward sentences; standardize "household head".

### Structural or Compliance Signals to Fix

1. **[CRITICAL] Hypotheses not numbered or stated as testable propositions.**
2. **[CRITICAL] No multiple-testing correction policy.**
3. **[CRITICAL] No attrition / missing-data rule binding.**
4. **[MAJOR] No deviations-from-PAP policy.**
5. **[MAJOR] No data-access status or timeline.**
6. **[MAJOR] No power discussion.**
7. **[MAJOR] Primary outcome vs. primary test conflated.**
8. **[MAJOR] Decision rule for the main estimator absent.**
9. **[MAJOR] No replication / open-code commitment.**
10. **[MINOR] §11 should appear immediately after the hypotheses.**
11. **[MINOR] "Working title — candidates" — pick one.**
12. **[MINOR] "Open issues to resolve" — resolve and remove.**
