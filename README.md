# Chemotactic Creative Ideation

**A Run-and-Tumble Model for Autonomous Parameter Discovery in Creative Systems**

*Timmy A Ostgaard*
*March 20, 2026*

---

## Abstract

Creative systems with tunable parameters face a fundamental polarization problem: when multiple dimensions of creative behavior are compressed onto a single control axis, the system is forced into false tradeoffs between orthogonal qualities. A high setting on a "creativity slider" implies low analytical rigor, when in reality many valuable ideas demand both simultaneously.

We propose a two-dimensional wandering model based on bacterial chemotaxis — the run-and-tumble algorithm with chemotactic gradient bias — as a mechanism for autonomous parameter discovery in creative systems. A point drifts continuously through a 2D parameter disc via smooth interpolation toward randomly spawning waypoint targets. Output quality acts as a chemical gradient signal: productive regions bias future target placement, while unproductive regions trigger purely random reorientation. The result is a system that discovers its own optimal operating parameters through reward-biased stochastic exploration, without prescribed convergence or manual tuning.

This model synthesizes six established research threads — cognitive foraging, creative foraging, Lévy flights in memory, dopaminergic exploration-exploitation switching, chemotaxis-to-cognition mapping, and chemotactic origins of spatial representation — into a unified computational framework. Each thread has been developed independently; no prior work combines them into a working model for creative parameter search.

---

## 1. The Polarization Problem

Creative systems — whether generating music, visual art, prose, or engineering solutions — typically expose tunable parameters that control the character of their output. A common pattern is the single slider: one axis running from "conservative" to "creative," or from "structured" to "freeform."

The problem is that many valuable creative dimensions are **orthogonal**, not opposed. Consider a system that generates ideas and then researches their feasibility:

- **Creative freedom** controls how far the system ranges from conventional thinking
- **Grounding depth** controls how thoroughly the system validates and contextualizes ideas

A single slider coupling these dimensions forces a diagonal: more creative freedom means less grounding, and vice versa. But some of the most valuable ideas require **both extremes simultaneously** — a wildly unconventional concept that is also deeply researched and contextualized. The slider cannot express this.

This is not a UI problem. It is a **dimensional collapse** problem. When *N* orthogonal dimensions are projected onto one axis, the system can only explore a one-dimensional subspace of the full parameter volume. Entire quadrants of productive behavior become unreachable.

---

## 2. The 2D Parameter Disc

The solution is straightforward: decouple the dimensions.

Instead of a single slider from 0 to 100, define a **two-dimensional disc** where each axis is independent:

```
                    High Grounding (100)
                          │
          Q2              │              Q1
     Wild + Grounded      │      Focused + Grounded
                          │
   ───────────────────────┼───────────────────────
                          │
          Q3              │              Q4
     Wild + Ungrounded    │      Focused + Ungrounded
                          │
                    Low Grounding (0)

   Low Freedom (0) ──────────────── High Freedom (100)
```

**Q1** (high freedom, high grounding): Novel ideas with deep research backing. The "CRISPR smart contacts" quadrant — ideas that sound wild but have legitimate scientific grounding.

**Q2** (low freedom, high grounding): Careful, well-researched incremental work. Reliable, well-validated.

**Q3** (low freedom, low grounding): Quick conventional responses. Minimal exploration, minimal validation.

**Q4** (high freedom, low grounding): Pure brainstorming. High novelty, no reality check. The "shower thoughts" quadrant.

All four quadrants are now reachable. But this raises a new question: **who controls where in the disc the system operates?**

Manual control defeats the purpose — the human operator would need to know the optimal position in advance. What we need is a mechanism that **discovers productive regions autonomously**.

---

## 3. The Wandering Algorithm

A point exists on the parameter disc. It represents the system's current operating parameters. It moves continuously, and wherever it happens to be at the moment of evaluation determines the system's behavior for that cycle.

The motion works as follows:

1. A **target waypoint** spawns at a random position on the disc
2. The point **interpolates** smoothly toward the target:

   ```
   position += α × (target − position)
   ```

   where α is a chase rate (0 < α < 1), typically 0.15–0.30

3. **Before the point arrives**, a new random target spawns
4. The point redirects toward the new target
5. The process repeats every cycle (e.g., every 60 seconds)

The point **never reaches its target**. It is always mid-flight, always in transition. Its trajectory through the disc is a smooth, meandering curve — not a sequence of discrete jumps.

