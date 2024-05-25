# Git Configuration

## SSH Setup

### Generate key

> [!Warning]
> **Never** share your private key. This is the key to your castle.

Generate a key in GitBash by running the following with your email address.

```bash
ssh-keygen -t rsa -C "email@domain.com"
```

> [!Note]
> When saving, check that the file and location are ->
> `~/.ssh/id_rsa`
>
> Public `id_rsa.pub`, and private `id_rsa`, keys will be stored in your `~/.ssh` directory.

#### Passphrase

You'll be prompted to enter a passphrase to protect your key.

### Adding Key

#### Register Key

Make sure the SSH agent is running ->

```bash
eval $(ssh-agent -s)
```

Then add the key ->

```bash
ssh-add ~/.ssh/id_rsa
```

#### GitHub

Add your **public** key to your GitHub profile by copying the output from ->

```bash
cat ~/.ssh/id_rsa.pub
```

Now add this to [settings here](https://github.com/settings/keys)...

## Issues

In the unlikely event you run in to problems, some details are listed here.

### "Unable to negotiate ... no matching host key type found"

If you receive a message like the above, then in the `.ssh` directory, create a file called `config` with the following contents:

```text
Host host.address.goes.here.com
    User git
    IdentityFile ~/.ssh/id_rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
    HostKeyAlgorithms +ssh-rsa
```

If the above doesn't work, check your OpenSSH version.

```ssh -V```

If the result is *earlier* than `6.8`, update your installation. If the result is *earlier* than `8.5`, then use the following config instead.

```text
Host host.address.goes.here.com
    User git
    IdentityFile ~/.ssh/id_rsa
    PubkeyAcceptedKeyTypes +ssh-rsa
    HostKeyAlgorithms +ssh-rsa
```

>[!NOTE]
>`6.8` introduced `PubkeyAcceptedKeyTypes`, which *later* changed to `PubkeyAcceptedAlgorithms` in `8.5`.
>
>Further detail is available in the [OpenSSH cookbook](https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Public_Key_Authentication).