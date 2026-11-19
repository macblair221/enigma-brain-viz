# ENIGMA Brain Explorer

An interactive 3D brain that shows how brain structure differs across 11 brain conditions, built from the published summary statistics in the [ENIGMA Toolbox](https://github.com/MICA-MNI/ENIGMA).

Open `index.html` in a browser, or turn on GitHub Pages for this repo (Settings → Pages → deploy from `main`, root) to get a live link. It is a single self-contained page; the only things it loads from the web are three.js, three-mesh-bvh and Google Fonts.

## What it shows

- **The brain:** pick a condition and the brain lights up. Blue areas are smaller than usual in people with that condition, orange areas are bigger, and dark areas show no big difference. Brighter means a bigger difference.
- **Point at an area** to see its name, what it helps with, and how much it changed ("a little", "noticeably" or "much" smaller or bigger).
- **See inside** shows the deep structures, like the hippocampus and amygdala.
- **Compare:** a bar chart of how many of the 84 brain areas are different in each condition. Click a bar to see that condition on the brain.

Conditions: 22q11.2 deletion syndrome, ADHD, anorexia, antisocial behavior, autism, bipolar disorder, depression, epilepsy, OCD, Parkinson's disease and schizophrenia.

The page is written so someone with no background (think middle school) can follow it. The raw numbers are still there in small print and in the "For scientists" section at the bottom.

## How the numbers are used

- Effect sizes are Cohen's d from the toolbox's `d_icv` column (case vs control).
- A difference is "reliable" if it passes FDR correction (q < .05). Where only uncorrected p-values were released (Parkinson's, and deep structures for ADHD and depression), p < .05 is used.
- An area only lights up (and counts in the bar chart) if the difference is reliable **and** at least small (|d| >= 0.2), since tiny effects reach significance easily in samples this large.
- "A little", "noticeably" and "much" mean |d| of 0.2 to 0.5, 0.5 to 0.8, and 0.8 or more.
- Epilepsy is all types combined. The page shows cortical thickness and subcortical volume.
- These are group averages. They say nothing about any individual brain and can't be used for diagnosis.

## Rebuilding from the source data

```bash
pip install -r requirements.txt
git clone --depth 1 https://github.com/MICA-MNI/ENIGMA.git
mkdir -p build/out
python build/extract.py ENIGMA/enigmatoolbox/datasets build/out
python build/assemble.py build/out index.html
```

`extract.py` picks the main case-vs-control table for each condition, parses the stats, and exports the fsaverage5 cortex and subcortical meshes. It also splits triangles on area borders so outlines are smooth, simplifies the subcortical meshes, and bakes a sulcal-depth shading map. `assemble.py` drops the data and mesh into `build/template.html`.

## Sources

All statistics come from the ENIGMA working group papers bundled in the ENIGMA Toolbox (Lariviere et al., 2021, *Nature Methods*). The full citation list for each condition is in the page's "About the data" panel. The ENIGMA Toolbox is released under the BSD 3-Clause license.
