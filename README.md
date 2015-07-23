# Gomail
[![Build Status](https://travis-ci.org/go-gomail/gomail.svg?branch=v2-unstable)](https://travis-ci.org/go-gomail/gomail) [![Code Coverage](http://gocover.io/_badge/github.com/go-gomail/gomail)](http://gocover.io/github.com/go-gomail/gomail) [![Documentation](https://godoc.org/github.com/go-gomail/gomail?status.svg)](https://godoc.org/github.com/go-gomail/gomail)

## Introduction

Gomail is a simple and efficient package to send emails. It is well tested and
documented.

It is versioned using [gopkg.in](https://gopkg.in) so I promise
they will never be backward incompatible changes within each version.

It requires Go 1.2 or newer.


## Features

Gomail supports:
- Attachments
- Embedded images
- HTML and text templates
- Automatic encoding of special characters
- SSL and TLS
- Sending multiple emails with the same SMTP connection
- Any method to send emails: SMTP, postfix (not included but easily doable), etc


## Documentation

https://godoc.org/gopkg.in/gomail.v2-unstable


## Download

    go get gopkg.in/gomail.v2-unstable


## Example

```go
package main

import (
	"gopkg.in/gomail.v2-unstable"
)

func main() {
	msg := gomail.NewMessage()
	msg.SetHeader("From", "alex@example.com")
	msg.SetHeader("To", "bob@example.com", "cora@example.com")
	msg.SetAddressHeader("Cc", "dan@example.com", "Dan")
	msg.SetHeader("Subject", "Hello!")
	msg.SetBody("text/html", "Hello <b>Bob</b> and <i>Cora</i>!")
	msg.Attach(gomail.NewFile("/home/Alex/lolcat.jpg"))

	d := gomail.NewPlainDialer("smtp.example.com", "user", "123456", 587)

	// Send the email to Bob, Cora and Dan
	if err := d.DialAndSend(msg); err != nil {
		panic(err)
	}
}
```


## FAQ

### x509: certificate signed by unknown authority

If you get this error it means the certificate used by the SMTP server is not
considered valid by the client running Gomail. As a quick workaround you can
bypass the verification of the server's certificate chain and host name by using
`SetTLSConfig`:

    d := gomail.NewPlainDialer("smtp.example.com", "user", "123456", 587)
    d.TLSConfig = &tls.Config{InsecureSkipVerify: true}

Note, however, that this is insecure and should not be used in production.


## Contribute

See [CONTRIBUTING.md](CONTRIBUTING.md).


## Change log

See [CHANGELOG.md](CHANGELOG.md).


## Contact

You can ask questions on the [Gomail
thread](https://groups.google.com/d/topic/golang-nuts/ywPpNlmSt6U/discussion)
in the Go mailing-list.


## Support

If you want to support the development of Gomail, I gladly accept donations.

I will give 100% of the money I receive to
[Enfants, Espoir Du Monde](http://www.eedm.fr/).
EEDM is a French NGO which helps children in Bangladesh, Cameroun, Haiti, India
and Madagascar. All its members are volunteers so its operating costs are only
1.9%. So your money will directly helps children of these countries.

As an added bonus, your donations will also tip me by lowering my taxes :smile:
I will send an email with the receipt of the donation to EEDM annually to all
donors.

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=PYQKC7VFVXCFG)
