# Managing DHCP server configuration

This page considers the configuration files of the DHCP server in
<span data-role="file">/etc/dhcp/dhcpd\_update/</span>.

## Who modifies the files

The files are updated with the command `dhcpd-update`, which is also run
when updating the provisioning plugins. This commands fetches
configurations files from the `provd.wazo.community` server.

## How to update the source files

### Ensure your modifications are working

  - On a Wazo, edit manually the file
    <span data-role="file">/etc/dhcp/dhcpd\_update/\*.conf</span>
  - `service isc-dhcp-server restart`
  - If errors are shown in
    <span data-role="file">/var/log/daemon.log</span>, check your
    modifications

### Edit the files

  - Edit the files in the Git repo `xivo-provd-plugins`, directory
    <span data-role="file">dhcp/</span>
  - Push your modifications
  - Go in <span data-role="file">dhcp/</span>
  - Run `make upload` to push your modifications to
    `provd.wazo.community`. There is no `testing` version of these
    files. Once the files are uploaded, they are available for all Wazo
    installations.
