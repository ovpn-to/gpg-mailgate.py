gpg-mailgate is a gateway for Postfix that uses the GNU Privacy Guard application to encrypt e-mails before being sent to the next hop.

### INSTALLATION: ###
  1. Ensure that GPG is installed and configured.
    1. Make sure public keys for all of your potential recipients are available in the GPG home directory you use in step 2
  1. Configure /etc/gpg-mailgate.conf based on the provided sample config
  1. Place gpg-mailgate.py in /usr/local/bin/
  1. Place the GnuPG directory in /usr/local/lib/python2.5/site-packages
  1. Add the following to the end of /etc/postfix/master.cf
```
gpg-mailgate    unix    -       n       n       -       -       pipe
        flags= user=nobody argv=/usr/local/bin/gpg-mailgate.py

127.0.0.1:10028 inet    n       -       n       -       10      smtpd
        -o content_filter=
        -o receive_override_options=no_unknown_recipient_checks,no_header_body_checks
        -o smtpd_helo_restrictions=
        -o smtpd_client_restrictions=
        -o smtpd_sender_restrictions=
        -o smtpd_recipient_restrictions=permit_mynetworks,reject
        -o mynetworks=127.0.0.0/8
        -o smtpd_authorized_xforward_hosts=127.0.0.0/8
```
  1. Add the following to /etc/postfix/main.cf
```
content_filter = gpg-mailgate
```
  1. Restart postfix.