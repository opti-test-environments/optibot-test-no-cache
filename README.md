# optibot-test-no-cache

OptiBot test repository — Node.js project without dependency caching.

## Workflow

Workflow (`ci.yml`) with two independent jobs (no needs chain):
- **build**: checkout, setup-node (NO cache), npm install, test, build
- **lint**: checkout, setup-node (NO cache), npm install, lint

## Intentional Inefficiencies

| # | Inefficiency | Details |
|---|-------------|---------|
| 1 | No dependency cache | Both jobs run `npm install` without cache |
| 2 | No concurrency cancellation | Overlapping runs waste resources |
| 3 | Broad triggers | push/PR to main, no path filters |

## Expected OptiBot Behavior

| Strategy | Detection | Static Analysis | Experiment | Expected Result |
|----------|-----------|----------------|------------|----------------|
| Add dependency cache | Detected | Needs experiment | 3 CI runs | Pass if >=10% improvement |
| Concurrency cancellation | Detected | Sufficient (skip) | Skipped | Auto-pass |
| Trigger narrowing | Detected | Needs experiment | 1 CI run | Pass if CI succeeds |

## Expected Final State

- 3 discoveries
- 1 optimization PR (cache if >=10%, else concurrency)

