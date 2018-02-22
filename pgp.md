How to use PGP
===
This is simple guide on how to use PGP on your day to day work

Installation
---
It's better to install `paperkey` together for managing your private secret key, e.g store on paper.

Note: this note is based on gnupg 2.2. Some commands are not avaiable for lower version

Linux (centos, redhat):
    
    $ yum install -y gnupg2 paperkey

Mac 

    $ brew install gnupg2 paperkey


Key generation
---

**Primary**

Generate your key (this is your ultimate master key!). Do change the key size to 4096.

    $ gpg --full-generate-key

Follow the prompts, make sure you have a good passphrase as it's your last line of defence.

Back up your secret key using paperkey. 

    $ gpg --export-secret-key [key id] | paperkey > your-secret-paperkey.asc

Print out `your-secret-paperkey.asc` on a paper, write down your passphrase on the margin, fold it, put it in
an envelope and hide it :D

Back up your public key, storing it somewhere for distribution or send it to key servers

    $ gpg --export [key id] > your-public-key.asc

or
1. Send to default key server
  
        $ gpg --send-keys [key id]

2. Send to other key server, e.g. pgp.mit.edu

        $ gpg --keyserver keyserver.com --send-keys [key id]

**Subkeys**

This is the signing key you should be using every day. Better to give it an expiry date

    $ gpg --edit-key [key id]

    $ gpg> addkey
    Please select what kind of key you want:
    (3) DSA (sign only)
    (4) RSA (sign only)
    (5) Elgamal (encrypt only)
    (6) RSA (encrypt only)
    Your selection? 4

    Key is valid for? (0) 2y

    $ gpg> save

**Remove secret key only use subkeys on your laptop**

Export subkeys

    $ gpg --export-secret-subkeys [key id] > subkeys

Remove master secret key (make sure you already have it backup!)

    $ gpg --delete-secret-key [key id]

Import subkeys

    $ gpg --import subkeys

If you import subkeys to a differen machine, you will need to import the public key as well. E.g import from a key server:

    $ gpg --keyserver keyserver.com --recv [public key id]

**Restore secret key**

Restore the secret key

    $ paperkey --pubring your-publickey.asc --secrets your-secret-paperkey.asc --output secret-key.asc

Import your secret key

    $ gpg --import secret-key.asc

If the uid shows `[ unknown]`, you can make a decision by:

    $ gpg --edit-key [key id]
    $ gpg> trust

Signing your git commit
---
If your don't want to use passphrase every time when you commit, you can update the signing key passphrase to empty. However you are at your own risk as if someone obtain your no passphrase signing key, they can pretend to be you.

    $ git config --global user.signingkey [your signing key id]
    
Then use `-S` on your commit. 

**Note**: If you have passphrase, make sure you have run `export GPG_TTY=$(tty)` or put that in your bash profile

    $ git commit -S -m 'message' 

For git version < 2, you will need to use alias `commit -S` command for auto signing. For git version 2, auto signing everytime without using `-S` option, run

    $ git config --global commit.gpgsign true
