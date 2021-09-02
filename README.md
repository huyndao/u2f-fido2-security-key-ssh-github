# How to set up U2F/FIDO2 hardware security key to use with SSH and Github

## Generate ssh key pair
```
$ ssh-keygen -C 'your.email@provider.com' -f $HOME/.ssh/user-github -t ed25519-sk
Generating public/private ed25519-sk key pair.
You may need to touch your authenticator to authorize key generation.
Enter PIN for authenticator: ****************
You may need to touch your authenticator (again) to authorize key generation.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/$USER/.ssh/user-github
Your public key has been saved in /home/$USER/.ssh/user-github.pub
The key fingerprint is:
SHA256: <hash> your.email@provider.com
```

It is probably more convenient to use an empty passphrase.

## Copy public key
```shell
$ xclip -i -sel clip < $HOME/.ssh/user-github.pub
```

## Add public key to GitHub
Go to https://github.com/settings/keys, click on `New SSH key` and paste key and save

## Update $HOME/.ssh/config with the following
```
Host github.com
 Hostname ssh.github.com
 Port 443
 User git
 IdentityFile %d/.ssh/user-github
 IdentitiesOnly yes
```

## Test ssh connection to GitHub
```
$ ssh -T git@github.com
Confirm user presence for key ED25519-SK SHA256: <hash>
Hi user! You've successfully authenticated, but GitHub does not provide shell access.
```

## Set up your repository with the appropriate URL 
```shell
$ git remote set-url origin git@github.com:username/your-repository.git
```

## Now you're ready to add files, commit, push/pull!
```
$ git remote show origin 
Confirm user presence for key ED25519-SK SHA256: <hash>
* remote origin
  Fetch URL: git@github.com:username/your-repository.git
  Push  URL: git@github.com:username/your-repository.git
  HEAD branch: main
  Remote branch:
    main tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local ref configured for 'git push':
    main pushes to main (up to date)
```

## References
* https://www.yubico.com/blog/github-now-supports-ssh-security-keys/
* https://github.blog/2021-05-10-security-keys-supported-ssh-git-operations/
* https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
