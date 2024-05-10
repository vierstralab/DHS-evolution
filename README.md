# DHS Evolution Analysis

This repository contains scripts and notebooks for the analysis of DHS evolution, specifically focusing on mapping the human DHS index to other species and analyzing reciprocal peaks between humans and mice. The analysis leverages updated datasets such as cactus447way and multiz100way.

## Contents

- **reciprocal_peaks.md**  
  Provides documentation on identifying reciprocal peaks between human and mouse. This is closely associated with the Jupyter notebook `Cactus447way_april_index-latest_version_reciprocal_peaks.ipynb`.

- **Cactus447way_april_index-latest_version_reciprocal_peaks.ipynb**  
  Contains specific code sections for reciprocal mapping, designed to complement the markdown documentation.

- **Cactus447way_april_index.ipynb**  
  This is the main working notebook that includes code for processing mapping results from the latest index version. It features analysis of reciprocal peaks and contains various visualizations and data processing workflows.

## Usage

For sorting, and intersections the bedops/2.4.26 module is required.   
**Note:** The code in this repository frequently accesses data from the user's server folder. Paths and specific locations are configured to match this setup. If you are adapting this code for your own use, you may need to modify these paths to match your environment.
