Next-generation Email
---------------------

Web of Trust (WoT) based

All emails must be encrypted and signed

Each address has a public/private key pair
* Generated by server for authorized clients
* May be shared with client to offload encryption

Users may vouch for each other (sign email address)

Senders may obtain pre-send token before sending
* ttl+recipient_sig(to email address, sender email address, ttl)
* valid for length of token (recommendation: 5 min)

TLS verification of hosts
* Receiver must use sni
* Receiver must present valid tls certificate
* Sender must present valid tls certificate
* TLS may be validated via DANE or traditional CA architecture

Sane email definition, case insensitive 
* <?user>[a-z0-9.-]+(<?tag>\+[a-z01.-]+)?@(<?host>[a-z0-9][a-z0-9-.]+
* user + tag < 256 octets
* host < 256 octets

Servers connect to counterparts
* Deliver message
* Request Vouches
 * list(sha256(voucher email) + voucher_sig(vouched email))
* Request Vouched
 * list(sha256(vouched email) + voucher_sig(vouched email))
 * may respond iff request-subject has vouched for requester.
* Send Vouch (voucher mail + voucher_sig(vouched email))
* Request recipient's public key
 * Receiver may return random, but valid value for non-existent addresses
* Sender must have a PTR record
 * Certs on IPs are not valid

Receiving Server Restrictions
* Receiver must not delete an email without user interaction
 * user interaction includes setting up delete filter, e.g. black list
* Receiver must not vouch for email address without user interaction
* Receiver may choose to not accept a message and must tell sender message is being denied.
 * Receiver should provide reason.
* Receiver may choose to accept and discard messages for non-existent users.
* Receiver must honor sender white-list
* Receiver must accept mail with a valid pre-send token
* Receiver may and should use network-layer filtering (as this gives a clear fail to the sender)
 * e.g. ip-based deny, ip-based throttling
 * Receiver should not permanently, or on a long-term basis,  block network addresses
* Receiver must present all accepted messages to use in default mailbox
* Receiver may mark emails as "questionable" if they do not have
 * White-list sender
 * First- or Second- order vouch
 * Message is accompanied by a valid pre-send token
* Receiving server must deny email with a host differing from TLS Cert CN
 * Mailing lists must have a From: of the list
 * Mailing lists should act as if they are forwarding email

Each message must have a static, globally and temporally unique identifier


