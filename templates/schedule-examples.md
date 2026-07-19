# Schedule examples

Cron (Linux), adjust the hour to your quiet time:

    0 4 * * 1,3,5  /path/to/run.sh article
    0 4 * * 2,4,6  /path/to/run.sh page
    0 2 * * 0      /path/to/run.sh optimize
    30 1 * * *     /path/to/run.sh index

Each run.sh line wraps: source the .env, then `claude -p "<the nightly run prompt>"` with a hard timeout (55 to 75 minutes) and a status line on failure. On Mac, use one launchd plist per job instead of cron; ask your agent to generate them, it knows your machine.
