# xivo-dird-phoned

xivo-dird-phoned is an interface to use directory service with phone. It
offers a simple REST interface to authenticate a phone and search result
from <span data-role="ref">wazo-dird</span>.

## Usage

xivo-dird-phoned is used through HTTP requests, using HTTP and HTTPS.
Its default port is 9498 and 9499. As a user, the common operation is to
search through directory from a phone. The phone need to send 2
informations:

  - \`xivo\_user\_uuid\`: The Wazo user uuid that the phone is
    associated. It's used to search through personal contacts (see
    <span data-role="ref">dird\_services\_personal</span>).
  - \`profile\`: The profile that the user is associated. It's used to
    format results as configured.

<div class="note">

<div class="admonition-title">

Note

</div>

Since most phones dont't support HTTPS, a small protection is to
configure authorized\_subnets in
<span data-role="ref">configuration-files</span>

</div>

## Launching xivo-dird-phoned

On command line, type `xivo-dird-phoned -h` to see how to use it.
