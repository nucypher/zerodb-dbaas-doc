ZeroDB-as-a-service's documentation
===============================================

Account
========

Using passphrase or certificates for encryption and authentication
---------------------------------------------------------------------
In ZeroDB, 2 ways of operation are possible. Using GPG keys and using
passphrases.

.. note:: Since we both encrypt and authenticate, GPG keys are a better choice
          than SSL certificate because the former contain both encryption and
          signing keys, the latter - only signing keys.

Registration
--------------

For database as a service, email address IS the username. There are two way to
work with user accounts: command line and in a browser.

Website
`````````
You can choose whether to use passphrase or public key to authenticate.

Passphrase
'''''''''''''
If passphrase is chosen, an Ed25519 private key is derived from it using scrypt
as a hashing algorithm and email address as salt. Private key is calculated
right in the browser, public key is derived from it and sent to the server,
along with email address. Private key is not saved anywhere.

GPG key
'''''''''''''
If GPG key way is chosen, user uploads a GPG key in export format. Server saves
only the signing part to confirm user's identity. Alternatively, there are ways
to generate a GPG key right in the browser without talking to the server.

Confirmation
'''''''''''''
The server confirms existence of email address in a standard way (sending a
registration confirm email). After that, the account can be used.

Command line
```````````````
Command-line interface works in conceptually a similar way as in the browser.
However, everything happens in a Python script. One installs necessary scripts
as::

    $ sudo pip install zerodb-cli

and uses as::

    $ zerodb-cli register president@whitehouse.gov [--no-gpg]
    Warning: no GPG key found for this email address! Using a passphrase
    Enter the passphrase:
    Repeate the passphrase:

    Account requested for president@whitehouse.gov
    Look for confirmation email in your inbox!

or::

    $ zerodb-cli register president@whitehouse.gov [--use-gpg]
    Account requested for president@whitehouse.gov
    Look for confirmation email in your inbox!

If a GPG key exists for the email address in a local keyring, it should be used
(unless --no-gpg is specified). Otherwise, a passphrase is used for account
creation.

Changing passphrase or certificate
------------------------------------

Usage and billing
-------------------

Hosted ZeroDB server
======================

Single-user server
---------------------

Multi-user server
---------------------

User management
`````````````````

Time machine
--------------

ZeroDB client
===============

Hosted client (for development)
----------------------------------

Local client
--------------

Interfacing from non-Python languages
---------------------------------------

JSON API
``````````

JavaScript wrapper for JSON API
`````````````````````````````````

Writing applications with ZeroDB
==================================

Command-line application example
----------------------------------

GUI application example (Electron)
------------------------------------
