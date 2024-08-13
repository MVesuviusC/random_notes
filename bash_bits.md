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