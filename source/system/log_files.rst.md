# Log Files

Every Wazo service has its own log file, placed in
<span data-role="file">/var/log</span>.

## asterisk

The Asterisk log files are managed by logrotate.

It's configuration files
<span data-role="file">/etc/logrotate.d/asterisk</span> and
<span data-role="file">/etc/asterisk/logger.conf</span>

The message log level is enabled by default in
<span data-role="file">logger.conf</span> and contains notices, warnings
and errors. The full log entry is commented in
<span data-role="file">logger.conf</span> and should only be enabled
when verbose debugging is required. Using this option in production
would produce VERY large log files.

  - Files location: <span data-role="file">/var/log/asterisk/\\\*</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## wazo-auth

  - File location: <span data-role="file">/var/log/wazo-auth.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/wazo-auth</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## wazo-calld

  - File location: <span data-role="file">/var/log/wazo-calld.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/wazo-calld</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## wazo-dird

  - File location: <span data-role="file">/var/log/wazo-dird.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/wazo-dird</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## wazo-upgrade

  - File location:
    <span data-role="file">/var/log/xivo-upgrade.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-upgrade</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-agentd

  - File location:
    <span data-role="file">/var/log/xivo-agentd.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-agentd</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-agid

  - File location: <span data-role="file">/var/log/xivo-agid.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-agid</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-amid

  - File location: <span data-role="file">/var/log/xivo-amid.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-amid</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## wazo-call-logd

  - File location:
    <span data-role="file">/var/log/wazo-call-logd.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/wazo-call-logd</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-confd

  - File location: <span data-role="file">/var/log/xivo-confd.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-confd</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-confgend

The xivo-confgend daemon output is sent to the file specified with the
`--logfile` parameter when launched with twistd.

The file location can be changed by customizing the
xivo-confgend.service unit file.

  - File location:
    <span data-role="file">/var/log/xivo-confgend.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-confgend</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-dird-phoned

  - File location:
    <span data-role="file">/var/log/xivo-dird-phoned.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-dird-phoned</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-dxtora

  - File location:
    <span data-role="file">/var/log/xivo-dxtora.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-dxtora</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-provd

  - File location: <span data-role="file">/var/log/xivo-provd.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-provd</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-purge-db

  - File location:
    <span data-role="file">/var/log/xivo-purge-db.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-purge-db</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-stat

  - File location: <span data-role="file">/var/log/xivo-stat.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-stat</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-sysconfd

  - File location:
    <span data-role="file">/var/log/xivo-sysconfd.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-sysconfd</span>
  - Number of archived files: 15
  - Rotation frequence: Daily

## xivo-websocketd

  - File location:
    <span data-role="file">/var/log/xivo-websocketd.log</span>
  - Rotate configuration:
    <span data-role="file">/etc/logrotate.d/xivo-websocketd</span>
  - Number of archived files: 15
  - Rotation frequence: Daily
