# Theta/gamma breakeven P&L decomposition and its extension to stochastic volatility

Source-reliability legend used below:
- **[PRIMARY, read]**: I extracted and read the original text (PDF) myself.
- **[SECONDARY]**: a third-party summary or reading note of the original; the wording or equation numbers may not match the original.
- **[DERIVED]**: my own derivation from cited primary formulas. It appears only under Inferences.

## Q1. Black-Scholes: delta-hedged P&L = ½ΓS²(realised var − implied var)dt and the theta-gamma breakeven

### Takeaway
Under Black-Scholes, the pricing PDE ties theta to dollar gamma: θ − rV + (r−q)SΔ = −½σ̂²S²Γ. Substituting this into the Itô/Taylor expansion of a delta-hedged, financed position leaves only ½ΓS²(δS²/S² − σ̂²δt). Theta "pays for" gamma at the implied-variance breakeven. Integrated over the option's life with hedging at implied volatility x, the P&L is ∫½S²Γ(S,x,u)(v_u − x²)du, a dollar-gamma-weighted average of realised-minus-implied variance (Carr & Madan 2002, as restated by Carr & Wu 2016). I verified this primary statement myself. I could not open Hull, Taleb, Sinclair, Carr's "FAQs", or the Ahmad & Wilmott paper.

