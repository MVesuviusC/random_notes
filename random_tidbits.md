# VScode

I'm getting disconnected from the server due to either OOM or too many spawned processes

https://github.com/microsoft/vscode-remote-release/issues/10804 suggests that it may be telemetry?

Disabled all telemetry

Also seeing javascript 'node' processes as the culprit

Disabling telemetry didn't seem to matter. I've started using salloc to get an allocation and connecting straight to that node.
