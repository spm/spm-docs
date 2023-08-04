# Key generation for Linux users

You must generate a key to be able to clone and work with the SPM repository. To do this, in your GitHub account go to “**Organizations**” and then in the left panel press “**SSH and GPG keys**”. In “**SSH keys**”, press the green button “**New SSH key**”

Add a title to your key in the field “**Title**”. Then, in the field “**Key**”, copy-paste the key you will generate with the following steps: 

1. Open a terminal and in your home directory type:

```bash
ssh-keygen -t ed25519 -C *your_email*
```
It will ask you to “Enter passphrase (empty for no passphrase):”. You can press enter and not create a passphrase or set one that you will have to confirm later. 

The output will be similar to this:

```bash
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/*your_user*/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/*your_user*/.ssh/id_ed25519
Your public key has been saved in /home/*your_user*/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:T3kB6rMhciPlSqcHp3SDmmoFu+ZDqs7IxFVcuj4JNeY *your_email*
The key's randomart image is:
+--[ED25519 256]--+
|       .  .      |
|    . o  . .     |
|   . S  .   .    |
| .. * o.   . .   |
|  o* E. D o .    |
|..=oB.+. * .     |
| +=*.X  . .      |
|**. *.o          |
|X= u.u           |
+----[SHA256]-----+
```

2. In the repertory `/home/*your_user*/.ssh` now you will have 2 new files similar to these:

```bash
id_ed25519  id_ed25519.pub
```

3. The public key is in the file with extension `.pub`. To read it, open the file or type:

```bash
cat .ssh/id_ed25519.pub_
```
The output/key should be something similar to this:

```bash
ssh-ed25519 AAAAC5NzaC1lZDI1NTE4AAAANI/iaJJ7eHWs+i2hxxQTX+xcalwd+QlhDSwgIkh3cvEc *your_email_here*
```
4. Copy this key and paste it in the field “**Key**” in your GitHub account.

Finally, to clone the GitHub repository, create a new work directory for SPM in your computer and move there in a terminal. Then type:

```bash
git clone git@github.com:spm/spm.git
```

You will have to enter the passphrase you created previously. And that’s all!
