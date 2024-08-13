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
# Remove a conda environment
`conda remove -n conda_env_name_here_yo --all`

# Get and set environment variables inside a conda environment
```{bash}
conda activate my_cool_conda_thing

conda env config vars list
conda env config vars set rrrSnvs_version=0.2.0

conda activate my_cool_conda_thing

echo ${rrrSnvs_version}
```
