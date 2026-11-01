# Secnd

Secnd is an AI lab building models that decide or ask for help.

A decision model takes a situation and a list of options, then returns one option with a
probability. To trust software to act alone, you need proof of when it's safe. So we measure how
many decisions a model can handle alone, with proof, per dollar.

**Status:** Ren is public. The Secnd product isn't built, and it has no users or revenue. No real
business has run Ren yet.

## Ren

Ren is our first model. It has 396M parameters and it's trained once. It can work from option names
alone and gets better with a few examples.

- **Weights:** [`Secnd-AI/ren` at `v1.1.0`](https://huggingface.co/Secnd-AI/ren/tree/v1.1.0), under CC BY-SA 4.0
- **Training:** 422,906 rows from 38 sets, 12 licensed public sets and 26 written ones, for about $129 of compute

## Results

| Test | Ren | Comparison |
| --- | ---: | --- |
| Typed-decisions, mixed test | 79.7% | OpenDecider-nano 79.6%; OpenDecider's run of Jev 1.13 75.4% |
| BANKING77, no examples | 69.0% | Jev 79.7%, from a published study |
| BANKING77, with examples | 89.9% with 8 | Jev 92.4% with 24, from the same study |
| MTOP, no examples | 76.0% | |
| MTOP, 8 examples | 92.4% | |

**Acting alone, at our 99% gate:**

- **MTOP:** Ren answered 21.0% alone and made 1 error in 923 answers. Its lower confidence bound is 99.40%.
- **BANKING77:** Ren answered 0% alone, because it couldn't prove it was safe. That gap is the research problem we're working on.
- **Limits:** the result covers one seed with full history, and it doesn't certify a new business. When the right option was missing, Ren wrongly accepted 2 in 500, above its target of 1.

**Speed and cost:**

- **GPU:** 26 ms, not counting the network.
- **Through the endpoint:** 424 ms p50 from India to one A10G in the US.
- **Cost:** $0.083 per 1,000 decisions at full load. Jev works out to about $0.008.

Sources: [training pools](docs/results/s2/wide-round.md), [gate](docs/results/s3r/README.md),
[load test](docs/results/s8/load-gpu-secnd-ren-1.json).

## Secnd

Secnd is our first product. It will connect to the tools where work happens, starting with GitHub,
then Slack, email and Notion. It will learn how you decide. When it's sure, it acts for you. When it
isn't, it asks. With consent, real decisions from Secnd will train the next Ren.

## Next

1. Build the GitHub connector.
2. Test Ren against Jev on real decisions.
3. Close the BANKING77 gap, so Ren can prove more of its answers are safe.

## Develop

This repository is a snapshot that still carries older application code, which will be removed. The
model code is in `backend/incontext.py`, `backend/typed.py` and `backend/reader_serving.py`.

With Python 3.12, [uv](https://docs.astral.sh/uv/) and Node.js 24:

```sh
uv sync --locked
npm ci
npm run check
```

Keep secrets, uploaded data, model files and caches out of Git. Keep calibration and test data out
of training.

- [Model documentation](docs/model.md)
- [Serving configuration](docs/serving.md)
- [Evaluation results](docs/results/)
