# Understanding AlphaFold Output Files

This document explains the output files generated by AlphaFold and their significance for protein structure prediction.

## Output Directory Structure

The output directory contains several types of files for each model prediction:

### 1. Model Predictions
- `unrelaxed_model_[1-5]_pred_0.pdb`: Initial predicted structure in PDB format
- `unrelaxed_model_[1-5]_pred_0.cif`: Initial predicted structure in mmCIF format
- `relaxed_model_[1-5]_pred_0.pdb`: Relaxed (energy-minimized) structure in PDB format
- `relaxed_model_[1-5]_pred_0.cif`: Relaxed structure in mmCIF format

### 2. Confidence Metrics
- `confidence_model_[1-5]_pred_0.json`: Per-residue confidence scores
  - Contains three key arrays:
    - `residueNumber`: Position in the sequence
    - `confidenceScore`: pLDDT score (0-100)
    - `confidenceCategory`: Confidence category (L=Low, M=Medium, H=High)
  - pLDDT interpretation:
    - < 50: Low confidence (L)
    - 50-70: Medium confidence (M)
    - 70-90: High confidence (H)
    - > 90: Very high confidence (H)

### 3. Ranking and Analysis
- `ranked_[0-4].pdb/.cif`: Models sorted by confidence, with 0 being the best
- `ranking_debug.json`: Details about the model ranking process
- `relax_metrics.json`: Metrics from the structure relaxation step
- `timings.json`: Performance metrics for different stages of prediction

### 4. Intermediate Data
- `features.pkl`: Input features extracted from the sequence
- `result_model_[1-5]_pred_0.pkl`: Raw prediction results
- `msas/`: Directory containing multiple sequence alignments used for prediction

## Key Files for Analysis

1. **Best Model**: Always check `ranked_0.pdb` first - this is AlphaFold's best prediction
2. **Confidence Assessment**: Review `confidence_model_[1-5]_pred_0.json` to understand prediction reliability
   - High confidence scores (>90) suggest very reliable predictions
   - Lower scores may indicate flexible or disordered regions

## File Formats

1. **PDB Files** (.pdb):
   - Standard format for protein structures
   - Easily viewable in molecular visualization software (PyMOL, VMD, etc.)
   - Contains atomic coordinates and basic metadata

2. **mmCIF Files** (.cif):
   - Modern format for protein structures
   - More detailed than PDB format
   - Better handles large structures and contains more metadata

## Using the Output

1. **Structure Analysis**:
   - Use `ranked_0.pdb` for your primary analysis
   - Compare with other ranked models to assess structural variability
   - Pay attention to regions with high confidence scores

2. **Quality Assessment**:
   - Check confidence scores to identify reliable regions
   - Look for consistently high-confidence regions across models
   - Be cautious about interpreting low-confidence regions

3. **Visualization**:
   - Color structure by pLDDT score to highlight reliable regions
   - Compare multiple models to understand structural flexibility
   - Focus analysis on high-confidence regions

## Visualizing PDB Files

To visualize the predicted protein structures (PDB files), you have several options:

1. **Desktop Applications**:
   - **PyMOL**: Professional-grade molecular visualization (recommended)
     - Download from: https://pymol.org/
     - Features:
       - High-quality rendering
       - Powerful analysis tools
       - Script automation support
       - Can color by B-factor (confidence scores in AlphaFold)
   
   - **UCSF Chimera**: Free academic visualization tool
     - Download from: https://www.cgl.ucsf.edu/chimera/
     - Good for structure analysis and comparison

   - **VMD**: Specialized in molecular dynamics
     - Download from: https://www.ks.uiuc.edu/Research/vmd/
     - Excellent for trajectory analysis

2. **Web-Based Viewers**:
   - **Mol***: Modern web-based viewer
     - Access via: https://molstar.org/viewer/
     - Just drag and drop your PDB file
   
   - **NGL Viewer**: Lightweight web viewer
     - Access via: http://nglviewer.org/ngl/
     - Good for quick visualization

