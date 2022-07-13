### Some commands that can make your life easier

#### Install package from a list:
```script
sudo apt install $(cat tools.list | tr "\n" " ") -y
```


#### Serious bash prompt for penetration test:

```bash
export PS1="-[\[$(tput sgr0)\]\[\033[38;5;10m\]\d\[$(tput sgr0)\]-\[$(tput sgr0)\]\[\033[38;5;10m\]\t\[$(tput sgr0)\]]-[\[$(tput sgr0)\]\[\033[38;5;214m\]\u\[$(tput sgr0)\]@\[$(tput sgr0)\]\[\033[38;5;196m\]\h\[$(tput sgr0)\]]-\n-[\[$(tput sgr0)\]\[\033[38;5;33m\]\w\[$(tput sgr0)\]]\\$ \[$(tput sgr0)\]"
```

![](Pasted%20image%2020220712113954.png)

Another version that will apply permanently:

```bash
#!/bin/bash

#### Make a backup of the .bashrc file
cp ~/.bashrc ~/.bashrc.bak

#### Customize bash prompt
echo 'export PS1="-[\[$(tput sgr0)\]\[\033[38;5;10m\]\d\[$(tput sgr0)\]-\[$(tput sgr0)\]\[\033[38;5;10m\]\t\[$(tput sgr0)\]]-[\[$(tput sgr0)\]\[\033[38;5;214m\]\u\[$(tput sgr0)\]@\[$(tput sgr0)\]\[\033[38;5;196m\]\h\[$(tput sgr0)\]]-\n-[\[$(tput sgr0)\]\[\033[38;5;33m\]\w\[$(tput sgr0)\]]\\$ \[$(tput sgr0)\]"' >> ~/.bashrc
```

#### Perform commands from multiple shell script:
Let's say you deploy a server contains multiple usefull script that you can deploy on other machines
You can use a text file which contains the name of those script files:
```bash
cat customization-scripts.txt

Prompt.sh
Tools.sh
GUI.sh
Tmux.sh
Vim.sh
```

Then you can use this command to download all & apply all of them into the selected machine:

```bash
for script in $(cat customization-scripts.txt); do curl -s http://myvps.vps-provider.net/$script | bash; done
```

#### Have fun with "find" command:
example:
```bash
find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null
```

`-type f` : Hereby, we define the type of the searched object. In this case, '`f`' stands for '`file`'.

`-name *.conf` : With '`-name`', we indicate the name of the file we are looking for. The asterisk (`*`) stands for 'all' files with the '`.conf`' extension.

`-user root` : This option filters all files whose owner is the root user.

`-size +20k` : We can then filter all the located files and specify that we only want to see the files that are larger than 20 KiB.

`-newermt 2020-03-03`  : With this option, we set the date. Only files newer than the specified date will be presented.

`-exec ls -al {} \;` : This option executes the specified command, using the curly brackets as placeholders for each result. The backslash escapes the next character from being interpreted by the shell because otherwise, the semicolon would terminate the command and not reach the redirection.

`2>/dev/null` : This is a `STDERR` redirection to the '`null device`', which we will come back to in the next section. This redirection ensures that no errors are displayed in the terminal. This redirection must `not` be an option of the 'find' command.