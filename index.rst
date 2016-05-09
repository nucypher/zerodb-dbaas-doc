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

Data encryption key (the actual symmetric key to encrypt data) is stored on the
server, encrypted with a symmetric key equal to private key from the
passphrase. We don't need public key encryption here because we don't want
somebody else to be able to write to our index on our behalf.

GPG key
'''''''''''''
If GPG key way is chosen, user uploads a GPG key in export format. Server saves
only the signing part to confirm user's identity. Alternatively, there are ways
to generate a GPG key right in the browser without talking to the server.

Private data encryption key is used to encrypt a random symmetric key to
encrypt the data, and that is stored at the server side during the registration
as well.

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

    $ zerodb-cli register president@whitehouse.gov [--use-gpg] [--keyfile ...]
    Account requested for president@whitehouse.gov
    Look for confirmation email in your inbox!

If a GPG key exists for the email address in a local keyring, it should be used
(unless --no-gpg is specified). Otherwise, a passphrase is used for account
creation.

Changing passphrase or GPG key
------------------------------------

Changing passphrase or certificate can be done either in web UI or from a CLI,
similarly to how it works in registration (apart from confirmation email).

Another difference is that we decrypt and encrypt a data encrypting key with
new key from certificate (or passphrase). So, the user doesn't have to
re-encrypt all his data when changing the gpg key or passphrase: everything
happens instantly.

Changing passphrase in the CLI::

    $ zerodb-cli new-passphrase president@whitehouse.gov
    Enter current passphrase:
    Enter new passphrase:
    Repeat the new passphrase:
    Passphrase for president@whitehouse.gov was changed!

Or changing GPG key (or switching to using gpg) from the CLI. Old key is not::

    $ zerodb-cli new-key president@whitehouse.gov
                 --old-key <key_id or dump> / --old-passphrase
                 --new-key <key_id or dump>

Usage and billing
-------------------
Users of ZeroDB service are able to pre-reserve disk space for the database.
Also web UI shows how much of the space is currently used.

Also (because Amazon pricing depends on it) the server collects statistics
about traffic and number of transactions. Even when this information is not
used for pricing, it is good to know for statistics.

Hosted ZeroDB server
======================

Single-user server
---------------------
Single-user is for personal use and development activities. User has a
email/passphrase pair (or gpg key). But this regime is not suitable for
companies which want to build end-to-end encrypted applications for multiple
users, for example.

Also in single-user deployment, user could be physically co-located with others
in the same database without necessarily knowing it.

Multi-user server
---------------------
Multi-user account can create and manage other users. This could be done
through web interface or CLI using zerodb-manage tool::

    $ zerodb-manage console email@domain.com
    >> ls
    alice@nist.gov
    bob@mit.edu
    charlie@google.com
    >> userdel charlie@google.com
    User charlie@google.com has been removed
    >> useradd eve@microsoft.com
    Enter path to the key file or passphrase:
    User eve@microsoft.com was successfully added
    >> chkey eve@microsoft.com
    Path to old key:
    Path to new key:

.. note:: Multi-user account gets, in a way, some powers we have: power to
          create and manage single users.

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
