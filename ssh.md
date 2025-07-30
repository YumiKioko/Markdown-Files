# SSH (Secure Shell)

## Description
SSH is a protocol for secure remote login and command execution over an unsecured network.

## Basic Syntax
```bash
ssh [user@]hostname [command]
```

## Examples

### Connect to a remote server
```bash
ssh user@192.168.1.10
```

### Specify a different port
```bash
ssh -p 2222 user@192.168.1.10
```

### Use an identity file (private key)
```bash
ssh -i ~/.ssh/id_rsa user@hostname
```

### Run a remote command
```bash
ssh user@hostname 'ls -la /var/www'
```

### Enable verbose output for debugging
```bash
ssh -v user@hostname
```

## Tips
- SSH keys can be generated with `ssh-keygen`
- Copy your public key to a server using `ssh-copy-id user@hostname`
- Use `~/.ssh/config` to simplify host configurations

## Security Notes
- Avoid using password-based login when possible.
- Disable root login and use firewall rules to limit SSH access.