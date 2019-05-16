# Asterisk Voicemail

## Delete voicemail

### Query

    GET /delete_voicemail

### Parameters

#### Mandatory

  - name  
    the voicemail name

#### Optional

  - context  
    the voicemail context (default is 'default')

### Errors

<table style="width:83%;">
<colgroup>
<col style="width: 18%" />
<col style="width: 22%" />
<col style="width: 43%" />
</colgroup>
<thead>
<tr class="header">
<th>Error code</th>
<th>Error message</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>404</p>
</blockquote></td>
<td>Not found</td>
<td>The voicemail does not exist</td>
</tr>
</tbody>
</table>

### Example requests

    GET /delete_voicemail HTTP/1.1
    Host: wazoserver
    Accept: application/json

### Example response

    HTTP/1.1 200 OK
    Content-Type: application/json
    
    {
        nothing
    }
