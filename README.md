# eval-salad-bench

**SALAD-Bench: Hierarchical Safety Benchmark (Multiple-Choice)**

> ⚠️ **Third-party eval.** This is a `register/` pointer in inspect_evals — the task code lives in an external repository of unaudited provenance and will execute on OpenEvalz infrastructure. Onboarding it is a security review, not a packaging task.

**Paper:** https://arxiv.org/abs/2402.05044

The multiple-choice half of SALAD-Bench, a hierarchical LLM safety benchmark spanning
6 top-level safety domains, 16 task categories and 65 fine-grained categories. Each of
the 3,832 scorable questions pairs a potentially harmful query with three candidate
answers, and the model must select the safe option(s). One or two of the three may be
safe, so the gold label is a set and scoring is an exact set match: selecting only one
safe option when two are safe is wrong, as is including an unsafe option. Scoring is
deterministic and needs no judge model, using SALAD-Bench's regex MC-Judge over the
`<ans>...<eoa>` envelope (as distinct from the model-based MD-Judge used for the
free-form QA sets). Reports accuracy and stderr overall, plus accuracy grouped by
top-level safety domain.

## At a glance

| | |
|---|---|
| Upstream | [`register/salad-bench`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/register/salad-bench) |
| Group | — |
| Total samples | 0 |
| Execution class | `plain` |
| Cost class | `low` |
| Flags | no sandbox, no network |
| Tags | Safety |

### Tasks

| Task | Samples |
|---|---|
| `salad_bench_mcq` | 0 |

### External assets

_None declared upstream._

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/salad_bench_mcq \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
