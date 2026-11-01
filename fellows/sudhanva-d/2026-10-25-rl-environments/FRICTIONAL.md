# Week 1 Tasks

## 1. RL Environments project update: "Training Ants to Work Together"

Research-stage update: what the literature says about training a colony of ant
agents to forage alone and then together. No training code or results are
claimed in this video.

- 16:9 video: link to be added (render not yet uploaded to Drive)
- 9:16 video: link to be added (render not yet uploaded to Drive)
- Beat sheets: [16:9](ant-head-rl-research/beat_sheet_16x9.json) · [9:16](ant-head-rl-research/beat_sheet_9x16.json)

## 2. Explainer: "How a Quotient Filter Works"

- 16:9 video: link to be added (render not yet uploaded to Drive)
- 9:16 video: link to be added (render not yet uploaded to Drive)
- Beat sheets: [16:9](quotient-filter/beat_sheet_16x9.json) · [9:16](quotient-filter/beat_sheet_9x16.json)

## Task Subfolders

- [ant-head-rl-research](ant-head-rl-research/)
- [quotient-filter](quotient-filter/)

---

# Research Done This Week: RL Environments

**Question.** How should a colony of ant agents be trained to forage, first with
each ant learning alone and then with the policies "loosened" so they cooperate?
The tasks are to find food, bring it home, cut large food into transportable
pieces, and carry those pieces home together, as real ants do. Two worlds are
planned: a grid world and a free (continuous) world.

**Findings.**

1. **Independent learners have a ceiling.** In the main cooperative MARL benchmark,
   independent learning works when an agent can succeed from its local view, but
   extensive coordination under partial observability needs centralised
   information during training (Papoudakis et al., NeurIPS 2021).
2. **Loosening = train together, act alone (CTDE).**
   - VDN: the team value is the sum of per-agent values, Q_tot = Σ_i Q_i (Sunehag et al., 2018).
   - QMIX: a monotonic mixing network on the global state, ∂Q_tot/∂Q_i ≥ 0, so each agent's greedy action is also the team's (Rashid et al., 2018).
   - MAPPO: PPO with a shared, centralised critic (Yu et al., 2022).
   - Parameter sharing improved returns in the benchmark.
3. **Where it breaks.** With very sparse rewards, value decomposition can fail
   outright: on the benchmark's robot-warehouse task, VDN and QMIX did not learn.
   A shared team reward also invites free riders, so the plan measures each ant's
   share of the work, not only the team total.
4. **Environments to build on.** Level-Based Foraging is a grid world where food
   can only be loaded when the agents' combined levels are high enough, which forces
   cooperation by design. The multi-agent particle environments (Lowe et al., 2017)
   provide a continuous world.
5. **The spec from real ants.** Groups carrying a load coordinate through the
   forces they feel in the load itself, and a newly attached ant that knows the way
   can steer the whole group (Feinerman et al., Nature Physics 2018). Depending on
   species and load, ants either cut large prey into pieces or carry it whole
   (cooperative-transport reviews, e.g. McCreery & Breed 2014).

**Plan that follows from it.** Baseline: independent Q-learning (own reward, blind
to teammates). Then VDN, QMIX and MAPPO with parameter sharing. Measure team food
delivered **and** each ant's share. Implementation is next week's work.

**Sources.**

- Papoudakis, Christianos, Schäfer, Albrecht. *Benchmarking Multi-Agent Deep RL Algorithms in Cooperative Tasks.* NeurIPS 2021 Datasets & Benchmarks. https://arxiv.org/abs/2006.07869
- Sunehag et al. *Value-Decomposition Networks for Cooperative Multi-Agent Learning.* AAMAS 2018.
- Rashid et al. *QMIX: Monotonic Value Function Factorisation for Deep MARL.* ICML 2018. https://arxiv.org/abs/1803.11485
- Yu et al. *The Surprising Effectiveness of PPO in Cooperative Multi-Agent Games.* NeurIPS 2022.
- Lowe et al. *Multi-Agent Actor-Critic for Mixed Cooperative-Competitive Environments.* NeurIPS 2017.
- Feinerman et al. *The physics of cooperative transport in groups of ants.* Nature Physics 2018.
- McCreery & Breed. *Cooperative transport in ants: a review of proximate mechanisms.* Insectes Sociaux 2014.

---

# Frictional Log

*Entries written on 2026-11-01 from the working session's record, describing
work done in the week of 2026-10-25.*

## 2026-10-25: Quotient filter explainer

- **Tried and expected.** A short explainer whose every number comes from running
  a real quotient filter rather than from a slide. I expected the film to be about
  the mechanism (quotient picks the slot, remainder is stored, three bits per slot
  keep runs straight).
- **What the measurements said.** At 75% load, 2,929 of 1,000,000 never-seen keys
  got a false "maybe" (0.293%, against 0.292% predicted by 1 − e^(−α/2^r)), and no
  inserted key got a "no". A Bloom filter given the same bits made 844 false
  "maybe"s, and at equal error the quotient filter needs about 21% more memory.
  Its payoff is locality: about 1.09 cache lines per lookup against 9.98. Lookups
  cost 1.5 slots at 50% load and 249 at 95%.
- **Where it resisted.**
  - The first cut was 3.7 minutes, so I compressed it to 9 beats and 76 seconds.
  - One draft line said a Bloom filter "needs 21% less memory". That is wrong (the
    Bloom filter needs about 17% less; the quotient filter needs 21% more), and it
    was corrected before audio.
  - The 9:16 version needed real re-layouts, not crops: an 8-slot table becomes
    2 rows × 4, and the load curve becomes horizontal bars to fit the narrow frame
    and the type-size rules.
- **Claude's contribution and what I kept.** Claude wrote the filter code, the
  measurement script and the visual scenes, and drafted the narration. I approved
  the script, asked for the shorter cut, and kept the measured numbers as the only
  numbers on screen.
- **Still open.** Upload both renders to Drive and add the links above.

## 2026-10-25: RL Environments research update

- **Tried and expected.** A video on training ant agents to forage alone and then
  together. Claude started a small simulator to produce numbers. I stopped it: this
  week is the research stage, and code updates should wait for actual code.
- **What the false start taught.** In that throwaway simulator, independently
  trained ants already scored the maximum, so the world did not require cooperation
  at all. The real environments must force it (heavy pieces, partial views, sparse
  rewards), which is exactly what Level-Based Foraging is designed to do.
- **Where it resisted.**
  - The approved 10-beat script ran too long. I asked for 6 beats, which dropped
    the typed overview and the recap page.
  - Branding: the video now introduces me and names the project "RL Environments"
    in narration, watermark and outro.
  - The synthetic voice blurred "RL" and needed a pronunciation fix ("ar · el"),
    checked by transcribing the audio.
  - Older copies of the renders in a sub-folder still showed the previous watermark
    and had to be replaced.
- **Claude's contribution and what I kept.** Claude searched and summarised the
  benchmark, value-decomposition, MAPPO and ant-transport literature, drafted the
  script and built the visuals. I approved the script and directed the 6-beat cut,
  the persona and the project name. On screen, every diagram is labelled as an
  illustration, and findings are paraphrased with citations rather than quoted.
- **Still open.**
  - Check the exact wording of each cited finding against the full papers before
    publishing.
  - Upload the renders and add links.
  - Implementation starts next week.
