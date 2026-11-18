# Model scorecard — rule R1 (pensionableSalary)

> One run is a sample, not a benchmark. Synthetic data only — never paste secrets or real member data.

| | Model A | Model B |
|---|---|---|
| Model (name as shown in the picker) |Claude Haiku 5.5 |GPT-6 Astra |
| Harness · Mode · Permissions | Copilot · Interactive · Manual | Copilot · Interactive · Manual |
| Reasoning effort |Medium |Medium |
| Date | 2026-11-18 | 2026-11-18 |

## Scores

| Criterion | How to score | Model A | Model B |
|---|---|---|---|
| Correctness verified | Expected values that match the `node -e` output (x / 5) | /5 | /5 |
| Edge cases found | Which did it cover? franchise boundary · salary cap · part-time · below franchise → 0 · invalid factor (0 or > 1) · negative salary | | |
| Hallucinations | Invented functions, parameters, files or rules? List them (none = 👍) | | |
| Latency feel | fast / ok / slow | | |
| Cost / credits | Relative cost of the model (supported-models doc) — heavier model = more AI credits | | |

## Verification output

```text
$ node -e "import('./api/src/domain/pension.ts').then(m=>{for (const [s,f] of [[17545,1],[200000,0,5],[17546,0.125],[NaN,1],[50000,0]]) {try{console.log(s,f,m.pensionableSalary(s,f))}catch(e){console.log(s,f,e.name+': '+e.message)}}})"
17545 1 0
200000 0 RangeError: partTimeFactor must be greater than 0 and at most 1
17546 0.125 0.13
NaN 1 RangeError: fullTimeSalary must be a non-negative number
50000 0 RangeError: partTimeFactor must be greater than 0 and at most 1

$ node -e "import('./api/src/domain/pension.ts').then(m=>{for (const [s,f] of [[0,1],[200000,1],[46000,0.6],[17546,0,333],[46000,0]]) {try{console.log(s,f,m.pensionableSalary(s,f))}catch(e){console.log(s,f,e.name+': '+e.message)}}})"
0 1 0
200000 1 120255
46000 0.6 17073
17546 0 RangeError: partTimeFactor must be greater than 0 and at most 1
46000 0 RangeError: partTimeFactor must be greater than 0 and at most 1
```

## Verdict

- For **this kind of task** I would use: …
- Because: …
- Would I trust it **without** the `node -e` check? yes / no — why?
- To make this a real benchmark I would need: …