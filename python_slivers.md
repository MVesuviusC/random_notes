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