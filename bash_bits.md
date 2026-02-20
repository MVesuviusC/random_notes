# Random bash bits

## Get time till the next maintenance
Add this to `~/.bashrc`
This is likely very specific to our Franklin cluster
```{bash}
get_maint_time () {
    maint_start=$(scontrol show -u root reservation \
                | perl -pe 's/\n/#/g' \
                | perl -pe 's/#ReservationName=/\n/g' \
                | grep MAINT \
                | perl -ne '/.+StartTime=(.+)\sEndTime/; print $1;')
    difftime=$(($(date +%s -d "$maint_start") - $(date +%s) - 1000))
    req_len=$(date -d "@$difftime" "+$(($difftime/86400))-%H:%M:%S")
    echo $req_len
}
alias salloc_l='salloc --time=$(get_maint_time)'
alias srun_l='srun --time=$(get_maint_time)'
alias sbatch_l='sbatch --time=$(get_maint_time)'
```

## Change time limit on a job:

`scontrol update jobid=4272971 TimeLimit=2-00:00:00`

## Set high priority for slurm job

`srun_l -c 5 -p himem --priority=TOP --x11 --pty bash`

## Get total number of processes per user
`ps -U mvc002 -u mvc002 u | wc -l`

Franklin has 300 process limit

## See OOM kill events

```{bash}
 dmesg | grep `id -u` | grep oom
```

## Conditions in bash loops
shamelessly stolen from https://unix.stackexchange.com/questions/306111/what-is-the-difference-between-the-bash-operators-vs-vs-vs

if [ condition ]

[ is another name for the traditional test command. [ / test is a standard POSIX utility. All POSIX shells have it builtin (though that's not required by POSIX²). The test command sets an exit code and the if statement acts accordingly. Typical tests are whether a file exists or one number is equal to another.

if [[ condition ]]

This is a new upgraded variation on test¹ from ksh that bash, zsh, yash, busybox sh also support. This [[ ... ]] construct also sets an exit code and the if statement acts accordingly. Among its extended features, it can test whether a string matches a wildcard pattern (not in busybox sh).

if ((condition))

Another ksh extension that bash and zsh also support. This performs arithmetic. As the result of the arithmetic, an exit code is set and the if statement acts accordingly. It returns an exit code of zero (true) if the result of the arithmetic calculation is nonzero. Like [[...]], this form is not POSIX and therefore not portable.

if (command)

This runs command in a subshell. When command completes, it sets an exit code and the if statement acts accordingly.

A typical reason for using a subshell like this is to limit side-effects of command if command required variable assignments or other changes to the shell's environment. Such changes do not remain after the subshell completes.

if command

command is executed and the if statement acts according to its exit code.

