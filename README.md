Next-generation Email
---------------------

Web of Trust (WoT)

Each address has a public/private key pair
* Generated by server for authorized clients
* May be shared with client to offload encryption
* All emails must be encrypted and signed

All Messages are encrypted

Users may vouch for each other (sign email address pair)

Senders may obtain pre-send token before sending
* ttl+sig(to email address, sender email address, ttl)
* valid for length of token (recommendation: 5 min)

TLS verification of hosts
* Receiver must use sni
* Receiver must present valid tls certificate
* Sender must present valid tls certificate
* TLS may be validated via DANE or traditional CA architecture

Sane email definition, case insensitive (<?user>[a-z0-9.-]+(<?tag>\+[a-z01.-]+)?@(<?host>[a-z0-9][a-z0-9-.]+)
* user + tag < 256 octets
* host < 256 octets

Servers connect to counterparts
* Deliver message
* Request Vouches
 * list(sha256(voucher email) + sig)
* Request Vouched address
 * list(sha256(vouched email) + sig)
* Send Vouch (voucher mail + sig)
* Request recipient's public key
 * Receiver may return random, but valid value for non-existent addresses

Restrictions
* Receiver must not delete an email w/o user interaction (u.i. inc. setting up del filter)
* Receiver must not vouch for email address w/o user interaction
* Receiver may choose to not accept a message. Receiver should provide reason.
* Receiver may choose to accept and discard messages for non-existent users.
* Receiver must honor sender whitelist
* Receiver must accept mails with a valid pre-send token