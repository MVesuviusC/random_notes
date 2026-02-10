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

