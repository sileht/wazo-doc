# Graphics

There are graphics, locate to
<span data-role="file">/var/cache/munin/www/localdomain/localhost.localdomain/</span>,
that give a historical overview of a Wazo system's activity based on
snapshots recorded every 5 minutes. Graphics are available for the
following resources :

  - <span data-role="file">cpu-\*.png</span>
  - <span data-role="file">entropy-\*.png</span>
  - <span data-role="file">interrupts-\*.png</span>
  - <span data-role="file">irqstats-\*.png</span>
  - <span data-role="file">load-\*.png</span>
  - <span data-role="file">memory-\*.png</span>
  - <span data-role="file">open\_files-\*.png</span>
  - <span data-role="file">open\_inodes-\*.png</span>
  - <span data-role="file">swap-\*.png</span>

Each graphic is available with different time range: `day`, `week`,
`month`, `year`

## Troubleshooting

### Missing graphs

Symptoms:

  - daily graphs are missing
  - weekly/monthly/yearly graphs stop updating
  - a mail is sent from cron every 5 minutes about a "bad magic number"

Cause:

  - the machine was forcefully stopped, while munin (responsible for the
    graphs) was running, resulting in a file corruption

Resolution:

  - Run the following
        command:
    
        rm /var/lib/munin/htmlconf.storable /var/lib/munin/limits.storable

  - The graphs will be restored on the next run of munin, in the next 5
    minutes.
