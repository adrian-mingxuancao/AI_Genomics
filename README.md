# AI_Genomics Project

This repository contains the implementation and integration of two powerful genomics models: GET (Gene Expression Transformer) and AlphaFold.

## Project Structure

```
AI_Genomics/
├── models/
│   ├── get_model/          # GET model implementation
│   │   ├── data/          # GET-specific data
│   │   ├── configs/       # Model configurations
│   │   └── checkpoints/   # Model checkpoints
│   └── alphafold/         # AlphaFold implementation
│       ├── data/          # Symbolic link to /net/scratch/caom/alphafold_data
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
- Location: `/net/scratch/caom/alphafold_data`
- Contains all generic AlphaFold data including:
  - Sequence databases
  - Structure templates
  - Parameter files
  - Model weights

### GET Model Data
- Data location: To be determined
- Required data will be documented once obtained

## Model Implementation Mindmaps

### AlphaFold Implementation
- Located in `docs/alphafold_mindmap.md`
- Key components:
  - Data preprocessing pipeline
  - Model architecture
  - Inference pipeline
  - Result visualization

### GET Implementation
- Located in `docs/get_mindmap.md`
- Key components:
  - Data preprocessing
  - Model architecture
  - Training pipeline
  - Inference pipeline

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
- AlphaFold: Follow setup instructions in `models/alphafold/README.md`
- GET: Follow setup instructions in `models/get_model/README.md`

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
