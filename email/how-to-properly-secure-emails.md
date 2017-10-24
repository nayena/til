# How to properly secure emails

Email is one of those ancient things, that managed to stand the test of
time, but that has not been without a bunch of issues. Spammers typically
use a variety of techniques to spoof email addresses, which IETF (and
some other organisations) try to come up with solutions to combat the
malicious intent from bad people.

Today, you typically see 3 things that needs to be configured on a DNS
level, in order to make email somewhat secure. They are:

## SPF

The Sender Policy Framework is basically a TXT record telling the world,
which servers are allowed to send emails from this domain. This is
typically something that the email server receiving the mail would have
to verify.

Example:

```
$ dig +short TXT protonmail.com | grep spf
"v=spf1 include:_spf.protonmail.ch ~all"
```

For verifying the SPF, you can use a little handy tool to query SPF the
server with the IP the mail was sent from and the email used, like so:

```
$ spfquery --scope mfrom --id <email> --ip <ip> --helo-id <mailserver-id>
pass
protonmail.com: Sender is authorized to use '<email>' in 'mfrom' identity (mechanism 'include:_spf.protonmail.ch' matched)
```

## DKIM

Domain Keys Identified Mail sounds complicated, but is nothing more than
the process of signing and verifying emails. A public/private key pair
is generated, where the public part is public to the world in a TXT record
and only trusted servers have the private key. The server receiving the
email can then in turn decrypt the message using the key and verify the
signature in the mail headers. For more technical details, see the [wiki
page about it](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail#Technical_details).

## DMARC

So what happens if SPF and DKIM fails? Enter DMARC. Using DMARC, you can
decide what should happen if both SPF _and_ DKIM fails. There are three
actions that can be taken by the server: none, quarantine and reject.
_None_ does exactly what you'd suspect: Just deliver the mail as per usual.
_Quarantine_ puts the mail into spam and _reject_ flat out rejects the
email - something that higher profile targets may wish, like paypal or
google.

To test out your email score, you can use [Mail Tester](https://www.mail-tester.com),
which will rate you on a scale from 0 to 10.

I personally use protonmail and they have an [excellent write-up about
the typical anti-spoofing and security measures provided today](https://protonmail.com/support/knowledge-base/anti-spoofing/).