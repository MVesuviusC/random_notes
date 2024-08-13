# Conda environments

Package from: https://github.com/PacificBiosciences/pbbioconda

# Example of making a conda environment for isoseq3
```{bash}
export PATH="/gpfs0/export/apps/easybuild/software/Miniconda3/4.7.10/bin:$PATH"
# Or load the module
conda create -n isoseq python=2.7
eval "$(conda shell.bash hook)" # Don't need to do this most likely
conda activate isoseq

conda install -n isoseq -c bioconda isoseq
conda install -n isoseq -c bioconda lima
```

# Tell conda not to automatically turn itself on!
`conda config --set auto_activate_base false`

# Use a conda environment inside of a sbatch file
Add this to the top of the sbatch file

```{bash}
ml load Miniconda3/4.9.2
eval "$(conda shell.bash hook)"
conda activate conda_env_name_here_yo
```