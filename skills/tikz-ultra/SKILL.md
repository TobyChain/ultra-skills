---
name: "tikz-ultra"
description: "Create, revise, compile, debug, and export diagrams whose required source of truth is TikZ/PGF/PGFPlots or LaTeX. Invoke when the user explicitly requests TikZ, PGF, PGFPlots, a .tex file, a tikzpicture fragment, LaTeX-native figures, commutative diagrams in LaTeX, modification of existing TikZ code, or reproducible compilation within a LaTeX project. Do not invoke merely because the subject is a paper figure, flowchart, architecture, network, plot, or 3D drawing; when no LaTeX-native source is required, route visual-file and image deliverables to figure-ultra."
---

# TikZ Ultra

Create publication-ready TikZ/PGF figures as reproducible LaTeX source. Prefer
semantic structure, stable geometry, and successful compilation over decorative
complexity.

## Boundary with figure-ultra

Choose by required source format, not by figure topic:

- Use this skill when `.tex`, `tikzpicture`, TikZ/PGF/PGFPlots, LaTeX-native reuse, or TikZ compilation is required.
- Use `figure-ultra` when the desired editable source is `.drawio` or `.excalidraw`, when the primary result is an image, when Matplotlib should render supplied data, or when the user asks generically to draw without selecting LaTeX.
- If the user requests both `.tex` and PNG/SVG/PDF, use this skill once: compile the TikZ source and export from the verified PDF.
- Use both skills only when the user explicitly requires two independent editable source formats.

## Required Reference

Read [references/tikz-reference.md](references/tikz-reference.md) before writing
or modifying TikZ. Consult
[assets/tikz-cheatsheet.pdf](assets/tikz-cheatsheet.pdf) when the compact
reference does not cover a requested primitive or when a visual example is
needed.

## Workflow

1. Identify the figure type, output format, target document class, dimensions,
   language, and any publisher constraints.
2. Choose the smallest required library set from the reference.
3. Plan named nodes, coordinates, layers, styles, and connection routing before
   writing paths.
4. Generate a self-contained `.tex` file unless the user explicitly requests
   only a `tikzpicture` fragment.
5. Compile with `latexmk -pdfxe` for Chinese text, `pdflatex` for plain English,
   or `lualatex` when graph drawing requires Lua.
6. Fix compilation errors before visual polishing.
7. Inspect the rendered PDF at final display size. Check clipping, overlaps,
   labels, arrowheads, line weights, and whitespace.
8. Export PNG or SVG only after the PDF is correct.

## Output Contract

- Deliver editable `.tex` source.
- Keep library declarations explicit and minimal.
- Use named styles for repeated visual roles.
- Use named coordinates and nodes instead of unexplained numeric routing.
- Keep labels readable at the final insertion size.
- Report the compiler and export commands that were run.
- If compilation tools are unavailable, state that validation was not run.

## Figure Routing

| Request | Preferred construction |
| --- | --- |
| Flowchart or pipeline | `positioning`, `shapes.geometric`, orthogonal paths |
| Architecture or network | named nodes, `fit`, `backgrounds`, explicit layers |
| State machine | `automata`, `positioning`, `arrows.meta` |
| Tree or hierarchy | `child` syntax or `graphs`; `mindmap` when requested |
| Commutative diagram | `matrix` plus labeled edges |
| Function or statistical plot | `pgfplots`; use pure TikZ only for small fixed data |
| Neural network | loop-generated layers and connections |
| Geometry | `calc`, `intersections`, `angles`, `quotes` |
| Repeated symbols | define a `pic` or reusable style |
| 3D schematic | custom x/y/z vectors, `tikz-3dplot`, or `pgfplots` |
| Cross-picture annotation | `remember picture,overlay`; compile twice |

## Non-Negotiable Rules

- End every path, including `\node`, with `;`.
- Load every non-core feature with `\usetikzlibrary{...}`.
- Do not assume `scale` changes text or line widths. Add `transform shape` only
  when the entire figure should scale.
- Distinguish nodes from coordinates: node-to-node edges stop at node borders.
- TikZ trigonometric functions use degrees; append `r` for radians.
- Wrap option values containing commas in braces.
- Treat loop and math macros as scope-local.
- Use `overlay` intentionally because it does not affect the bounding box.
- Use XeLaTeX or LuaLaTeX with `ctex`/`xeCJK` for Chinese.
- Keep text concise; move long explanations into captions or document prose.

## Minimal Standalone Template

```latex
\documentclass[tikz,border=2mm]{standalone}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}

\begin{document}
\begin{tikzpicture}[
  node distance=8mm and 12mm,
  box/.style={draw,rounded corners=2pt,inner sep=4pt},
  flow/.style={-{Stealth},thick}
]
  \node[box] (input) {Input};
  \node[box,right=of input] (output) {Output};
  \draw[flow] (input) -- (output);
\end{tikzpicture}
\end{document}
```

## Verification

Before handoff:

1. Compile from a clean auxiliary state.
2. Confirm there are no undefined control sequences or missing libraries.
3. Confirm every edge source and target exists.
4. Check that labels do not cross nodes or arrows.
5. Check the bounding box and surrounding whitespace.
6. Recompile twice when using references, overlays, or externalization.
7. Confirm exported raster images use at least 300 DPI for print.