### Cited Findings
- **[PRIMARY, read]** Carr & Wu (2016) restate the Carr & Madan (2002) result: "if one buys the options at BMS volatility x and performs continuous delta hedge at this volatility, the delta-hedged PL can be written as PL = ∫_t^T ½ S_u² B_SS(S_u, x, u)(v_u − x²) du" (their eq. 19). Setting PL = 0 gives x² as "a weighted average of the instantaneous variance rate with the weight given by the dollar gamma of the option at each point of the sample path". A variance swap is the special case with constant dollar gamma. — [Carr & Wu, "Analyzing volatility risk and risk premium in option contracts: A new theory", JFE 120 (2016) 1–20, Sec. 3, eq. (19)](https://engineering.nyu.edu/sites/default/files/2018-09/CarrJournalofFinEconomics2016.pdf)
- **[PRIMARY, read]** Same paper: the ex-post P&L of buying and delta-hedging to expiry equals the BMS value difference "PL(t,T) = B(S, ORV, t) − B(S, I_t, t)" (eq. 20). ORV is the "option realized volatility" that sets the delta-hedged PL to zero. — [Carr & Wu 2016, eq. (20)](https://engineering.nyu.edu/sites/default/files/2018-09/CarrJournalofFinEconomics2016.pdf)
- **[SECONDARY]** Bergomi, *Stochastic Volatility Modeling* (2016), Ch. 1, per a third-party chapter summary:
  - BS pricing PDE (1.4): dP/dt − rP + (r−q)S dP/dS = −(σ̂²/2)S² d²P/dS².
  - Delta-hedged P&L (1.5): P&L = −(S²/2)(d²P/dS²)(δS²/S² − σ̂²δt), shown from the option seller's side, hence the minus sign. Bergomi calls this "a useful accounting tool".
  - Here the financing (−rP) and carry ((r−q)SΔ) terms sit on the left-hand side of the PDE alongside dP/dt. This is how Bergomi folds financing into the breakeven.

  — [junfanz1, "Stochastic Volatility Modeling Ch. 1 Introduction – Notes"](https://junfanz1.github.io/blog/book%20notes%20series/Stochastic-Volatility-Modeling-Char-1-Introduction-Notes)
- **[SECONDARY]** Same summary, multi-asset generalisation (Bergomi Ch. 1, Sec. 1.1): with cross-gammas φ_ij = S_iS_j d²P/dS_idS_j, there is a positive "break-even covariance matrix" C such that A = −½Σ_ij φ_ij C_ij, "relating the model's theta and cross-gammas". The summary implies A is the (financing-adjusted) theta but does not define it explicitly. — [junfanz1 notes](https://junfanz1.github.io/blog/book%20notes%20series/Stochastic-Volatility-Modeling-Char-1-Introduction-Notes)
- **[PRIMARY, read]** Bergomi's 2004 Risk article uses the same "gamma/theta P&L" language. Hedging −2ln(S) with the BS delta, "the gamma portion of the gamma/theta profit and loss realised during Δt" is expanded to third order in ΔS, to explain why variance swaps are not priced at the log-strip volatility. — [Bergomi, "Smile dynamics", Risk Sept 2004, pp. 117–123 (copy on Crépey's site)](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/0904_tech_bergomi.pdf)
- **[SECONDARY]** Ahmad & Wilmott, "Which Free Lunch Would You Like Today, Sir?: Delta Hedging, Volatility Arbitrage and Optimal Portfolios", Wilmott Magazine, Nov 2005.
  - The paper distinguishes implied, actual and hedging volatilities.
  - Hedging at actual volatility gives a guaranteed, path-independent final profit with volatile mark-to-market P&L.
  - Hedging at implied volatility gives smooth mark-to-market P&L but a path-dependent total.
  - I could not access the paper's equations; wilmott.com is members-only and the StudyLib copy had no body text.

  — [wilmott.com listing](https://wilmott.com/?p=1358); [HarbourFront Quant newsletter summary](https://harbourfrontquant.substack.com/p/implied-vs-realized-volatility-in)
- **[SECONDARY]** A quant-problem page derives P&L ≈ ½ΓS²[(ΔS/S)² − σ_imp²Δt] and calls a delta-hedged option over many days "a pure volatility trade". — [quantvault.org problem 1502](https://quantvault.org/problem-1502-theta-gamma-relationship-and-delta-hedged-p-l.html)

### Inferences
- **[DERIVED]** Sketch of the derivation in the BS case.
  - Long one option V(t,S) at implied σ̂, short Δ = V_S shares. The cash account is V − ΔS, financed at r. Dividends of qΔS dt are paid on the short stock.
  - dΠ = V_t dt + Δ dS + ½Γ dS² − Δ(dS + qS dt) − r(V − ΔS)dt.
  - The Δ dS terms cancel, leaving dΠ = [V_t − rV + (r−q)SΔ]dt + ½Γ dS².
  - The BS PDE sets the bracket to −½σ̂²S²Γ, so dΠ = ½ΓS²(dS²/S² − σ̂²dt).
  - So the "theta" that pays for gamma is the financing-adjusted theta V_t − rV + (r−q)SΔ, not the bare V_t.
  - This matches the arrangement of Bergomi's (1.4), where the −rP + (r−q)S P_S terms sit next to dP/dt (secondary source).
- Carr & Wu set r = 0 throughout (see Q3), so in their formulas financing is absent by assumption, not handled explicitly.

### Gaps
- I could not open Peter Carr's "FAQs in Option Pricing Theory" (1999/2002). The NYU math URL redirects to a faculty page, and only the Scribd abstract is visible ([Scribd](https://www.scribd.com/doc/202131602/faq2)). Its exact theta/gamma statement is unverified.
- I did not access Hull, Wilmott (PWIQF), Taleb (*Dynamic Hedging*) or Sinclair (*Volatility Trading*) in this session. I found no citable page or equation numbers for their breakeven statements; the report writer should not attribute specific formulas to them from these notes.
- Ahmad & Wilmott's exact formulas are unverified, as is the treatment of discounting e^{−r(t−t0)} in their integral.
- Carr & Madan (2002), "Towards a theory of volatility trading", is cited here only through Carr & Wu 2016.

## Q2. Stochastic vol: theta pays for gamma, vanna, volga and the variance drift

### Takeaway
In a stochastic volatility model the pricing PDE makes (financing-adjusted) theta equal to minus the sum of four model-priced terms:
- dollar-gamma × instantaneous variance
- vanna × spot-vol covariance
- volga × vol-of-vol²
- vega × risk-neutral variance drift

Each second-order Greek therefore has a breakeven level: the model's covariance parameter. Realised P&L is the Greek times (realised − model) covariance, plus the vega term on the unexpected variance move. Bergomi's central point is that the purpose of an SV model is to supply these breakevens (theta matching ∂²P/∂σ² and ∂²P/∂S∂σ). Carr & Wu (2016) give the same structure in implied-volatility coordinates, with a vega × implied-vol drift term.

### Cited Findings
- **[PRIMARY, read]** Carr & Wu (2016), Proposition 1. With dS/S = √v dW, dI = μdt + ωdZ, and E[dWdZ] = ρdt, no dynamic arbitrage gives the "fundamental PDE" (eq. 6):
  - −B_t = μ_t B_σ + ½v_t S_t² B_SS + ρ_t ω_t √v_t S_t B_Sσ + ½ω_t² B_σσ
  - The terms are named "theta for B_t, vega for B_σ, dollar gamma for S²B_SS, dollar vanna for S B_Sσ, and volga for B_σσ". The class is christened the "Vega-Gamma-Vanna-Volga (VGVV) model".
  - When μ and ω are independent of (K, T), eq. (6) "defines a linear relation between the BMS theta of the option and its vega, dollar gamma, dollar vanna, and volga".

  — [Carr & Wu 2016, Sec. 2.1, eq. (6)](https://engineering.nyu.edu/sites/default/files/2018-09/CarrJournalofFinEconomics2016.pdf)
- **[PRIMARY, read]** Carr & Wu (2016) note that Arslan, Eid, Khoury & Roth (2009) derived the special case with zero implied-vol drift ("GVV model": gamma, vanna, volga, no vega term). They also note that eq. (6) "is not a PDE in the traditional sense": it constrains the stochastic quantities and does not solve for the value function. — [Carr & Wu 2016, Sec. 2.1](https://engineering.nyu.edu/sites/default/files/2018-09/CarrJournalofFinEconomics2016.pdf)
- **[SECONDARY]** Carr & Wu (2020), "Option Profit and Loss Attribution and Pricing: A New Framework", J. Finance 75(4):2271–2316.
  - Per a reading note: dB = [B_t dt + B_S dS + B_I dI] + [½B_SS dS² + ½B_II dI² + B_IS dS dI] + higher-order terms.
  - Taking risk-neutral expectations gives B_t = −[½B_SS S²σ² + ½B_II I²ω² + B_IS I S γ + B_I I μ], with proportional moments of dI/I.
  - Under BS cash Greeks this becomes I² = 2μI²τ + σ² + ω²d₁d₂I²τ − 2γd₂I√τ.
  - The note assumes zero rates.

  — [double-free reading note](https://double-free.github.io/notes/trading/option/OptionPnL); abstract: [IDEAS/RePEc](https://ideas.repec.org/a/bla/jfinan/v75y2020i4p2271-2316.html)
- **[PRIMARY, read]** Bergomi (2004), on why SV models are needed for exotics like Napoleons and reverse cliquets: "∂²P/∂σ̂² and ∂²P/∂S∂σ̂ are sizeable and a suitable model needs to price in a theta to match these gammas." The BS price has "no theta matching ∂²P/∂σ̂²" and "no theta matching ∂²P/∂S∂σ̂". Spot-vol correlation produces "a negative profit and loss to the seller, not accounted for in the Black-Scholes price". — [Bergomi, "Smile dynamics", Risk Sept 2004, p. 117](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/0904_tech_bergomi.pdf)
- **[SECONDARY]** Bergomi, *Stochastic Volatility Modeling*, Ch. 1, Sec. 3, per chapter summary.
  - After delta-hedging and gamma-hedging the exotic P with vanilla O (λ = P_SS/O_SS, eq. 1.18), the residual P&L (1.20) is: −(P_σ̂ − λO_σ̂)δσ̂ − ½(P_σ̂σ̂ − λO_σ̂σ̂)δσ̂² − (P_Sσ̂ − λO_Sσ̂)δSδσ̂.
  - Unlike δS², there is "no deterministic δt term" offsetting δσ̂² and δSδσ̂. There is no implied vol-of-vol or implied correlation parameter to set breakeven levels, hence the need for SV models.
  - The book frames the aim of SV models as modelling implied-vol dynamics and providing "simple break-even accounting conditions for the P&L", not modelling realised volatility.

  — [junfanz1 notes](https://junfanz1.github.io/blog/book%20notes%20series/Stochastic-Volatility-Modeling-Char-1-Introduction-Notes)
- **[SECONDARY]** Heston-specific attribution, from an online textbook chapter (Sung Chul Lee, "Financial Math", Sec. 16.6):
  - PDE: V_t + ½vS²V_SS + ρξvS V_Sv + ½ξ²v V_vv + (r−q)SV_S + κ(θ−v)V_v − rV = 0.
  - P&L: dΠ = Θdt + ½ΓvS²dt + 𝒱dv + 𝒜ρξvS dt + ½𝒢ξ²v dt − r(V − ΔS)dt.
  - Substituting the PDE leaves dΠ = 𝒱[dv − κ(θ−v)dt] = 𝒱ξ√v dW².
  - Θ = V_t, with vega taken with respect to variance v.
  - Caveat: the page's portfolio omits dividend income on the hedge, so its q-handling looks inconsistent. It is a non-peer-reviewed web textbook.

  — [sungchullee.github.io, "P&L Explanation Under Heston"](https://sungchullee.github.io/financial_math_book_writing/ch16/greeks/pnl_explanation_under_heston)
- **[SECONDARY]** A 2025 arXiv paper building on Bergomi (2016, Sec. 2.4.1) defines a "P&L bleed" Z = β̂∂_αV + ½γ̂²∂_ααV + ρ_Sα α_S γ̂ ∂_SαV for the move from local to stochastic vol. It is a drift (vega) + volga + vanna structure. — [arXiv 2503.14997, "The fundamental representation of pricing adjustments"](https://arxiv.org/pdf/2503.14997)

### Inferences
- **[DERIVED]** General SV breakeven form (Heston coordinates), long a delta-hedged option, financed, under real-world dynamics:
  - dΠ = ½ΓS²(dS²/S² − v dt) + V_Sv(dS dv − ρξvS dt) + ½V_vv(dv² − ξ²v dt) + V_v(dv − κ(θ−v)dt)
  - Each bracket is "realised minus model-breakeven". The PDE has already used theta to pay the model-expected amounts.
  - Gamma, vanna and volga breakevens are v, ρξv·S, and ξ²v.
  - The vega/drift term is first-order in dv. Its "breakeven" is the risk-neutral drift κ(θ−v); its expected value under P reflects the variance risk premium (drift under P minus under Q).
- **[DERIVED]** This is the SV analogue of Carr & Wu's μB_σ term and of the β̂∂_αV term in arXiv 2503.14997.

### Gaps
- I did not read Bergomi's own book text for the multi-Greek SV breakeven equation or its numbering (e.g. Ch. 7/8 on forward variance models). The book is paywalled and no primary excerpt was found.
- I found no Bergomi Risk article titled "Theta (and other Greeks)". The search returned no such title, so the reference may be misremembered or may be a book section. Unverified.
- No Gatheral source was accessed on this point.
- I found no peer-reviewed Heston-specific P&L-attribution paper; only the web textbook above.

## Q3. How theta is defined in these decompositions; Bergomi's forward-variance state and the drift term

### Takeaway
In every source checked, theta is the partial time derivative with the other state variables held fixed:
- Carr & Wu: (S, I) fixed, with B the BMS formula.
- Heston: (S, v) fixed.
- Bergomi Ch. 1, BS case: S fixed (secondary source).

Which variables are held fixed changes what theta "pays for". With instantaneous variance v as state (Heston), the PDE contains a variance-drift term κ(θ−v)V_v, so theta also pays for vega × drift. With implied vol as state (Carr & Wu), theta pays for vega × implied-vol drift μ. With the forward-variance curve ξ^T(t) as state (Bergomi), forward variances are driftless under the pricing measure, so the drift-times-vega term vanishes from the pricing equation. Theta then pays only for second-order terms (gamma, vanna, volga/cross-gammas in ξ).

### Cited Findings
- **[PRIMARY, read]** Carr & Wu (2016) take partial derivatives of B(S, σ, t) and evaluate them at (S_t, I_t(K,T), t). "Subscripts of B denote partial derivatives"; "theta for B_t". Theta is therefore ∂B/∂t at fixed S and fixed implied vol. — [Carr & Wu 2016, Sec. 2.1](https://engineering.nyu.edu/sites/default/files/2018-09/CarrJournalofFinEconomics2016.pdf)
- **[PRIMARY, read]** Bergomi, "Smile dynamics II" (Risk Oct 2005).
  - A forward variance-swap position "generates a profit and loss linear in V_{t+dt}^{T1,T2} − V_t^{T1,T2} at lowest order in dt, at zero initial cost. Thus the pricing drift of any forward FV V_t^{T1,T2} is zero."
  - With ξ^T_t "the value of the variance for date T, observed at time t", "we are free to specify any dynamics on the ξ^T(t) that complies with the requirement that ξ^T(t) be drift-less".
  - Footnote: "The drift-less nature of forward VS variances had been noticed before (see Dupire, 1996)".

  — [Bergomi, "Smile dynamics II", Risk Oct 2005 (copy on Crépey's site)](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/bergomi.pdf)
- **[SECONDARY]** Heston web textbook: Θ = V_t with (S, v) fixed; the drift term κ(θ−v)V_v appears explicitly in both the PDE and the theta identity. — [sungchullee.github.io](https://sungchullee.github.io/financial_math_book_writing/ch16/greeks/pnl_explanation_under_heston)

### Inferences
- **[DERIVED]** Consequence for forward-variance models. Take P(t, S, ξ^·) with the ξ^T as tradable, driftless state variables. Delta-hedge, and vega-hedge each forward variance (with variance swaps, at zero cost). The P&L then contains no first-order drift term. Theta (at fixed S and fixed forward-variance curve, financing-adjusted) pays only for ½S²P_SS·ξ^t_t (the spot gamma, at instantaneous variance ξ^t_t), the S–ξ cross-gammas × model covariance, and the ξ–ξ gammas × model covariance.
- **[DERIVED]** Theta is coordinate-dependent. Heston's V_t|_{S,v} differs from a forward-variance-model theta at fixed ξ curve, because the Heston forward curve E[v_T] = θ + (v−θ)e^{−κ(T−t)} rolls with t at fixed v. The κ(θ−v)V_v term is exactly that roll-down. Re-expressed in forward-variance coordinates, the roll is absorbed and the drift term disappears.
- The precise statement in Bergomi's book (theta defined at fixed ξ curve, with no drift term) is consistent with the 2005 primary text but was not verified in the book itself.

### Gaps
- I did not obtain Bergomi's book wording on the theta definition in forward-variance models (Ch. 7), or the treatment of the ξ^t_t "short end" term.
- No source was found explicitly reconciling Heston-coordinate theta with forward-variance-coordinate theta; the inference above is mine.

## Q4. Carry/financing term (rV − (r−q)SΔ) handling

### Takeaway
Two conventions appear:
- Bergomi Ch. 1 (secondary) and the Heston textbook keep r and q in the PDE. The "theta" that pays for gamma is effectively V_t − rV + (r−q)SΔ. Financing of the option premium and of the hedge (including dividends) is netted against time decay, so the breakeven statement holds exactly.
- Carr & Wu (2016) assume zero rates explicitly, and the Carr & Wu (2020) reading note assumes zero rates and zero stock drift. Financing is absent by construction; the implicit convention is that forward/discounted quantities are used.

### Cited Findings
- **[PRIMARY, read]** Carr & Wu (2016), Appendix A proof of Prop. 1: "No dynamic arbitrage and no rates imply that both option drifts must vanish, leading to the fundamental partial differential equation". The stock is modelled as a Q-martingale: dS/S = √v dW (eq. 1). — [Carr & Wu 2016, eq. (1) and App. A, eqs. (44)–(45)](https://engineering.nyu.edu/sites/default/files/2018-09/CarrJournalofFinEconomics2016.pdf)
- **[SECONDARY]** Bergomi Ch. 1 BS PDE (1.4) carries −rP + (r−q)S dP/dS on the theta side. The summary notes that Sec. 3.3 assumes "zero interest rates for simplicity". — [junfanz1 notes](https://junfanz1.github.io/blog/book%20notes%20series/Stochastic-Volatility-Modeling-Char-1-Introduction-Notes)
- **[SECONDARY]** Heston textbook: the financing term is −r(V − ΔS)dt in dΠ; −rV and (r−q)SΔ appear in the theta identity. The text says the stock drift cancels with the Δ dS hedge, but its portfolio omits the qΔS dividend leg, which is a likely error. — [sungchullee.github.io](https://sungchullee.github.io/financial_math_book_writing/ch16/greeks/pnl_explanation_under_heston)
- **[PRIMARY, read]** Bergomi (2004) footnote: VS/log-contract pricing depends "on how dividends are modelled and assumptions on interest rate volatility". This indicates that carry conventions affect breakevens for variance products. — [Bergomi, Smile dynamics II, Risk Oct 2005, footnote 1](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/bergomi.pdf)

### Inferences
- **[DERIVED]** Full accounting with dividends: the financed, delta-hedged P&L is [V_t − rV + (r−q)SΔ]dt + ½ΓdS² + (SV terms). The term rV − (r−q)SΔ is the net carry: interest on premium minus interest on the short-stock proceeds net of dividends paid. Desks often report "theta" either as bare V_t or as V_t plus carry; only the latter satisfies the clean −½σ̂²S²Γ identity. Working in forward/undiscounted terms (r = q = 0, as Carr & Wu do) makes the two coincide.
- Ahmad & Wilmott's present-value integral reportedly discounts each instantaneous ½(σ² − σ̃²)S²Γ^i dt contribution at r. This is unverified; see Q1 Gaps.

### Gaps
- I found no primary source that explicitly names and separates the "carry" term rV − (r−q)SΔ from theta in a P&L-attribution table. Practitioner conventions (Sinclair, Taleb) were not accessed.
