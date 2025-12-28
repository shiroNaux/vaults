---
tags:
  - networking
  - dns
  - tls
---
# Introduction

**SNI (Server Name Indication)** is an extension to the TLS protocol that lets your browser tell a server _which hostname you’re trying to reach_ **at the very start** of the TLS handshake.

Think of it as adding an apartment number when delivering a package. Without it, the server only sees the building’s street address (the IP), not which apartment (domain) you want.

# Advantages

Before SNI existed, HTTPS had a big limitation:

- Many websites share a single IP address (virtual hosting).
    
- Each HTTPS site has its own TLS certificate.
    
- But the TLS handshake happens **before** the browser sends the HTTP Host header.
    
- So the server didn’t know _which_ certificate to present.
    

This caused **common name mismatch errors** — the browser would say “Your connection is not private” because it got the wrong certificate.

**SNI fixes this** by putting the hostname directly into the TLS ClientHello message, so the server can choose the correct certificate immediately.

# References
1. https://www.cloudflare.com/learning/ssl/what-is-sni/