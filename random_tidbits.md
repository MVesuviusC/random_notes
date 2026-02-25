# VScode

I'm getting disconnected from the server due to either OOM or too many spawned processes

https://github.com/microsoft/vscode-remote-release/issues/10804 suggests that it may be telemetry?

Disabled all telemetry

Also seeing javascript 'node' processes as the culprit

Disabling telemetry didn't seem to matter. I've started using salloc to get an allocation and connecting straight to that node.

## Connecting VScode to igm servers
Emily got error:
`could not establish connection to 'node': the remote host oes not meet the pre-requisites for running the VS Code server.`

Solution: install older version of vscode :-| due to OS being too old

> Starting with VS Code release 1.99 (March 2025), the prebuilt servers distributed by VS Code are only compatible with Linux distributions that are based on glibc 2.28 or later. These include for example, Debian 10, RHEL 8, or Ubuntu 20.04.

## WinSCP
To get files to open instead of edit when double clicked: Options > Preferences > Panels > Double-Click > Operation to perform on double-click: Open

## Figshare downloading files
Find the item number, which is at the end of the download link

Put that into this: wget -O my_file.qs "https://ndownloader.figshare.com/files/item_number_here"

## Illumina demultiplexing of 10x data

For flex data (SI-TS-**) use I7 normal and I5 reverse compliment

## DNA nexus

### DNAnexus app building

source ~/virtualEnvs/dnanexus/bin/activate
dx login # login to server, choose which project to attach to
dx build # build app from the json and associated file(s)
deactivate

Examples in random_scripts/DNA_nexus

To use, either upload to project or copy from another project.

Click on app in folder on dna nexus site, select settings. When looking for files, make sure you select "Current folder and subfolders" or whatever so you can see your files.

### downloading

# load environment
source dnanexus/bin/activate

# login
dx login

dx ls OS_stratification_2025_05_19/BAM/*RNA-Seq.bam \
  | perl -pe 's/ .+//' \
  > misc/RNAseq_bam_files.txt

dx describe --json --multi OS_stratification_2025_05_19/BAM/*RNA-Seq.bam.bai \
  > misc/RNAseq_file_metadata.json

dx download works well too, but had issues where md5sums were missing from the file metadata and the file was empty.

Ended up using
dx make_download_url ${file_id} and then wget -c in a loop in 26_stratification_data project


## Unmute zoom when signing in on podium through phone
Push *1 (bottom left of dialer keypad)
Brings up menu on far screen (for us)
hit 2 to unmute mic
Hit * again to hide menu
