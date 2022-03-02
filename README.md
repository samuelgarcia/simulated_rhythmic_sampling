# simulated_rhythmic_sampling

This repository contains all the code used to run the analyses and produce the figures for the paper:
> "Putative rhythms in attentional switching can be explained by aperiodic temporal structure." (In prep.)


## Requirements

- Python 3
- External Python dependencies are listed in `requirements.txt`.
- `mtspec` must be installed using conda: `$ conda install -c conda-forge mtspec`
- The analyses in Fig. 6 require R and the *rcompanion* library.
- To re-run the simulations, you can either run them one at a time (which will be very slow), or in parallel on a computing cluster. To use a cluster, you'll need to make sure the `sbatch` job scheduler in installed, as well as the helper bash function [`sbatch_submit`](https://github.com/gbrookshire/sbatch_submit).


## Analysis details


### Plots

Recreate the plots in Figures 2-4 and Supplementary figure 1 by running the script `generate_plots.py`. Recreate Fig. 5 by running `notebooks/Re-analyses of public data-sets.ipynb`. Recreate Fig. 6 by running the other Jupyter notebooks in the `notebooks/` directory (with `USE_CACHE = True`).


### Tables

Table 1 (comparison of false positives) is generated by `false_pos_stats.R`. Table 2 (detection ratios) is generated  by `reconstructed_oscillations()` in `generate_plots.py`.

### Reproduce the analyses

To re-run all the simulations reported in Figures 2-4, run `batch_simulations.sh`. This will take a pretty long time if you run the analyses in series. To run the analyses in parallel using `sbatch`, change the hostname in line 38 to the hostname of your computing cluster. You'll also need to make sure sbatch has access to all the necessary libraries by adapting `load_python.sh` to match the names of these libraries on your cluster, and then making sure that this file is on the search path.

The analyses in Figure 5 are in the Jupyter notebook `notebooks/Re-analyses of public data-sets.ipynb`.

The analyses in Figure 6 are in three Jupyter notebooks in the `notebooks/` directory. To re-run these simulations instead of using the previously-saved values, set the variable `USE_CATCH = False`.
- Fig. 6a: `Cutoff frequency for the AR surrogate analysis.ipynb` 
- Fig. 6b-c: `Multiple comparison corrections.ipynb`
- Fig. 6d-f: `Signal length.ipynb`
- Fig. 6g-i: `Sampling rate.ipynb`


### Simulations using 1/f noise

The 'white noise' and 'random walk' simulations were performed using a power-law $1/f^b$ noise generator. This generator produces white noise when b = 0, and random walk noise when b = 2.


### Filenames of saved simulations

I've saved the results of all simulations in the `results/` directory. These filenames are based on the following template:

{Analysis method}`_`{Aperiodic details}`_f_`{Oscillation frequency}`_amp_`{Oscillation amplitude}{Additional description}`.npy`

The analysis method can take any of the following values:

- LF2012: `landau`
- FSK2013: `fiebelkorn`
- AR surrogate: `ar`
- Robust est.: `mann_lees`

The aperiodic details tell you which noise type each simulation used:

- Fully random: `fully_random`
- White noise: `exp_0.00` (power-law noise with an exponent of 0)
- Random walk: `exp_2.00` (power-law noise with an exponent of 2)
- AR(1) noise: `ar_0.50_ma_0.00`

Oscillation frequency and amplitude tell you about the frequency and amplitude of the true oscillation that was added to the aperiodic background noise. For simulations without any oscillations, both of these are set to `0.00`.

The additional description notes any variations from the standard analysis. For example, the `-FDR` label shows simulations using FDR correction (when that is not the standard for that analysis).

Example:
`mann_lees_exp_2.00_f_0.00_amp_0.00.npy` is random walk noise with no behavioral oscillation (frequency of 0, amplitude of 0), analyzed using the robust est. method.


