# latentreasoning

Source for **[latentreasoning.net](https://latentreasoning.net)** —
a curated map of research on language models that reason in continuous latent space instead of
emitted tokens.

## The organising idea

Autoregressive decoding runs a loop:

```
h_t = f(e_1..e_t)          # final-layer hidden state
x_t ~ softmax(W h_t)       # <- the quantiser
e_{t+1} = E[x_t]
```

The middle line is a **quantiser**: it rounds a `d`-dimensional state down to one of `|V|` tokens,
about 17 bits. Latent reasoning is what happens when you delete it (`e_{t+1} = h_t`), or when you
close the loop inside the layer stack instead of around it.

The consequence that organises the site: under token-space decoding the complete internal state at
step `t` is a deterministic function of the emitted prefix — **the transcript is a sufficient
statistic for the computation**. That is what makes a chain of thought legible in principle, and
exactly what latent reasoning gives up.

## Pages

| Page | Contents |
|---|---|
| `index.html` | The thesis, the state of play, and the full bibliography |
| `intro.html` | The quantiser, the sufficient-statistic property, the two axes |
| `families.html` | Six families of methods, and what is known to fail |
| `theory.html` | Expressivity results, with complexity classes stated properly |
| `monitorability.html` | What oversight is lost, how much existed, the deployment question |
| `implementations.html` | Code, open checkpoints, benchmarks |
| `map.html` | d3 force-directed literature map, with dashed edges for verified absences |
| `timeline.html` | d3 swimlane timeline, 53 events across four strands |

Plain static HTML with one shared `style.css`. No build step. KaTeX and d3 from CDN.

## Editorial rules

- Every arXiv identifier was resolved against the arXiv and Semantic Scholar APIs; every GitHub and
  Hugging Face URL was fetched. Repository statistics are as of September 2026.
- Papers are cited with the result that matters, including the ones that cut against the method.
  Coconut's GSM8K number appears next to its ProsQA number.
- Rebuttals are first-class. This field has an unusual number of them and several headline
  mechanisms did not survive replication.
- Dashed edges on the literature map are claims about absences, so each is written out with a
  confidence level on that page.
- Claims sourced from news reporting rather than primary documents are marked as such.

## Contributing

Open an issue or a PR. Useful contributions: a paper that belongs in the bibliography, a
correction to a date or a number, a gap edge that has since been closed, or a repository whose
maintenance status has changed.

## Deployment

GitHub Pages from the default branch, root directory. `CNAME` points at
`latentreasoning.net`.

## License

MIT — see `LICENSE`.
