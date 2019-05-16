# xivo-purge-db

Keeping records of personal communications for long periods may be
subject to local legislation, to avoid personal data retention. Also,
keeping too many records may become resource intensive for the server.
To ease the removal of such records, `xivo-purge-db` is a process that
removes old log entries from the database. This allows keeping records
for a maximum period and deleting older ones.

By default, xivo-purge-db removes all logs older than a year (365 days).
xivo-purge-db is run nightly.

<div class="note">

<div class="admonition-title">

Note

</div>

Please check the laws applicable to your country and modify
`days_to_keep` (see below) in the configuration file accordingly.

</div>

## Tables Purged

The following features are impacted by xivo-purge-db:

  - <span data-role="ref">call\_logs</span>
  - Call center statistics

More technically, the tables purged by `xivo-purge-db` are:

  - `call_log`
  - `cel`
  - `queue_log`
  - `stat_agent_periodic`
  - `stat_call_on_queue`
  - `stat_queue_periodic`
  - `stat_switchboard_queue`

## Configuration File

We recommend to override the setting `days_to_keep` from
`/etc/xivo-purge-db/config.yml` in a new file in
`/etc/xivo-purge-db/conf.d/`.

<div class="warning">

<div class="admonition-title">

Warning

</div>

Setting `days_to_keep` to 0 will NOT disable `xivo-purge-db`, and will
remove ALL logs from your system.

</div>

See <span data-role="ref">configuration-priority</span> and
`/etc/xivo-purge-db/config.yml` for more details.

## Manual Purge

It is possible to purge logs manually. To do so, log on to the target
Wazo server and run:

    xivo-purge-db

You can specify the number of days of logs to keep. For example, to
purge entries older than 365 days:

    xivo-purge-db -d 365

Usage of `xivo-purge-db`:

    usage: xivo-purge-db [-h] [-d DAYS_TO_KEEP]
    
    optional arguments:
      -h, --help            show this help message and exit
      -d DAYS_TO_KEEP, --days_to_keep DAYS_TO_KEEP
                            Number of days data will be kept in tables

## Maintenance

After an execution of `xivo-purge-db`, postgresql's [Autovacuum
Daemon]() should perform a [VACUUM]() ANALYZE automatically (after 1
minute). This command marks memory as reusable but does not actually
free disk space, which is fine if your disk is not getting full. In the
case when `xivo-purge-db` hasn't run for a long time (e.g. upgrading to
15.11 or when <span data-role="ref">days\_to\_keep
\<purge\_logs\_config\_file\></span> is decreased), some administrator
may want to perform a [VACUUM]() FULL to recover disk space.

<div class="warning">

<div class="admonition-title">

Warning

</div>

VACUUM FULL will require a service interruption. This may take several
hours depending on the size of purged database.

</div>

You need to:

    $ wazo-service stop
    $ sudo -u postgres psql asterisk -c "VACUUM (FULL)"
    $ wazo-service start

## Archive Plugins

In the case you want to keep archives of the logs removed by
xivo-purge-db, you may install plugins to xivo-purge-db that will be run
before the purge.

Wazo does not provide any archive plugin. You will need to develop
plugins for your own need. If you want to share your plugins, please
open a [pull request](https://github.com/wazo-pbx/xivo-purge-db/pulls).

## Archive Plugins (for Developers)

Each plugin is a Python callable (function or class constructor), that
takes a dictionary of configuration as argument. The keys of this
dictionary are the keys taken from the configuration file. This allows
you to add plugin-specific configuration in
`/etc/xivo-purge-db/conf.d/`.

There is an example plugin in the [xivo-purge-db git
repo](https://github.com/wazo-pbx/xivo-purge-db/tree/master/contribs).

### Example

Archive name: sample

Purpose: demonstrate how to create your own archive plugin.

#### Activate Plugin

Each plugin needs to be explicitly enabled in the configuration of
`xivo-purge-db`. Here is an example of file added in
`/etc/xivo-purge-db/conf.d/`:

``` sourceCode yaml
enabled_plugins:
    archives:
        - sample
```

#### sample.py

The following example will be save a file in `/tmp/xivo_purge_db.sample`
with the following content:

    Save tables before purge. 365 days to keep!

``` sourceCode python
sample_file = '/tmp/xivo_purge_db.sample'
```

>   - def sample\_plugin(config):
>     
>       - with open(sample\_file, 'w') as output:  
>         output.write('Save tables before purge. {0} days to
>         keep\!'.format(config\['days\_to\_keep'\]))

#### Install sample plugin

The following `setup.py` shows an example of a python library that adds
a plugin to xivo-purge-db:

``` sourceCode python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from setuptools import setup
from setuptools import find_packages


setup(
    name='xivo-purge-db-sample-plugin',
    version='0.0.1',

    description='An example program',
    packages=find_packages(),
    entry_points={
        'xivo_purge_db.archives': [
            'sample = xivo_purge_db_sample.sample:sample_plugin',
        ],
    }
)
```
