#### Why Use Tails OS?

```
amnesia, noun:
forgetfulness; loss of long-term memory.

incognito, adjective & adverb:
(of a person) having one's true identity concealed.
```

Tails is a live system that aims to preserve your privacy and anonymity. It helps you to use the Internet anonymously and circumvent censorship almost anywhere you go and on any computer but leaving no trace unless you ask it to explicitly.

It is a complete operating system designed to be used from a USB stick or a DVD independently of the computer's original operating system. It is Free Software and based on Debian GNU/Linux.

Tails comes with several built-in applications pre-configured with security in mind: web browser, instant messaging client, email client, office suite, image and sound editor, etc.

The key benefits to using Tails are:

* Online anonymity and censorship circumvention

* Can be used anywhere and leave no physical trace

* State-of-the-art cryptographic tools

Feel free to read more at Tails' website!

https://tails.boum.org/about/index.en.html

https://tails.boum.org/doc/index.en.html

https://tails.boum.org/doc/about/warning/index.en.html

#### How to run SSH for GIT in Tails

- Step 1 You download Tails [The instructions given on the Tails site explain it clearly](https://tails.boum.org/install/). Otherwise, if you would like to clone a copy of tails just ask me how!



- Step 2 Configure your persistent volume! This will allow you to save your repositories from Github, and also will keep your SSH configuration files.
  - 2A Once you have logged in for the first time, go to the applications tab in the top left corner, and under the "System Tools" menu, select "Configure persistent volume"
  - 2B When specifying the files you wish to save, select SSH Client, GNUPG, and dotfiles (And any others you would like to have persisted)
  - 2C Create a secure passphrase. You will need to enter this any time you wish to interact with your encrypted files. [An example is Diceware](http://world.std.com/~reinhold/diceware.html)
  - 2End That's all for step 2! Whenever you boot into Tails now, you will receive a dialog upon booting asking if you would like to use your persistent volume, or the amnesic version that doesn't save anything. When using SSH and GIT you will want to use your persistent volume. **RESTART TAILS AT THIS POINT** Login with your persistent volume and configure an adminstrator password (Press the + in the bottom left)
  
  
* Step 3 Generate an RSA Key for Github.
  * 3A I'll refer you to [Hub's own guide](https://help.github.com/articles/connecting-to-github-with-ssh/) on how to generate an SSH key, but here's the command you will want to enter into BASH
  
  ```
  ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
  Enter a file in which to save the key (~/.ssh/id_rsa) is the path you should use, you may need to enter this manually.
  Enter passphrase (empty for no passphrase): [Type a passphrase]
  Enter same passphrase again: [Type passphrase again]
  ```
  
  * 3B Once the key has been generated, go to your /home folder open the dropdown menu, and select "show hidden files". Open up .ssh and copy the contents from id_rsa to [the settings page on github](https://github.com/settings/keys)

* Step 4 Install git through Bash

  * 4A To do this, you will need sudo/root privileges. In order to protect your system, Tails is programmed to boot without a root password enabled. If you did not configure one upon startup, you will need to restart and do so now. (Click the + on the boot dialog screen, configure admin password)
  * 4B Type the following into bash 
  ```
  sudo apt install git
  ```
  This installs git.
  
* Step 5 Generate config and known_hosts files in your ~/.ssh/ file
```
cd ~/.ssh/
touch config
touch known_hosts
```

* Step 6 Edit your config file to add these two lines
```
Host github.com
    User git
```
There should be 4 spaces before "User git"

* Step 7 Attempt a clone from your github repository
  * 7A CD to where you would like to save your git repository
  ```
  cd ~/Persistent
  ```
  * 7B Enter the command
  ```
  sudo git clone ssh://git@github.com:/YourUsername/YourRepository.git
  ```
  You should get a dialog asking you to add Github to the known hosts file. 
  * 7B (In case you don't get asked to add github to known hosts)
  Try this command instead
  ```
    GIT_SSH_COMMAND="ssh -o StrictHostKeyChecking=no -o MACs=\"hmac-sha2-512-etm@openssh.com,hmac-sha2-512\" -o KexAlgorithms=\"curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256\" -o IdentitiesOnly=yes -o IdentityFile=~/path/to/your/id_rsa" git clone ssh://git@github.com/YourUsername/YourRepository.git
  ```
  
* Step 8 Once github has been added to your known_hosts file, edit your config file so it looks something like this

```
#Path to known hosts should be ~/.ssh/known_hosts (By default)
#Path to id_rsa should be ~/.ssh/id_rsa (By default)
Host github.com
    User git
    MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-512
    KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256
    StrictHostKeyChecking yes
    UserKnownHostsFile /path/to/known_hosts 
    IdentitiesOnly yes
    IdentityFile /path/to/id_rsa
```

* Step 9 You can now securely use git commands through secure shell with Tails OS!

```
cd ~/Persistent/Repository
sudo git clone ssh://git@github.com/YourGitUsername/YourRepository.git
```

