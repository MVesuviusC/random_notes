# Debugging

Contents of launch.json in ~/.vscode/
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "debugpy",
            "request": "launch",
            "program": "${file}",
            "args": [
                "-t", "/home/gdrobertslab/lab/Counts/S0115/possorted_bam.bam",
                "-n", "/home/gdrobertslab/lab/Analysis/Matt/stjude/rawData/SJOS046149_G1.WholeGenome.bam",
                "-j", "1",
                "-b", "5000000",
                "-r", "/reference/homo_sapiens/hg38/ucsc_assembly/illumina_download/Sequence/WholeGenomeFasta/genome.fa",
                "--outdir", "testDir"
            ],
            "console": "integratedTerminal"
        },
        {
            "name": "Python Debugger: Current File",
            "type": "debugpy",
            "request": "launch",
            "program": "${file}",
            "env": {"PYDEVD_WARN_EVALUATION_TIMEOUT": "500"},
            "console": "integratedTerminal"
        }
    ]
}

```

## Making environments

### UV
uv init --python 3.12
The default project location is your cwd, the environment in .venv/ will be created when you add something (lazy creation)

or
uv init -n my_project -p 3.12

### install stuff
uv add --project my_project scanpy
or
uv add scanpy
or
uv pip install [thing here]
 This method will not add the package to pyproject.toml

#### Activate
source cool_env/bin/activate
or
source .venv/bin/activate

environment can be reproduced from pyproject.toml and uv.lock
