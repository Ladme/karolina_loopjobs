# Karolina Slurm LoopJobs

Scripts for running Gromacs simulations on the Karolina supercomputer (IT4I).

## Run Scripts
- `loop_md_cpu`: Runs a single CPU-only job on one node.
- `loop_md_gpu`: Runs a single GPU-accelerated job on one node.
- `loop_re_cpu`: Runs HREX simulation over multiple replicas using CPUs only (can use one or more nodes).

## Submission Script
- `loop_sub`: Submits jobs in a loop to the batch system.

## How to Use
1. Set up your job directory as usual.
2. Copy the appropriate run script (e.g., `loop_md_cpu`) and the submission script (`loop_sub`) into your job directory.
3. Edit the header of the run script:
   - Replace `[[ACCOUNT_ID]]` with your account ID.
   - Adjust the walltime, number of CPUs, and GPUs as needed.
   - Note that `loop_md*` scripts can only run jobs on a single node.
4. Submit the job using: `./loop_sub RUN_SCRIPT N`, where `N` is the number of cycles you want the job to run. All jobs will be submitted at once, but only the first will start. The rest will wait for the previous one to finish.
5. After each cycle, output files will be saved in the `storage` directory. Standard output and error logs will be in `.out` and `.err` files in the job directory.

## Important Notes
- You cannot add more cycles to the simulation until all submitted cycles have finished. Do not run `loop_sub` again until the previous cycles are complete.
- `loop_sub` creates service files (`sub_job_JOBID`) to prevent duplicate submissions. These files will be removed once all cycles finish. If the simulation crashes, you may need to delete these files manually before running `loop_sub` again.
- The `loop_sub` script also generates a `finished_cycle` file, which tracks the completed cycles. **Do not delete this file!** Removing it could cause the simulation to resume from the wrong cycle, potentially overwriting your data. This file also allows you to add more cycles after the current ones are done.
- Jobs run directly in the job directory. Make sure to submit your jobs from `scratch`.
- You can add `loop_sub` to a directory in your `PATH` so you don’t have to copy it to each job directory.