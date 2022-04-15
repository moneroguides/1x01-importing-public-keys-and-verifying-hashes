# Getting to Grips with Monero - Importing Public Keys & Verifying Hashes

## Prerequisites:

* gpg2 (Linux)
* Kleopatra (Windows) - [GPG4WIN](https://www.gpg4win.org)

<hr/>

### INTRO

Hello there travelers and welcome to the first installment in the "Getting to grips with Monero" series.

This video is for those of you who want to ensure that the software you're downloading and running is in fact the one that the authors intended you to receive. In other words this is for the security conscious user.

It may seem like a pain at first, but most of what we're doing here is setup and won't be required when verifying files in future. It's also just a good habit to get into and once you get the hang of it, it's a breeze.

Following these steps before running anything on your machine will mean you're safer than if you don't, but please be aware that this is in a relative sense. With these steps you're ensuring that you're running the software the author intended; it isn't a way to ensure that what the author intended is safe.

With that out of the way, let's get on to the matter at hand, the GNU Privacy Guard (GPG).

GPG is a complete and free implementation of the OpenPGP standard that's been in circulation since 1997. 
GPG is free software, meaning that it can be freely used, modified and distributed. I recommend taking a look at [the GNU project and the Free Software Foundation](https://www.gnu.org/gnu/gnu.html). Their webpage, is pretty interesting and you'll find a lot more free software there.

You can also learn more about the OpenPGP standard [on their wiki page](https://en.wikipedia.org/wiki/Pretty_Good_Privacy). All of this is linked in the description.

GPG allows anyone to encrypt and sign their data and communications. You'll typically be prompted to carry out some basic operations when handling Monero-related software so we thought it'd be good to have an easy to follow guide.

If you haven't installed the software for your operating system already, please go ahead and do so now. As with all our videos, you'll find all the required links and terminal commands in the description.

Most Linux distributions will have GPG installed already. If you find that you don't have it, please check your package manager and install it from there. For Windows users you'll need to install Kleopatra.

### CREATING YOUR OWN KEYS

Let's start by opening up a terminal or Powershell window and checking which version of GPG we have installed.

Don't worry that I'm using Windows, these steps are relevant to Linux as well. Feel free to copy and paste along with this video, but bare in mind that it's good practice to double check the content remains unedited.

Let's start by typing `gpg --version` and pressing enter. If you're not familiar with using the command line, please remember, you need to hit enter after each time that you type a command for the terminal to read your input.

If you weren't sure if you had it installed, this is a pretty good test. This command will print some interesting information, including: the version, the licence, the home directory for GPG and the supported algorithms.

Now we've confirmed that it's installed, we're going to create our very own set of shiny new keys.

Keys typically come in pairs, a private key and a public key. In simple terms, a private key is used to prove who you are and a public key is used to tell other people who you are. It's a pretty simple system, but has deep roots in cryptography. If you're interested in reading a little more about the fundamentals, check out the link in the description entitled [How PGP Works](https://users.ece.cmu.edu/~adrian/630-f04/PGP-intro.html).

To generate our own set of keys we're going to use the command `gpg --full-generate-key`, don't forget to include the hyphens.

First it will ask you what kind of key you'd like to create. For the purposes of this video we'll be using the default option 'RSA', so we'll select option 1.

Next it will ask you about the level of encryption you require, the larger the number the more data is used to generate the keys. More data generally means more secure. We're going to choose 4096 bits as it offers the highest level of encryption.

We'll then be asked how long we want our keys to remain valid, this is a personal choice. As there is no limit to the number of keys I can generate and because I won't be hosting my keys publicly, I'm going to choose 0, 'never expires'.

All keys are assigned user IDs. They're typically made up of a name, comment and email address. For the purposes of this video I'm going to use the following credentials. Once we've entered all this information we're going to type 'o' for 'okay', but before we hit enter I want to warn those who like to use password generators: you're going to need to enter a new password in the next step.

Once we've entered and confirmed the password, we're going to generate some entropy, in other words, move the mouse and bash the keyboard a bit. The encryption of your keys is based on the state of your system, moving the mouse, switching windows and typing helps create more randomness.

Now that we've generated our very own keypair, we can use the `--list-keys` flag to verify it's existence; `gpg --list-keys`. During the creation of ours keys, GPG has automatically added them to something called a keyring. The subject of key rings is a little outside the scope of this video, however it's important to know that they may be different and you can create as many as you like. We won't be specifying any in this video and beacuse of that we're going to be working with the default keyring.

### VERIFYING & IMPORTING PUBLIC KEYS

To put everything we've just done into practice we're going to verify the install files for the official Monero software. So let's head over to the official Monero [github repository](https://github.com/monero-project/monero) and click on the "Releases" link on the right hand side. The latest release will be at the top of the page.

Scroll down to "Official Download Links" and download the file that suits you best. For me it's the 64-bit Windows version.

Now that it's downloaded we’ll need two important pieces of information:

- Either a signed copy of the file hashes; or a signature file ending with '.asc'
- The public key of the person who signed them

The first of these is located in the "hashes.txt" which you'll find via a link on the release page. I'm going to right click and "save link as". Let's open it and take a look at what we've got here.

Hashes are unique identifiers that are generated by a hashing algorithm. These algorithms are generated using information about the file's contents so we can detect changes easily and quickly. In our case, we're interested in making sure that the hash of the file we've downloaded matches the hash contained in this file.

If you look at the heading and footer, you'll notice that this file actually comes in the form of a signed message. This is important, because now we can see if these hashes were signed by someone we know. If we take a look just above the signature we can see that the person who signed it was a contributor by the name 'binaryfate'.

This leads us on to the second piece of information we need; Binaryfate's public key. We can find this via the github page we started on. Click on the folder labelled **utils**, then **gpg_keys**. Here we can find a list of all the contributors' public keys. Click on **binaryfate.asc** and then right-click **Raw** and save it as an .asc file.

Next, open your file explorer and navigate to the directory in which you saved these files. Ensure that both files are in the same folder. For me, I've just dumped them on the desktop.

With the file explorer open, shift+right-click and select ‘open Powershell here’. If you're a Linux user, you can 'right-click' and select open terminal here. Since I'm a Windows 11 user it's a bit different for me as I just have to right-click and choose 'Open in Windows Terminal.' In all cases it's a fairly similar process.

At this point we’re interested in taking a look at the fingerprint of binaryfate's public key. Fingerprints are best thought of as summaries of the key. We can view this fingerprint with the following command: `gpg --keyid-format long --with-fingerprint binaryfate.asc`.

Fingerprints are a useful way of making sure that the key you're importing is actually the one you want. If you know the person whose key you're trying to import you can check it with them the old-fashioned way by just asking if this key belongs to them, but you'll often find that a fingerprint will be hosted somewhere on the internet so you won't need to contact anybody directly.

Please bear in mind, if a web page or repo has been compromised and the fingerprint and the public key are located in the same place, the likelihood is that these were compromised too, so checking the fingerprint is pretty much pointless. However, if the fingerprint and key are hosted in multiple locations, it's always worth comparing them to be sure.

In our case, we can find the fingerprint for binaryfate's public key on the [GetMonero](https://www.getmonero.org/resources/user-guides/verification-allos-advanced.html) site. Click on the heading "Verify and Import Signing Key" and scroll down until you find the "Verify the fingerprint matches" section. Now we can visually compare that to the one which is printed in our terminal window. If you've got a match, great, if not, verify the web links you used and report your findings to the community.

Now that we have a little more confidence in the key we've downloaded we're going to use the command `gpg --import binaryfate.asc`. Once again we can use `gpg --list-keys` to verify that it's been added to our keyring.

### SIGNING SOMEONE ELSE'S PUBLIC KEY

This step is in many cases unnecessary and is more relevant to public keyrings and the pgp ecosystem as a whole. It's particularly important for something called the 'web of trust', which is totally outside the scope of this video. However you can learn more about the [web of trust](https://wiki.p2pfoundation.net/Web_of_Trust) through the p2p foundation. For us, going through this step means that you have a more simple log when you verify signatures in the future.

It's possible to add this level of trust to the signature you've imported with the `--edit-key` flag. In our case we'll use the command `gpg --edit-key` along with binaryfate's email address to identify the key:

`gpg --edit-key binaryfate@getmonero.org`.

This command will start up the GPG prompt. You'll know it's running when you see `gpg>`, you now have a few options including the ability to sign it with your own key, type ‘help’ to find out what else you can do.

For now, let’s type 'sign', confirm with 'y' and then enter the password you set up in the previous step. Don't worry if you see `failed: Access is denied`. This seems to happen every time the password prompt appears and does not mean the operation has failed. If you try to sign the key again you will be able to confirm whether or not the first attempt was successful

All done.

Use 'ctrl+c' to exit the gpg prompt.

### VERIFYING SIGNATURES & HASHES

Everything we've done so far has now given us the ability to verify the signature in the text file we downloaded earlier.

The next command we're going to use is `gpg --verify hashes.txt`. If you're typing this out yourself, remember that you can use 'TAB' to autocomplete file names.

If everything has gone to plan, we should see a message stating 'Good signature from "binaryFate <binaryfate@getmonero.org>"'.

If you don't receive this message, please be sure to verify the links you used and report your findings to the community.

Now we can compare the hashes in the text file to those of the file we downloaded.

If you're using Linux you can use the command `sha256sum --check hashes.txt`, then look for the version we downloaded and check to see if everything is "OK".

If you're using Windows, use the command `Get-Filehash` and then the name of the file you downloaded. Manually compare this hash to the one in the hashes.txt file.

GREAT! Now we know the software that we have is the one intended by the person who signed these hashes.

### SIGNING AND DECRYPTING MESSAGES

GPG has a lot of interesting features and gives users the ability to sign, encrypt and decrypt messages.

The 'hashes.txt' file we used earlier was signed with GPG by its creator, this also applies to our [contact details](https://moneroguides.org/about/) on our website and the scripts for our videos.

Let's have a go at signing our own "clear text" message. First create a text file called anything you like. Here's one I made earlier. Then use the command: `gpg --user * --clearsign message.txt`. The asterisk ( * ) should be replaced by the name given to the private key you wish to use. The file name message.txt can be replaced by the name of whatever file you wish to sign. If you created multiple key pairs and you're unsure of your user name, you can use the command `gpg --list-keys` as we've done previously.

Once the command is finished running you should have a new file in the working directory. It should have the same format as your original file with the suffix ".asc".

It's also possible to encrypt and decrypt messages. The most convenient way to share these messages is using the ACSII armor function. The following command will create an encrypted message using the monero-guides private key: `gpg --user monero-guides --sign --armor message.txt`. Anyone who has our public key, is able to decrypt this message.

Decrypting these messages is rather simple. All we need to do is use the command `gpg --decrypt message.txt.asc`.

Another great feature is the ability to encrypt messages which can only be decrypted by specific people. To do this, you need the public key of the recipient. In the next example we're going to encrypt a message that can be decrypted by only us at monero-guides: `gpg --user monero-guides --recipient monero-guides --sign --armor message.txt`.

Watch out for the easter eggs coming up in the following videos. There'll be a nice reward for a lucky viewer who's been paying attention.

### OUTRO

To close out this tutorial we'll leave you with this piece of advice: PGP relies heavily on trust so you should be sure that the public key you have in your keyring is in fact trustworthy. If it isn't, then you may as well skip everything in this guide and accept the potential risks of running an unverified file. Although everything in this tutorial is optional, it is best practice and greatly reduces the capabilities of bad actors to infiltrate your machine.

The instructions in this video will be relevant to a lot of our other videos so please make yourself familiar with GPG.

Anyway, that's it for this installment, may your newfound GPG powers bring you peace and privacy!

~moneroguides
