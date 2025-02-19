# AlphaFold Implementation Mindmap

## Implementation Status Overview
- [x] Basic Setup Complete (Repository & Structure)
- [x] Data Pipeline Ready (Data Available)
- [ ] Model Running
- [ ] Results Validation

## Data Pipeline
- Input Processing
  - [x] FASTA sequence parsing (code available)
  - [x] MSA generation (code available)
  - [x] Template search (code available)
  - [x] Feature generation (code available)

## Model Architecture
- Evoformer
  - [x] MSA stack (code available)
  - [x] Pair stack (code available)
  - [x] Attention mechanisms (code available)
- Structure Module
  - [x] 3D coordinate prediction (code available)
  - [x] Confidence scoring (pLDDT) (code available)
- [x] Recycling (code available)

## Dependencies
- [ ] JAX
- [ ] HH-suite
- [ ] OpenMM
- [ ] Kalign

## Execution Flow
1. [ ] Sequence Input
2. [ ] MSA Generation
   - [ ] Sequence search against databases
   - [ ] Multiple sequence alignment
3. [ ] Template Search
4. [ ] Feature Processing
5. [ ] Model Prediction
6. [ ] Structure Refinement
7. [ ] Output Generation

## Data Requirements
- Location: /net/scratch2/caom/alphafold_data (✓ FOUND)
  - [x] BFD (small_bfd available)
  - [x] MGnify
  - [x] PDB70
  - [x] UniRef90
  - [x] UniProt
  - [x] PDB structure files (pdb_mmcif)
  - [x] Model parameters (params)
  - [x] Additional: uniref30, pdb_seqres

## Output Processing
- [ ] PDB file generation
- [ ] Confidence score visualization
- [ ] Structure visualization
  - [ ] PyMOL integration
  - [ ] ChimeraX support
