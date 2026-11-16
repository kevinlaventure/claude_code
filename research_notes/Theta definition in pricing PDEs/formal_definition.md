# Formal definition of theta in option pricing PDEs (Black-Scholes and Heston)

Source-quality note: most textbook full texts (Hull, Wilmott, Gatheral's book, Rouah, Bergomi, Taleb) were not readable online. The strongest primary-type sources I could actually read are: (a) Jim Gatheral's Courant Institute lecture notes (Fall 2004), which are the precursor of chapters 1-2 of *The Volatility Surface* and explicitly "follow Wilmott (1998) closely"; (b) Kohn/Allen NYU Derivative Securities notes (2012, keyed to Hull 8th ed.); (c) NAG Library documentation for its Heston Greeks routine; (d) Hull's own 7th-edition chapter slides (search snippet only); (e) Wikipedia. The Heston 1993 PDF was retrieved but is a scanned image and could not be text-extracted. Anything taken from memory or from secondary sources is flagged.

## Q1. How do standard references define theta?

### Takeaway
Every source I could verify defines theta as the rate of change of value with respect to the passage of calendar time, all else equal. That is ∂V/∂t, or equivalently −∂V/∂τ with τ = T − t, holding the other pricing inputs fixed. None of the verified sources spells out "S, v, r, q and model parameters fixed" in one sentence. The "all else equal" condition is stated in words (Hull, Wikipedia) or follows from the partial-derivative notation (NAG, Kohn).

### Cited Findings
- **Hull (7th ed., Ch. 17 slides, by Hull):** "Theta (Θ) of a derivative (or portfolio of derivatives) is the rate of change of the value with respect to the passage of time"; "The theta of a call or put is usually negative." — [Hull Ch.17 slides on issuu (search snippet; page now 404)](https://issuu.com/zalimkasap/docs/ch17hullofod7thed). FLAG: the book's fuller wording, "...with respect to the passage of time with all else remaining the same. Theta is sometimes referred to as the time decay of the portfolio," is widely quoted. I could not verify it against a page image.
- **Hull per-day convention:** an R package implementing Hull 7th ed. examples (Ch. 17, pp. 367–368) says to divide the annual theta by 365 for "per calendar day" and by 252 for "per trading day." — [ustreasuries ThetaCall docs](https://rdrr.io/github/grfiv/ustreasuries/man/ThetaCall.html) (secondary; attributes this to Hull).
- **Kohn/Allen, NYU (keyed to Hull 8th ed.):** tabulates "theta = ∂/∂(−T)" for calls and puts (formulas at t = 0; "the formulas applicable at any time t are similar, with T replaced by T−t"). It states: "Theta is defined as the derivative with respect to a decrease in time to option expiry. This is the standard market convention, since the primary concern is with a 'decay' in option value as time passes. (The market convention is often to show the theta as the derivative with respect to a decrease of one business day, in which case the value returned by the formula needs to be divided by the number of business days in a year, about 252)." — [Kohn & Allen, Section 5 notes](https://math.nyu.edu/~kohn/derivative.securities/2012/section5.pdf)
- **Wikipedia "Greeks (finance)":** Θ = −∂V/∂τ. Theta "measures the sensitivity of the value of the derivative to the passage of time... the 'time decay.'" "By convention in options valuation formulas, τ, time to expiry, is defined in years." Theta is usually divided by the number of days in a year, and "Whether to count calendar days or business days varies by personal choice." — [Wikipedia](https://en.wikipedia.org/wiki/Greeks_(finance))
- **NAG Library (Heston Greeks routine s30nd):** theta is "the first-order Greek measuring the sensitivity of the option price P_ij to change in time, i.e., −∂P_ij/∂T," where T is time to expiry. The other inputs (S, v_t, r, q, κ, η, σ_v, ρ) are separate arguments with their own sensitivities. This means theta is a partial with those held fixed (my inference from the interface). — [NAG s30ndf](https://support.nag.com/numeric/nl/nagdoc_29/flhtml/s/s30ndf.html)
- **Gatheral (lecture notes, the basis of *The Volatility Surface*, Ch. 1):** writes the valuation equation for V(S, v, t) with ∂V/∂t as the partial with respect to calendar time t. S, v and t are independent variables, and "some function f of the independent variables S, v and t" is identified as the market price of volatility risk. He does not use the word "theta" in this passage. — [Gatheral, Lecture 1 (2004), §1.2](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/lecture1_2004.pdf)
- **Bergomi (*Stochastic Volatility Modeling*, Ch. 1), via secondary reading notes:** the Black-Scholes equation (1.4) contains dP/dt. The delta-hedged P&L (1.5) is gamma × (δS²/S² − σ̂²δt), where the δt term is the theta offset. With multiple hedge instruments, "the model's theta and cross-gammas are related through" A = −½ Σ φ_ij C_ij, where C is a break-even covariance matrix. Sensitivities are evaluated "for current values of t, S." — [junfanz1 notes on Bergomi Ch.1](https://junfanz1.github.io/blog/book%20notes%20series/Stochastic-Volatility-Modeling-Char-1-Introduction-Notes) (SECONDARY; not verified against the book).
- Practitioner framing: theta "assumes the market is frozen and the only thing that's changing is the passage of time" — [tastytrade](https://tastytrade.com/learn/greeks/theta) (secondary/practitioner).

### Inferences
- Sign convention is consistent everywhere: theta = ∂V/∂t (calendar time) = −∂V/∂τ (time to expiry). Long vanilla options usually have negative theta.
- In a stochastic-volatility model the state variables are (S, v, t). The natural reading of "all else equal" is therefore S and the instantaneous variance v held fixed, together with r, q and the model parameters. NAG's interface (v_t as an input with its own vega) is consistent with this. A Black-Scholes-implied-vol theta, which holds σ_imp fixed instead, is a different quantity in general.

### Gaps
- **Wilmott (PWOQF):** I could not access the text to quote his definition. From memory (unverified): he defines Θ = ∂V/∂t and writes the BS equation as Θ + ½σ²S²Γ + rSΔ − rV = 0. Do not cite without checking.
- **Rouah (*The Heston Model and its Extensions*):** not accessed. His frouah.com notes ("Derivation of the Heston Model") returned 404. A Scribd copy was truncated.
- **Taleb (*Dynamic Hedging*):** not accessed. No verified quote.
- **Heston (1993):** PDF retrieved ([Imperial copy](https://www.ma.imperial.ac.uk/~ajacquie/IC_Num_Methods/IC_Num_Methods_Docs/Literature/Heston.pdf)) but it is a scanned image, so I could not extract its text. It does not define a Greek called "theta". In Heston's paper θ is the long-run variance parameter, a notational clash also noted by [optionsanalysissuite](https://www.optionsanalysissuite.com/documentation/heston-theta-param).
- Exact Hull page wording and the Bergomi book text are unverified.

## Q2. In the Heston PDE, is ∂V/∂t a partial with S and v fixed? Is ∂V/∂t = −∂V/∂τ noted?

### Takeaway
Yes. In the standard derivation, V = V(S, v, t) and ∂V/∂t is the partial with S and v held fixed (Gatheral, following Wilmott). Gatheral then explicitly substitutes τ = T − t, so that ∂/∂t becomes −∂/∂τ. One subtlety: in his transformed equation the −∂C/∂τ is taken at fixed forward log-moneyness x = ln(F(t,T)/K) and is applied to the forward (undiscounted) value. That is not the same as fixed S and spot value.

### Cited Findings
- **General SV valuation equation (Gatheral eq. 3, "we follow Wilmott (1998) closely"):** a portfolio Π = V − ΔS − Δ₁V₁ with V(S, v, t). Itô gives dΠ = (∂V/∂t + ½vS²∂²V/∂S² + ρηvβS∂²V/∂v∂S + ½η²vβ²∂²V/∂v²)dt − Δ₁(...)dt + (...)dS + (...)dv. Hedging out dS and dv leads to ∂V/∂t + ½vS²V_SS + ρηvβSV_vS + ½η²vβ²V_vv + rSV_S − rV = −(α − φβ)V_v. Both sides "must be equal to some function f of the independent variables S, v and t." — [Gatheral Lecture 1, §1.2](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/lecture1_2004.pdf)
- **Heston specialisation (Gatheral eqs. 12–13):** α = −λ(v − v̄), β = 1 gives ∂V/∂t + ½vS²V_SS + ρηvSV_vS + ½η²vV_vv + rSV_S − rV = λ'(v − v̄')V_v, with risk-adjusted λ', v̄'. This is the PDE in the brief, with ξ ↔ η, κ ↔ λ', θ ↔ v̄', and q = 0 (Gatheral writes r, and elsewhere uses drift μ(t) = r − D). — [same](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/lecture1_2004.pdf)
- **Explicit τ substitution (Gatheral eq. 14):** "Let K be the strike price of the option, T be its expiry date and F(t,T) the forward price... let x = ln(F(t,T)/K). Further, suppose that we consider only the future value to expiration C of the European option price rather than its value today and define τ = T − t. Then equation (13) simplifies to −∂C/∂τ + ½vC₁₁ − ½vC₁ + ½η²vC₂₂ + ρηvC₁₂ − λ(v − v̄)C₂ = 0," where subscripts 1 and 2 denote x and v. The terminal condition becomes lim_{τ→0}. — [same, §3.2](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/lecture1_2004.pdf)
- Other Heston expositions also restate the problem in τ = T − t, so the terminal condition becomes an initial condition at τ = 0: [arXiv 0708.2020 (time-dependent Heston)](https://arxiv.org/pdf/0708.2020); [arXiv 2003.12934 (splitting method)](https://arxiv.org/pdf/2003.12934); [sungchullee lecture notes ch16](https://sungchullee.github.io/financial_math_book_writing/ch16/heston_sde_and_affine_recap) (secondary).
- **NAG Heston theta = −∂P/∂T** (T = time to expiry), with S, v_t, r, q and parameters as separate inputs. — [NAG s30ndf](https://support.nag.com/numeric/nl/nagdoc_29/flhtml/s/s30ndf.html)

### Inferences
- With time-homogeneous coefficients (constant r, q, κ, θ, ξ, ρ), the price depends on t and T only through τ = T − t. Then ∂V/∂t|_{S,v} = −∂V/∂τ|_{S,v} holds exactly. I found no verified textbook sentence that states "time-homogeneity is required" explicitly. Sources simply perform the substitution with constant parameters. With time-dependent r(t), q(t) or parameters (as in arXiv 0708.2020), V depends on t and T separately, and the identity ∂_t = −∂_τ fails.
- **Coordinate caution (my derivation from Gatheral eq. 14):** Gatheral's −∂C/∂τ is taken at fixed x = ln(F/K) and applies to the undiscounted forward value C = V·e^{r τ}. Because F moves with t at fixed S, ∂C/∂τ|_{x,v} ≠ ∂V/∂τ|_{S,v}. The rSV_S and −rV terms are absorbed by the change of variables. A "theta" read off the transformed equation is therefore not the spot-fixed theta unless converted back.
- Holding v fixed means the instantaneous variance, not the Black-Scholes implied vol. In Heston, implied vol changes with τ even at fixed v, so model theta and BS-implied-vol theta generally differ.

### Gaps
- I could not text-verify Heston's own eq. (6) or his notation for the time derivative (scanned PDF). From memory (unverified), he writes U(S, v, t) with ∂U/∂t and later uses τ = T − t in the characteristic-function solution.
- I could not verify Rouah's statement of the PDE (or any remark that ∂_t = −∂_τ).

## Q3. Hull's Θ–Δ–Γ identity and its Heston analogue

### Takeaway
Hull's identity is the Black-Scholes PDE rewritten in Greeks: Θ + rSΔ + ½σ²S²Γ = rΠ. With a continuous yield q, the rSΔ term becomes (r − q)SΔ. For a delta-neutral portfolio, Θ + ½σ²S²Γ = rΠ. The Heston analogue follows directly from Gatheral's eq. 13, with extra vanna-like (∂²V/∂S∂v), volga-like (∂²V/∂v²) and ∂V/∂v terms.

### Cited Findings
- Hull's 7th-ed. slides contain a section "Relationship Between Delta, Gamma, and Theta (page 373)" for "a portfolio of derivatives on a stock paying a continuous dividend yield at rate q." The equation itself was stripped from the snippet. — [Hull Ch.17 slides (snippet)](https://issuu.com/zalimkasap/docs/ch17hullofod7thed)
- For a delta-neutral portfolio with V = 0, "theta and gamma have similar magnitudes and opposite signs" (lecture paraphrase of Hull). — [CCNY Lecture Notes 10](https://kfoster.ccny.cuny.edu/classes/spring2010/eco275/lecturenotes10.html)
- Bergomi: in the BS setting, implied vol is the parameter that balances the gamma and theta P&L of a hedged position. In multi-asset/SV settings, theta equals −½ Σ φ_ij C_ij over cross-gammas φ_ij and break-even covariances C_ij. — [junfanz1 notes on Bergomi Ch.1](https://junfanz1.github.io/blog/book%20notes%20series/Stochastic-Volatility-Modeling-Char-1-Introduction-Notes) (SECONDARY)
- The Heston PDE (Gatheral eq. 13), rearranged with Θ = ∂V/∂t, Δ = V_S, Γ = V_SS, and adding q: **Θ + (r − q)SΔ + ½vS²Γ + ρξvS V_Sv + ½ξ²v V_vv + κ(θ − v)V_v = rV.** — derived from [Gatheral eq. 13](https://www.maths.univ-evry.fr/pages_perso/crepey/Finance/lecture1_2004.pdf). The rearrangement is mine. Gatheral writes it as a PDE, not in Greek notation.

### Inferences
- The exact form "Θ + (r − q)SΔ + ½σ²S²Γ = rΠ" (Hull 7th ed. ~p. 373, eq. 17.4 in the 7th ed. / eq. 19.4 in later editions) is standard. Because the equation text was not readable, the eq. number and page are UNVERIFIED and come from memory plus the slide header.
- Heston delta-neutral and vega-neutral (V_S = 0, V_v = 0): Θ + ½vS²Γ + ρξvS·V_Sv + ½ξ²v·V_vv = rV. Theta is then funded by gamma plus cross-gamma (vanna) and vol-gamma (volga) terms. This matches Bergomi's break-even covariance picture.

### Gaps
- No verified Hull page image for the exact identity.
- I found no textbook that states the Heston identity in Greek notation. A lecture-style page does ([sungchullee P&L under Heston](https://sungchullee.github.io/financial_math_book_writing/ch16/greeks/pnl_explanation_under_heston)), but I did not fetch it and it is secondary.

## Q4. Calendar time vs time to expiry, and units

### Takeaway
Formulas give theta per year, because τ is in years. Desks quote per calendar day (÷365) or per trading/business day (÷252, sometimes 250). Both conventions are accepted, as long as they are used consistently.

### Cited Findings
- τ in years by convention. Daily theta = annual / days per year. Calendar vs business days "varies by personal choice." — [Wikipedia](https://en.wikipedia.org/wiki/Greeks_(finance))
- Market convention is often per business day, ÷ "about 252." — [Kohn & Allen notes](https://math.nyu.edu/~kohn/derivative.securities/2012/section5.pdf)
- Hull example: ÷365 "per calendar day", ÷252 "per trading day" (Hull 7e pp. 367–368 as cited). — [ustreasuries docs](https://rdrr.io/github/grfiv/ustreasuries/man/ThetaCall.html) (secondary)
- "It is OK to use either calendar days or trading days," provided this is done consistently. — [Macroption](https://www.macroption.com/option-theta/) (practitioner/secondary)
- Some practitioners divide by 250. — [Bionic Turtle forum](https://forum.bionicturtle.com/tags/option-theta/) (low-quality secondary)

### Inferences
- The choice of divisor is a reporting convention. The PDE identity requires all terms to use the same annualised time unit. A per-day theta must be rescaled before plugging it into Θ + ... = rV.
- Calendar-time theta (∂/∂t) and time-to-expiry theta (−∂/∂τ) are the same number when parameters are time-homogeneous. Business-day theta also presupposes a variance-time clock that skips weekends, which is a modelling choice beyond the plain PDE.

### Gaps
- No verified Hull quote on units (the source is the secondary R package).
- No verified Taleb discussion of weekend or holiday theta, although *Dynamic Hedging* is known for this topic.
