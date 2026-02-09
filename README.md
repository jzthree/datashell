# datashell

Interactive terminal tools for exploring scientific data files. Navigate, visualize, and analyze large datasets without leaving the command line.

## Tools

### ash - Array Shell

An interactive shell for array data files. Supports HDF5, NumPy `.npy`, and `.npz` files. Navigate groups like a filesystem, slice arrays with numpy syntax, and visualize datasets with built-in plotting and an interactive browser.

```
ash data.h5
ash weights.npy
ash archive.npz
```

**Supported formats:**
- **HDF5** (`.h5`, `.hdf5`, `.hdf`) — full group/dataset hierarchy
- **NPY** (`.npy`) — single array, exposed as `data` dataset
- **NPZ** (`.npz`) — multiple arrays, each as a dataset in the root group

**Features:**
- Filesystem-style navigation (`ls`, `cd`, `tree`, `pwd`) with tab completion
- Numpy-style slicing (`cat weights[0:10, :5]`, `head`, `tail`, `peek`)
- Full metadata inspection (`info`, `shape`, `dtype`, `attrs`)
- Statistical analysis (`stats`, `hist`, `unique`)
- Interactive browser (`browse` / `b`) with pan/zoom for 1D signals and 2D heatmaps
- Inline plotting (`plot`) for quick visualization
- Disk usage (`du`) and regex search (`find`)
- Export to `.npy`, `.csv`, `.tsv`

The interactive browser renders pixel-level graphics via sixel protocol (with automatic curses text-mode fallback). 2D datasets are displayed as viridis heatmaps. Only the visible portion of data is loaded per frame via stride slicing, so browsing stays responsive on arbitrarily large arrays.

`h5sh` is provided as a backwards-compatible alias for `ash`.

<!-- ![ash 2D heatmap browser](screenshots/h5sh_heatmap.png) -->

### bwsh - BigWig Explorer

An interactive shell for bigWig genomic signal files. Browse chromosome-wide signal tracks with a built-in genome browser.

```
bwsh signal.bw
```

**Features:**
- Chromosome navigation (`chroms`, `cd`, `pwd`)
- Signal visualization (`view`, `peek`, `browse`)
- Interactive genome browser with sixel pixel rendering and curses fallback
- Pan/zoom with vim-style keys, zoom presets (100bp to 100Mb)
- Theme-aware rendering (auto-detects light/dark terminal)
- Configurable y-axis limits and NaN handling (mean vs nanmean)
- Region statistics, histograms, BED file analysis
- Export to `.npy`, `.bedgraph`, `.tsv`

<!-- ![bwsh genome browser](screenshots/bwsh_browser.png) -->

## Installation

Both tools are standalone Python scripts with minimal dependencies.

**Requirements:** Python 3.7+, numpy

```bash
# ash (array shell — HDF5, npy, npz)
pip install numpy        # h5py optional, needed only for HDF5 files
pip install h5py         # if you work with HDF5
cp ash ~/bin/
chmod +x ~/bin/ash
ln -s ash ~/bin/h5sh     # optional backwards-compatible alias

# bwsh
pip install pyBigWig numpy
cp bwsh ~/bin/
chmod +x ~/bin/bwsh
```

**Optional:** A sixel-capable terminal (iTerm2, WezTerm, foot, xterm) enables pixel-level graphics in the interactive browser. Without sixel support, both tools automatically fall back to curses text-mode rendering.

## Quick Reference

### ash

| Command | Description |
|---------|-------------|
| `ls [-l]` | List contents (long format shows chunks/compression) |
| `cd` / `tree` | Navigate groups |
| `cat ds[slice]` | Print values with numpy slicing |
| `peek ds` | Smart preview (corners + stats for large arrays) |
| `browse ds` | Interactive browser (1D signal or 2D heatmap) |
| `plot ds` | Inline bar chart |
| `stats ds` | Min/max/mean/std/median/percentiles |
| `hist ds` | Text histogram |
| `info ds` | Full metadata (shape, dtype, chunks, compression, attrs) |
| `du` | Disk usage per child |
| `find pattern` | Regex search across all paths |
| `export ds out.npy` | Save slice to file |

### bwsh

| Command | Description |
|---------|-------------|
| `chroms` | List chromosomes with sizes |
| `browse [region]` | Interactive genome browser (alias: `b`) |
| `view region` | ASCII signal plot |
| `peek [chrom]` | Whole-chromosome sparkline |
| `stats region` | Region statistics |
| `hist region` | Signal histogram |
| `ylim [ymin ymax]` | Set y-axis limits |
| `nanmode [on\|off]` | Toggle NaN=0 handling |
| `export region file` | Save to file |

### Browser Keys

| Key | Action |
|-----|--------|
| `h`/`l`, Left/Right | Pan 25% |
| `H`/`L` | Pan 50% |
| `k`/`j` | Zoom in/out |
| Up/Down | Scroll rows (ash 2D) / Zoom (bwsh) |
| `1`-`8` | Zoom presets |
| `g` | Goto position |
| `s` | Stats |
| `n` | Toggle NaN mode |
| `t` | Toggle theme (bwsh) |
| `y` | Set y-axis limits |
| `q` / Esc | Quit |

## License

MIT
