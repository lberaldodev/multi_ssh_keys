Setup to MultiSSH keys using macOS

1- Set the permissions
```chmod 700 ~/.ssh``` and then create the key:

```ssh-keygen -t ed25519 -C "your_email@example.com"```.

2- Enter file in which to save the key (/Users/xpto/.ssh/ssh_key_1):

```/Users/xpto/.ssh/ssh_key_1```.

3- Repeat the first and second step and replace the values (ssh_key_name and email).

4- Copy the ssh content and add to git each key:

```pbcopy < ~/.ssh/<SSH_KEY_FILE_NAME>.pub```.

5- Execute ```vi ~/.ssh/config```

6- Replace the content with:
```
  Host <HOST_1>
  HostName <HOST_1>
  AddKeysToAgent yes
  User git
  UseKeychain yes
  IdentitiesOnly yes
  IdentityFile ~/.ssh/<SSH_1_FILE_NAME>

  Host <HOST_2>
  HostName <HOST_2>
  User git
  IdentityFile ~/.ssh/<SSH_2_FILE_NAME>
  IdentitiesOnly yes
  AddKeysToAgent yes
  UseKeychain yes`
```

7- Test using:

```
ssh -T <HOST 1> (check inside the git clone command the host)
```
and
```
ssh -T <HOST 2> (check inside the git clone command the host)
```

8- Add to apple-key-chain:

```ssh-add --apple-use-keychain ~/.ssh/SSH_1_FILE_NAME``` 

and 

```ssh-add --apple-use-keychain ~/.ssh/SSH_2_FILE_NAME```

9- Create git config files to each ssh_key:

```vi ~/.gitconfig-<NAME_OF_FIRST_ENVIRONMENT>```

and

```vi ~/.gitconfig-<NAME_OF_SECOND_ENVIRONMENT>```

then put inside each file the correct content for each environment

```
[user]
  name = XPTO 1
  email = XPTO@XPTO.COM
```

10- Run ```vi ~/.gitconfig``` and replace the content inside to:
```
[includeIf "gitdir:<PATH_TO_YOUR_FOLDER_TO_FIRST_SSH>"]
  path = ~/.gitconfig-<NAME_OF_FIRST_ENVIRONMENT>

[includeIf "gitdir:<PATH_TO_YOUR_FOLDER_TO_FIRST_SSH>"]
  path = ~/.gitconfig-NAME_OF_SECOND_ENVIRONMENT
```

the path is something like:

```/Users/XPTO_USER/Desktop/personal/``` 

and 

```/Users/XPTO_USER/Desktop/enterprise/``` 

11- Open a git project inside each folder and check if the config is set using ```git config --show-origin user.name```. It should be as defined inside .gitconfig-<file_name>