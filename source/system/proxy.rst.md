# Proxy Configuration

If you use Wazo behind an HTTP proxy, you must do a couple of
manipulations for it to work correctly.

## apt

Create the <span data-role="file">/etc/apt/apt.conf.d/90proxy</span>
file with the following
    content:

    Acquire::http::Proxy "http://domain\username:password@proxyip:proxyport";

## provd

Proxy information is set with `xivo-provd` endpoint
`/provd/configuration/http_proxy`.

## dhcp-update

*This step is needed if you use the DHCP server of the Wazo. Otherwise
the DHCP configuration won't be correct.*

Proxy information is set via the
<span data-role="file">/etc/xivo/dhcpd-update.conf</span> file.

Edit the file and look for the `[proxy]` section.

## xivo-fetchfw

*This step is not needed if you don't use xivo-fetchfw.*

Proxy information is set via the
<span data-role="file">/etc/xivo/xivo-fetchfw.conf</span> file.

Edit the file and look for the `[proxy]` section.
