#Clear HSTS state (HTTP Strict Transport Security) in Chrome

Once you have visited the HTTPS version of a certain website, you might not be able to visit the HTTP version of the same.
This is because Google Chrome remembers this and routes you to the most safest version (HTTPS) for that domain. (and annoyingly also apply this to sub domains, regardless if they have HTTPS enabled or not) This is part of the HSTS protocol and usually a good thing.

Sometimes however you just want your browser to do what you instruct it to do. Here is hot to remove specific domain from HSTS


* Step-1: Point your browser to: [chrome://net-internals/#hsts](chrome://net-internals/#hsts)
* Step-2: Under ‘Delete Domain’, type the domain you try to access in HTTP
* Step-3: Click ‘delete’

Source(s):
* [Chrome clear HSTS state (HTTP Strict Transport Security)](https://kamaradski.com/2856/chrome-clear-hsts-state-http-strict-transport-security)
