# AI_Genomics Project

This repository contains the implementation and integration of two powerful genomics models: GET (Gene Expression Transformer) and AlphaFold.

## Project Structure

```
AI_Genomics/
├── models/
│   ├── get_model/          # GET model implementation (175MB)
│   │   ├── tutorials/      # Jupyter notebooks for data processing and model usage
│   │   │   ├── prepare_pbmc.ipynb     # Data processing tutorial
│   │   │   ├── finetune_pbmc.ipynb    # Model fine-tuning tutorial
│   │   │   ├── predict_atac.ipynb     # ATAC prediction demo
│   │   │   └── pretrain_pbmc.ipynb    # Pre-training tutorial
│   │   ├── get_model/     # Core model implementation
│   │   └── env.yml        # Conda environment specification
│   └── alphafold/         # AlphaFold implementation (34MB)
│       ├── data/          # Symbolic link to alphafold_data
│       ├── configs/       # Model configurations
│       └── checkpoints/   # Model checkpoints
├── experiments/
│   ├── get_experiments/   # GET experiment scripts and results
│   └── af_experiments/    # AlphaFold experiment scripts and results
├── utils/                 # Shared utility functions
├── notebooks/            # Jupyter notebooks for analysis
└── docs/                 # Documentation and model mindmaps
```

## Model Data Locations

### AlphaFold Data
Required data includes:
  - Sequence databases (UniRef90, BFD, MGnify)
  - Structure templates (PDB70)
  - Parameter files
  - Model weights

Note: AlphaFold data setup will be done separately following the official installation guide.

### GET Model Data
The GET model requires the following data preparation steps:
1. PBMC Data Processing:
   - Follow the tutorial in `models/get_model/tutorials/prepare_pbmc.ipynb`
   - Data processing pipeline includes:
     - Peak sorting (chr1, chr2, chr3 order)
     - Count matrix preparation
     - Quality checks (>3M depth recommended)

2. Model Training Data:
   - Fine-tuning data: Follow `models/get_model/tutorials/finetune_pbmc.ipynb`
   - ATAC prediction: Use `models/get_model/tutorials/predict_atac.ipynb`
   - Pre-training: Reference `models/get_model/tutorials/pretrain_pbmc.ipynb`

## Setup and Installation

1. Clone the repository:
```bash
git clone https://github.com/[your-username]/AI_Genomics.git /home/caom/AI_Genomics
cd /home/caom/AI_Genomics
```

2. Create and activate a conda environment:
```bash
conda create -n ai_genomics python=3.8
conda activate ai_genomics
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Set up model-specific requirements:
   - GET Model:
     ```bash
     cd models/get_model
     conda env create -f env.yml
     conda activate get
     ```
   - AlphaFold:
     ```bash
     # Create AlphaFold conda environment
     cd models/alphafold
     conda create -n alphafold python=3.8
     conda activate alphafold
     
     # Install JAX with CUDA support
     pip install --upgrade "jax[cuda]" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
     
     # Install other dependencies
     conda install -y -c conda-forge openmm=7.5.1 pdbfixer
     conda install -y -c bioconda hmmer hhsuite kalign2
     pip install -r docker/requirements.txt
     
     # Download genetic databases and model parameters
     # (This will be done separately following cluster-specific storage guidelines)
     ```
     
     Note: We use conda environment instead of Docker in the cluster environment for:
     - Better integration with SLURM job scheduler
     - Direct access to cluster's optimized CUDA libraries
     - Improved performance without Docker virtualization
     - Better resource management
     - Direct access to cluster storage

## Computational Resources

### GPU Access
To access GPU resources for model training and inference, use the following SLURM command:

```bash
srun -p general --pty -t 120:00 --cpus-per-task=32 --mem=64G --gres=gpu:a100:2 /bin/bash
```

This command requests:
- Partition: general
- Time limit: 120 hours
- CPUs: 32 cores
- Memory: 64GB
- GPUs: 2 NVIDIA A100 GPUs
- Interactive bash session

Note: Adjust the resources (time, CPU, memory, GPU) based on your specific needs.

## Version Control

This repository uses Git for version control. Important files:
- `.gitignore`: Excludes large data files, model checkpoints, and environment-specific files
- `.gitattributes`: Handles large file storage using Git LFS
- `requirements.txt`: Lists all Python dependencies

## Contributing

1. Create a new branch for your feature
2. Make your changes
3. Submit a pull request

## License

Please refer to the original licenses of [AlphaFold](https://github.com/google-deepmind/alphafold) and [GET](https://github.com/GET-Foundation/get_model) models.
