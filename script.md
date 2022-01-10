### Prerequisites:

* gpg2 (Linux)
* Kleopatra (Windows) - [GPG4WIN](https://www.gpg4win.org)

.....................................................

### INTRO

Hello there travelers and welcome to the first installment in the "Getting to grips with Monero" series.

This video is for those of you who want to ensure that the software you're downloading and running is in fact the one that the authors intended you to receive. In other words this is for the security conscious user. It may seem like a pain at first, but it's a good habit to get into and once you get the hang of it, it's a breeze.

Following these steps before running anything on your machine will mean you're safer than if you didn't, but please be aware that this is in a relative sense. With these steps you're ensuring that you're running the software the author intended, but it is not a way to ensure that what the author intended is safe.

With that out of the way, let's get on to the matter at hand; The GNU Privacy Guard (GPG).

GPG is a complete and free implementation of the OpenPGP standard that has been in circulation since 1997. 
GPG is free software, meaning that it can be freely used, modified and distributed. I recommend taking a look at [the GNU project and the Free Software Foundation](https://www.gnu.org/gnu/gnu.html). Their webpage, is pretty interesting and you'll find a lot more free software there.

You can also learn more about the OpenPGP standard [on their wiki page](https://en.wikipedia.org/wiki/Pretty_Good_Privacy).

GPG allows anyone to encrypt and sign their data and communications. You'll typically be prompted to carry out some basic operations when handling Monero-related software so we thought it would be good to have an easy to follow guide.

If you haven't installed the software for your operating system already, please go ahead and do so now. As with all our videos, you'll find all the required links in the description.

Please be aware that most Linux distributions will have GPG installed already. If you find that you don't have it, please check your package manager and install it from there.


### CREATING YOUR OWN KEYS

Let's start by opening up a terminal or Powershell window and checking which version of GPG we have installed.
Don't worry that I'm using ubuntu, this is relevant to all OS's. If you're a windows user, please open a Powershell window instead.
Do take a look at the video description as most of the commands we’re going to use can be found there. Feel free to copy and paste along with this video.

Let's start by typing `gpg --version` and pressing enter. If you're not familiar with using the command line, please remember, you need to hit enter after each time that you type a command for the terminal to read your input. 

If you weren't sure if you had it installed, this is a pretty good test. This command will print some interesting information, including the version, the licence, the home directory for GPG and the supported algorithms.

Now we've confirmed that it's installed, we're going to create our very own set of shiny new keys.

Keys typically come in pairs: a private key and a public key. In simple terms, a private key is one which you use to prove who you are and a public key is one which is used to tell other people who you are. It's a pretty simple system, but has deep roots in cryptography. If you're interested in reading a little more about the fundamentals, check out the link in the description entitled [How PGP Works](https://users.ece.cmu.edu/~adrian/630-f04/PGP-intro.html).

To generate our own set of keys we're going to use the command `gpg --full-generate-key`, don't forget to include the hyphens 

First it will ask you what kind of key you'd like to create. For the purposes of this video we'll be using the default option 'RSA', so we'll select option 1. Next it will ask you about the level of encryption you require, the larger the number the more data is used to generate the keys. More data generally means more secure. We're going to choose 4096 bits as it offers the highest level of encryption.

We will then be asked how long we want our keys to remain valid. This is a personal choice, but in this case because there is no limit to the number of keys I can generate and because I won't be hosting my keys publicly, I'm going to choose 0, 'never expires'.

All keys are assigned user IDs. They're typically made up of a name, comment and email address. For the purposes of this video, this name and email address could be quite literally anything. Your own credentials could even be as crazy as mine. Once we've entered all this information we're going to type 'o' for 'okay', but before we hit enter I want to warn those who like to use password generators: you're going to need to enter a new password in the next step.

Once we've entered and confirmed the password, we're going to generate some entropy, in other words, move the mouse and bash the keyboard a bit. The encryption of your keys is based on the state of your system, moving the mouse and switching windows helps create more randomness.

Now that we've generated our very own keypair, we can use the `--list-keys` flag to verify it's existence: `gpg --list-keys`. After the creation of ours keys, GPG has automatically added them to something called a keyring. The subject of key rings are a little outside the scope of this video, however it's important to know that they may be different and you can have as many as you like. As we won't be specifying any in this video, we're going to be working with the default keyring.


### VERIFYING & IMPORTING PUBLIC KEYS

To put everything we've just done into practice we're going to verify the install files for the official Monero software. So let's head over to the official Monero [github repository](https://github.com/monero-project/monero) and click on the "Releases" link on the right hand side. The latest release will be at the top of the page. Scroll down to "Official Download Links" and download the file that suits you best. For me it's the 64-bit Linux version.
Now that it's downloaded we’ll need two more pieces of information. This is important, every time we verify a file using this process we will need two things:

- The hashes
- The public key

The first of these is located in the "hashes.txt" which you'll find via a link in the video description. I'm going to right click and "save link as". Let's open it and take a look at what we've got here. Hashes are unique identifiers that are generated by a hashing algorithm. These algorithms are generated using information about the file's contents so we can detect changes easily and quickly. We're interested in making sure that the hash of the file we've downloaded matches the hash contained in this file.

If you look at the heading and footer, you'll notice that this file actually comes in the form of a signed message. This is important, because now we can see if these hashes were signed by someone we know. If we take a look just above the signature we can see that the person who signed it was a contributor by the name 'binaryfate'. 

The second piece of information we need is binaryfate's public key. We can find it via the github page we started on. Click on the folder labelled "utils", then "gpg_keys". Here we can find a list of all the contributors' public keys. Right click on binaryfate's name and select "save link as".

Next, open your file explorer and navigate to the directory in which you save these files. If you haven't saved them to the same place, please move them now.

With the file explorer open, right click and select ‘open terminal here’. If you're a windows user, you'll need to use 'shift+right click' and select open powershell here.

At this point we’re interested in taking a look at the fingerprint of binaryfates public key. Fingerprints are best thought of as summaries of the key.
We can view this fingerprint with the following command, `gpg --keyid-format long --with-fingerprint binaryfate.asc`

Fingerprints are a useful way of making sure that the key you're importing, is actually the one you want. If you know the person whose key you are trying to import you can check it with them manually.
you'll often find that a fingerprint will be hosted somewhere on the internet and you don't need to go through that manual process.

Please bear in mind, if a webpage or repo has been compromised and the fingerprint and the public key are located in the same place, the likelihood is that both were compromised, so checking the fingerprint is pretty much pointless. However, if the fingerprint and key are hosted in seperate locations, it's always worth checking.

In our case, we can find the fingerprint for binaryfates public key on [Get Monero](https://www.getmonero.org/resources/user-guides/verification-allos-advanced.html).

Click on the heading 'Verify and Import Signing Key', scroll down until you find this section 'Verify the fingerprint matches'. Now let's visually compare that to the one which is printed in our terminal window.
If you've got a match, great, if not, verify the web links you used and report your findings to the community.

Now that we have a little more confidence in the key we've downloaded we're going to use the command `gpg --import binaryfate.asc`.
Once again we can use `gpg --list-keys` to verify that it has been added to our keyring.


### SIGNING SOMEONE ELSE'S PUBLIC KEY

This step is a little unnecessary and is more relevant to public keyrings and the pgp ecosystem as a whole, namely the 'web of trust', which is totally outside the scope of this video, but you can learn more about the [web of trust](https://wiki.p2pfoundation.net/Web_of_Trust) through the p2p foundation.
For us, going through this step means that you have a more simple log when you verify signatures in the future.

It is possible to add this level of trust to the signature you have imported with the `--edit-key` flag. 
In our case we will use the command `gpg --edit-key binaryfate@getmonero.org`
Once GPG is running you'll see 'gpg>', you now have a few options including the ability to sign it with your own key, type ‘help’ to find out what else you can do.

For now, let’s type 'sign', confirm with 'y' and now enter the password you set up in the previous step.

All done

Now use 'ctrl+c' to exit gpg


### VERIFYING SIGNATURES & HASHES

Everything we've done so far has now given us the ability to verify the signature on the hash file we downloaded earlier.

The next command we're going to use is `gpg --verify hashes.txt`, if you're typing this out yourself, remember that you can use 'TAB' to autocomplete.

If everything has gone to plan, we should see a message stating 'Good signature from "binaryFate <binaryfate@getmonero.org>"'.

If you don't receive this message, please follow the procedure recommended upon failed fingerprint verification.

GREAT!

Now we can compare the hashes in the text file to those of the file we downloaded.

If you're using linux you can use the command `sha256sum --check hashes.txt`, we're going to look for the version we downloaded and check to see if everything is "OK".

If you're using windows, please use the command `Get-Filehash` and then the name of the file you downloaded. Manually compare this hash to the one in the hashes.txt file.

Now we know, the software that we have is the one intended by the person who signed these hashes!


### SIGNING AND DYCRYPTING MESSAGES

GPG has a lot of interesting functions which include the ability to sign, encrypt and decrypt messages.

We at Monero Guides host our [public key](https://github.com/moneroguides/moneroguides-assets/blob/main/monero-guides.asc) on github which you can use it to verify the contact details on our websites [about page](https://moneroguides.org/about/) and our scripts.

To do this, import the monero-guides public key into your keyring and then save the signed message into a text file. I'm going to create a new text file called "message.txt". If everything is done correctly it can verifed using the previous steps.

You can sign your own "clear text" messages too using the command `gpg --user * --clearsign message.txt`. The * (wildcard) should be replaced by the name given to the private key you wish to use. If you create multiple key pairs and you're unsure of your user name, you can use the command `gpg --list-keys` as we've done previously.

As just mentioned it is also possible to encrypt and decrypt messages. The most convenient way to share these messages is using the ACSII armor function. The following command allows someone who has the monero-guides public key to decrypt any open message from us `gpg --user monero-guides --sign --armor message.txt`. 

Once the command is finished running you should have a new file in the working directory. It should have the same format as your original file with the suffix ".acs".

Another great feature of GPG is the ability to encrypt messages which can only be decrypted by specific people. To do this, you need the public key of the recipient. In the next example we're going to encrypt a message that can be decrypted by only us at monero-guides decrytp `gpg --user monero-guides --recpient monero-guides --sign --armor message.txt`.

It's pretty simple to decrypt these messages. All we need to do is use the command `gpg --decrypt message.txt.acs`.

Watch out for an easter egg coming up in the following few videos. There will be a nice reward for a lucky viewer who's been paying attention.


### OUTRO

PGP relies heavily on trust, it's for that reason you should be sure that the public key you have in your keyring is in fact trustworthy.

If you’re not, then you may as well skip this step and accept the consequences. That being said, checking the software you download is best practice.

The instructions in this video will be relevant to a lot of our other videos so please make yourself familiar with GPG.

Anyway, that's it for this installment, peace be with you privacy fans.