This is a **first-order exponential chase** (signal processing), a **seek behavior** (Craig Reynolds' steering behaviors), or equivalently, a **low-pass filtered random walk**. The smoothing prevents erratic jumps while the random target spawning ensures perpetual exploration.

At any moment, the point's position defines the system's creative parameters. Over time, the trail of positions traces out a wandering exploration of the full parameter space.

---

## 4. Chemotactic Gradient Bias

The wandering described above is purely random — it has no notion of "better" or "worse" regions. To make it adaptive, we add a **gradient signal**.

After each evaluation cycle, the system measures output quality using any scalar reward signal (e.g., a quality score, user rating, downstream success metric). This reward biases the placement of the **next** target waypoint:

- **Good outcome** (reward above threshold): The next target is biased toward the current position. The point continues exploring the productive region.
- **Bad outcome** (reward below threshold): The next target is placed purely at random. The point tumbles to a new region.

This is precisely **bacterial chemotaxis**:

| Bacterium | Creative System |
|-----------|----------------|
| Swim in a straight line (run) | Interpolate toward current target (seek) |
| Randomly reorient (tumble) | New random target spawns |
| Favorable chemical gradient → suppress tumbling | Good output quality → bias next target toward current region |
| Unfavorable gradient → increase tumbling | Bad output quality → fully random next target |
| Chemical concentration | Output quality score |
| Flagellar motor switch | Target placement function |

In *E. coli*, the chemotactic bias is implemented through the **CheY signaling pathway**: when the bacterium detects an increasing concentration of attractant, it suppresses the probability of tumbling, causing longer runs up the gradient. When concentration decreases, tumbling frequency increases, randomizing direction.

The creative system implements the same logic: reward biases the target distribution toward productive regions, while absence of reward restores full randomness. The system **climbs reward gradients without any explicit optimization** — it simply tumbles less when things are going well.

---

## 5. Anti-Polarization Mechanisms

Pure chemotaxis can get stuck. A bacterium in a nutrient-rich pocket may never leave. Three mechanisms prevent this:

### 5.1 Elastic Restoring Force

When the point drifts near the edge of the disc (extreme parameter values), a restoring force pulls it back toward the center:

```
if distance_from_center > threshold:
    position += β × (center − position)
```

This prevents lock-in at extremes and ensures the system can always return to moderate parameter regions.

### 5.2 Momentum Decay

Without continued positive reward, the chemotactic bias decays over time. A productive region discovered 20 cycles ago does not continue to attract the point indefinitely. The system must **keep earning** its bias through ongoing good outcomes.

### 5.3 Orthogonal Jolts

Periodically, the system forces a target spawn in the **opposite quadrant** from the current position. This is a deliberate probe — even if the current region is productive, the system occasionally checks whether a dramatically different region might be better.

This mirrors the biological observation that creative individuals leave productive patches **earlier than optimal foraging theory predicts** (Hart et al., 2017) — premature switching serves long-term exploration even at the cost of short-term efficiency.

---

## 6. Formal Properties

The algorithm has several properties that distinguish it from conventional optimization:

**The randomness never dies.** Unlike simulated annealing or gradient descent, the noise scale does not decrease over time. The system maintains perpetual exploration capacity regardless of how long it has been running. There is no convergence schedule.

**Convergence is emergent, not prescribed.** The system has no target state. If a productive region exists, the chemotactic bias will cause the point to spend more time there — but this is a statistical tendency, not a programmed destination. The system discovers its attractors rather than being told where they are.

**Self-rediscovery.** Because randomness persists, the system can escape any region — including regions it previously found productive. If the reward landscape changes (new evaluation criteria, different problem domain), the system automatically re-explores. A single lucky random target ("wild blink") can beat an entire history of settled behavior.

**Human override layer.** An operator can interact with the algorithm at two levels:
- **Soft bias**: Shift the target distribution toward a preferred region (30% pull per cycle). The system still wanders but with a directional preference.
- **Hard pin**: Freeze the point at a specific position. The system stops wandering and operates with fixed parameters. Useful when the operator already knows what they want.

---

## 7. Research Lineage

This model did not emerge from theory — it was discovered through engineering, then recognized as a known biological algorithm. The following six research threads independently established the components that this model synthesizes.

### 7.1 Cognitive Foraging Theory

Thomas T. Hills (University of Warwick) established that humans search internal mental representations using the same explore-exploit dynamics that animals use to forage in physical space. In experiments where participants searched semantic memory (e.g., "name all animals you can think of"), retrieval patterns showed local-then-global transitions consistent with the **marginal value theorem** from optimal foraging theory — people exploit a semantic cluster until it depletes, then switch to a new region (Hills, Jones & Todd, 2012; Hills et al., 2015).

This is the run-and-tumble pattern at the cognitive level: exploit a local gradient (run), then reorient to a new region (tumble).

### 7.2 Creative Foraging

Yuval Hart and colleagues at the Weizmann Institute designed a "creative foraging game" where participants explored a combinatorial shape space. Players showed alternating phases of **meandering exploration** (paths ~3x longer than shortest routes) and **focused exploitation** (near-optimal paths within shape categories). Critically, players left productive patches **far before depletion** — unlike optimal foraging predictions — suggesting that creative search involves **premature switching** that sacrifices short-term efficiency for long-term discovery (Hart et al., 2017).

Their 2024 follow-up demonstrated that divergent and convergent creativity correspond to different foraging strategies — people who searched divergently in physical space also scored higher on divergent thinking tasks (Hart et al., 2024).

### 7.3 Lévy Flights in Memory Retrieval

Rhodes and Turvey (2007) showed that time intervals between memory retrievals follow a **Lévy distribution** — a pattern of many short intervals punctuated by occasional long gaps. The closer the exponent approximated the theoretically optimal Lévy flight value of 2, the more efficient the retrieval.

Lévy flights are a mathematical cousin of run-and-tumble: both produce the same mixture of local exploitation and long-range jumps, but through continuous power-law distributions rather than discrete state switching.

### 7.4 Dopamine as the Tumble Signal

The neurotransmitter dopamine directly modulates the exploration-exploitation tradeoff in the brain. **Tonic dopamine** sets the threshold: high levels favor exploitation (keep running), low levels favor exploration (tumble). **Phasic dopamine** encodes reward prediction errors — the gradient signal. The basal ganglia acts as the action selection mechanism, with dopamine adjusting its selectivity (Humphries et al., 2012).

This is functionally analogous to the CheY signaling pathway in *E. coli*: a chemical signal that modulates the probability of switching between run and tumble states based on environmental feedback.

### 7.5 Chemotaxis → Cognition (Explicit Argument)

Nathaniel Travis (2022) directly argued that human behavior is "elaborate running and tumbling." The dopamine circuit plays a navigational role directly analogous to chemotaxis: bacteria sense chemical gradients passively; humans determine expected rewards through a predictive world model. But the underlying algorithm — run toward reward, tumble when the gradient flattens — is scale-invariant from bacterium to human.

### 7.6 Chemotaxis → Cognitive Map (Evolutionary Path)

Lucia Jacobs (UC Berkeley) argued that spatial representation evolved as an **exaptation** from the primitive building blocks of chemotaxis and chemoreception. The hippocampal cognitive map — the brain's spatial navigation system — is a descendant of the chemotactic gradient-sensing apparatus (Jacobs, 2012). This provides an evolutionary mechanism linking bacterial gradient sensing to the spatial search strategies observed in creative cognition.

---

## 8. The Gap

Each of these six threads was developed independently:

- **Cognitive foraging** established that mental search uses foraging dynamics — but did not propose a computational model for creative parameter tuning
- **Creative foraging** documented run-and-tumble behavior in creative search — but studied human behavior, not system design
- **Lévy flights** characterized the statistical signature of memory search — but as descriptive analysis, not a generative algorithm
- **Dopamine research** identified the neurochemical mechanism for exploration-exploitation switching — but did not connect it to creative system design
- **Travis** made the explicit chemotaxis-to-cognition argument — but as a conceptual essay, not an algorithm
- **Jacobs** traced the evolutionary path from chemotaxis to spatial cognition — but in evolutionary neuroscience, not computational creativity

**No published work unifies these threads into a computational model for autonomous creative parameter discovery.** This paper proposes that synthesis: bacterial chemotaxis — run-and-tumble with gradient bias — implemented as a 2D wandering algorithm with reward-driven target placement, serves as a formal and practical framework for creative systems that must discover their own optimal operating parameters.

---

## 9. Discussion

### Origin

This model was not derived from the literature. It emerged from an engineering constraint: a self-hosted AI system needed to balance creative freedom against analytical grounding, and a single slider kept forcing false tradeoffs. The 2D disc was the obvious fix for dimensional collapse. The wandering algorithm was the obvious fix for manual tuning. The reward bias was the obvious fix for random-only exploration. Only after implementation did the author recognize the algorithm as bacterial chemotaxis — a convergence that the six research threads above make unsurprising in retrospect.

### Generality

The model is not specific to any particular creative system. It applies wherever a system has:

1. **Two or more tunable parameters** that influence creative output
2. **A scalar quality signal** that can be measured after each evaluation cycle
3. **No known optimal parameter setting** that works across all inputs

Potential application domains include:

- **Music generation**: Balancing melodic novelty against harmonic coherence
- **Visual art systems**: Balancing stylistic freedom against compositional structure
- **Game AI**: Balancing unpredictability against strategic coherence
- **LLM prompt tuning**: Balancing temperature/creativity against factual accuracy
- **Drug discovery**: Balancing molecular novelty against synthetic feasibility
- **Evolutionary algorithms**: Adaptive mutation rate control in two or more dimensions

### Limitations

The model as described operates in two dimensions. Many creative systems have higher-dimensional parameter spaces. Extension to *N*-dimensional hyperspheres is straightforward in principle — the algorithm generalizes naturally — but the anti-polarization mechanisms (elastic restoring force, orthogonal jolts) require more careful design as dimensionality increases.

The reward signal must be scalar and reasonably timely. Systems where quality assessment is delayed by hours or days (e.g., waiting for human feedback) will have slower adaptation. Systems with multi-objective quality metrics require a scalarization function, which introduces its own design choices.

### Future Work

- **Multi-agent swarm variant**: Multiple points wandering the same disc, each with independent targets but shared reward signal. Analogous to bacterial swarm chemotaxis. Could enable parallel exploration of the parameter space.
- **Higher-dimensional discs**: Extension to 3D or *N*-dimensional parameter spaces with appropriate anti-polarization mechanisms.
- **Adaptive chase rate**: The interpolation rate α could itself be a function of reward history — faster chase (more decisive movement) when reward signal is strong, slower chase (more meandering) when exploring unknown territory.
- **Cross-system transfer**: Using the discovered productive regions from one creative task to initialize exploration on a related task.

---

## Sources

1. Hills, T. T., Jones, M. N., & Todd, P. M. (2012). Optimal foraging in semantic memory. *Psychological Review*, 119(2), 431–440. https://pubmed.ncbi.nlm.nih.gov/22329683/

2. Hills, T. T., Todd, P. M., & Jones, M. N. (2015). Foraging in semantic fields: How we search through memory. *Topics in Cognitive Science*, 7(3), 513–534. https://onlinelibrary.wiley.com/doi/10.1111/tops.12151

3. Todd, P. M., & Hills, T. T. (2020). Foraging in mind. *Current Directions in Psychological Science*, 29(3), 309–315. https://journals.sagepub.com/doi/10.1177/0963721420915861

4. Hills, T. T., Todd, P. M., Lazer, D., Redish, A. D., & Couzin, I. D. (2015). Exploration versus exploitation in space, mind, and society. *Trends in Cognitive Sciences*, 19(1), 46–54. https://pubmed.ncbi.nlm.nih.gov/25487706/

5. Hart, Y., Goldberg, A., Mayo, A. E., et al. (2017). Creative foraging: An experimental paradigm for studying exploration and discovery. *PLOS ONE*, 12(8), e0182133. https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0182133

6. Hart, Y., et al. (2024). Divergent and convergent creativity are different kinds of foraging. *Nature Communications*. https://pubmed.ncbi.nlm.nih.gov/38713456/

7. Rhodes, T., & Turvey, M. T. (2007). Human memory retrieval as Lévy foraging. *Physica A*, 385(1), 255–260. https://www.sciencedirect.com/science/article/abs/pii/S0378437107007509

8. Abbott, J. T., Austerweil, J. L., & Griffiths, T. L. (2015). Random walks on semantic networks can resemble optimal foraging. *Psychological Review*, 122(3), 558–569. https://cocosci.princeton.edu/papers/JAbbottRando.pdf

9. Humphries, M. D., Khamassi, M., & Gurney, K. (2012). Dopaminergic control of the exploration-exploitation trade-off via the basal ganglia. *Frontiers in Neuroscience*, 6, 9. https://www.frontiersin.org/journals/neuroscience/articles/10.3389/fnins.2012.00009/full

10. Jacobs, L. F. (2012). From chemotaxis to the cognitive map: The function of olfaction. *Proceedings of the National Academy of Sciences*, 109(Supplement 1), 10693–10700. https://pmc.ncbi.nlm.nih.gov/articles/PMC3386877/

11. Travis, N. (2022). Is human behavior just elaborate running and tumbling? https://nathanieltravis.com/2022/01/17/is-human-behavior-just-elaborate-running-and-tumbling/

12. Reynolds, C. W. (1987). Flocks, herds and schools: A distributed behavioral model. *ACM SIGGRAPH Computer Graphics*, 21(4), 25–34. https://dl.acm.org/doi/10.1145/37402.37406

---

## License

This work is licensed under [CC-BY-4.0](LICENSE). You are free to share and adapt this material for any purpose, including commercial use, as long as you give appropriate credit.

The algorithm described in this paper cannot be patented or copyrighted — algorithms are mathematical ideas. This paper's text is copyrighted and licensed under CC-BY-4.0 for open sharing with attribution.
