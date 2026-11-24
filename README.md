# SNN accelerator — architecture report

LaTeX source of an IEEE-style architecture report on a spiking neural network
accelerator with three-dimensional convolution and timestep-level early exit.

The report is written in the style of the FireFly papers (TVLSI'23, TCAD'24,
TCAS-I'25): IEEEtran, journal mode, two columns. It starts at Section I; the
abstract, introduction and related-work sections are deliberately absent.

## Layout

```
docs/architecture/
  main.tex              build entry point (title, author, \input of sections)
  preamble-ieee.tex     IEEEtran layout, notation macros, figure/algorithm setup
  01-fundamentals.tex   Section I: neuron model, 3-D spiking convolution,
                        Algorithm 1, cost of the sequential model
figures/
  3DCNN-Architecture.pdf    Fig. 1, exported from the .drawio below
  3DCNN-Architecture.drawio Fig. 1 source (page "3DSCNN")
```

## Build

pdfLaTeX, from `docs/architecture`:

```sh
latexmk -pdf -interaction=nonstopmode main.tex
```

`latexmk` sometimes stops one pass early and leaves `Reference ... undefined`
warnings; one more `pdflatex` pass clears them.

Required TeX packages (all on CTAN): `ieeetran`, `mathptmx`, `amsmath`,
`amssymb`, `graphicx`, `array`, `tabularx`, `booktabs`, `multirow`, `float`,
`enumitem`, `xcolor`, `listings`, `url`, `algorithm`, `algpseudocode`
(packages `algorithms` and `algorithmicx`), `hyperref`.

`newtx` is deliberately not used: `newtxmath` needs `centernot.sty`, which the
TeX Live install used here does not ship.

## Figures

Figures are drawn in draw.io and committed as PDF, so the document builds
without draw.io installed. After editing a `.drawio` file, re-export the page
(1-based index, `--crop` trims to the drawing):

```sh
draw.io --export --format pdf --crop -p 2 \
  --output figures/3DCNN-Architecture.pdf figures/3DCNN-Architecture.drawio
```

Wide figures use `\figplaceholderwide` from `preamble-ieee.tex` (a `figure*` at
`\textwidth`); single-column figures use `\figplaceholder`. Both fall back to a
framed placeholder when the PDF is missing, so the text can be written before
the artwork.

## Notation

| Macro | Renders as | Meaning |
|---|---|---|
| `\Cin` / `\Cout` | $C_\mathrm{in}$ / $C_\mathrm{out}$ | input / output channels |
| `\cin` / `\cout` | $c_\mathrm{in}$ / $c_\mathrm{out}$ | channel indices |

Depth is $D$ (with $D_i$, $D_o$, $D_h$ reserved for sizes), dilation is
$\Delta$, so the two never collide. Tensors are written as shapes:
$S(T,C_\mathrm{in},D_i,H_i,W_i)$, $W(C_\mathrm{out},C_\mathrm{in},K_d,K_h,K_w)$,
$O(T,C_\mathrm{out},D_o,H_o,W_o)$.

## Not in this repository

Reference corpora and source papers are kept locally and ignored:

- `Fewshot/` — figure corpus (read-only, includes a nested clone)
- `XiangShan-Design-Doc/`, `FireFly-v2/`, `systolic-array/`, `Tutorial/`
- the FireFly papers as PDF and as extracted text
- `docs/source/` — a separate XeLaTeX design document on the same IP
- `docs/firefly-v2/` — design-doc notes