3. **Analyzing ranked_0.pdb**:
   - This is AlphaFold's best prediction
   - The B-factor column (last number in each ATOM record) contains pLDDT confidence scores
   - In PyMOL, you can color by B-factor to visualize confidence:
     ```python
     # PyMOL commands
     load ranked_0.pdb
     spectrum b, rainbow   # Colors structure by confidence scores
     ```

4. **Best Practices**:
   - Always start by viewing `ranked_0.pdb`
   - Color the structure by confidence scores
   - Compare multiple models to understand flexibility
   - Look for regions with high confidence scores (>90)
   - Be cautious about interpreting low-confidence regions

5. **Important Features to Look For**:
   - Secondary structure elements (helices, sheets)
   - Overall fold and domain organization
   - Regions of high vs. low confidence
   - Potential flexible regions (varying between models)
   - Biologically important sites or motifs

## Understanding Pickle (.pkl) Files

AlphaFold generates two types of pickle files that contain detailed prediction data:

1. **result_model_[1-5]_pred_0.pkl**:
   Contains the raw prediction outputs including:
   - `distogram`: Distance predictions between residue pairs
   - `experimentally_resolved`: Predictions about atom positions
   - `masked_msa`: Multiple sequence alignment information
   - `predicted_lddt`: Raw predictions for local confidence
   - `structure_module`: Final atom positions and masks
   - `plddt`: Per-residue confidence scores (0-100)
   - `ranking_confidence`: Overall model confidence

2. **features.pkl**:
   Contains input features used for prediction:
   - `sequence`: The input protein sequence
   - `aatype`: Amino acid type encodings
   - `msa`: Multiple sequence alignment data
   - `template_*`: Information about structural templates
   - `residue_index`: Numbering of residues
   - `domain_name`: Name of the protein domain

### Key Metrics from Pickle Files

1. **Confidence Scores (pLDDT)**:
   - Range: 0-100
   - Higher is better
   - Your results show:
     - Average score: 89.16 (Very good)
     - Range: 65.29 - 93.71
     - Most residues have high confidence (>70)

2. **Multiple Sequence Alignment (MSA)**:
   - Your protein had 3,303 aligned sequences
   - This is a good number for prediction accuracy
   - More diverse alignments generally improve prediction quality

3. **Structure Module**:
   - Contains final atomic coordinates
   - Includes all backbone and side chain atoms
   - Shape: (34 residues, 37 atoms per residue, 3 coordinates)

### Analyzing Pickle Files

To analyze these files, you can use the provided `inspect_pkl.py` script:
```python
python inspect_pkl.py
```

This will show:
- Data structure of predictions
- Confidence scores
- Sequence information
- Template usage
- MSA statistics

## Why Five Models?

AlphaFold generates five models for each prediction for several important reasons:

1. **Sampling Different Conformations**:
   - Proteins can exist in multiple stable conformations
   - Different models may capture different possible structural states
   - Helps identify flexible or dynamic regions of the protein

2. **Confidence Assessment**:
   - Agreement between models indicates prediction reliability
   - Regions that vary between models may be:
     - Naturally flexible
     - Have multiple possible conformations
     - Harder to predict accurately

3. **Model Architecture**:
   - Each model uses slightly different neural network parameters
   - Models are trained independently with different random seeds
   - This ensemble approach improves prediction robustness

4. **Ranking System**:
   - AlphaFold ranks the five models based on predicted confidence
   - `ranked_0` represents the most confident prediction
   - Comparing ranks helps identify the most likely structure

5. **Scientific Best Practice**:
   - Multiple models follow the scientific principle of ensemble sampling
   - Helps avoid over-relying on a single prediction
   - Provides error estimates for the prediction

When analyzing results, it's important to:
- Start with the highest-ranked model (`ranked_0`)
- Compare models to identify consistent and variable regions
- Consider all models when the confidence scores are similar

## Performance Metrics

The `timings.json` file provides detailed information about:
- MSA generation time
- Feature processing time
- Model prediction time
- Structure relaxation time

This can be useful for:
- Optimizing future runs
- Understanding computational requirements
- Identifying bottlenecks in the prediction pipeline
