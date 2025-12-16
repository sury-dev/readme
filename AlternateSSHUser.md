# SSH Key Setup on Windows (GitHub) — Clean Domain-Safe Workflow

This guide outlines a reliable method for generating and configuring SSH keys on Windows, especially in environments where domain policies restrict permissions inside the user profile directory.

------------------------------------------------------------
1. Create a Dedicated Folder for SSH Keys
------------------------------------------------------------

Command to run:
    mkdir E:\sshkeys

------------------------------------------------------------
2. Generate a New SSH Key for GitHub
------------------------------------------------------------

Command to run:
    ssh-keygen -t ed25519 -C "suryansh.pandey@github" -f "E:\sshkeys\id_ed25519_me"

This produces:
    E:\sshkeys\id_ed25519_me      (private key)
    E:\sshkeys\id_ed25519_me.pub  (public key)

When prompted for a passphrase, press ENTER unless you want one.

------------------------------------------------------------
3. (Optional) Adjust Permissions
------------------------------------------------------------

Commands to run:
    icacls "E:\sshkeys\id_ed25519_me" /inheritance:r
    icacls "E:\sshkeys\id_ed25519_me" /grant:r "%USERDOMAIN%\%USERNAME%:(R)"

------------------------------------------------------------
4. Configure SSH for GitHub
------------------------------------------------------------

Edit the file:
    C:\Users\<your-username>\.ssh\config

Add this block:
    Host github.com-me
        HostName github.com
        User git
        IdentityFile E:/sshkeys/id_ed25519_me
        IdentitiesOnly yes

------------------------------------------------------------
5. Add the Public Key to GitHub
------------------------------------------------------------

Show your public key:
    cat E:\sshkeys\id_ed25519_me.pub

Copy the output, then go to:
    GitHub → Settings → SSH and GPG Keys → New SSH Key
Paste and save.

------------------------------------------------------------
6. Test the SSH Setup
------------------------------------------------------------

Command to run:
    ssh -T git@github.com-me

Expected output:
    Hi <your-github-username>! You've successfully authenticated...

------------------------------------------------------------
7. Use the Key for Git Operations
------------------------------------------------------------

Clone using this key:
    git clone git@github.com-me:<user>/<repo>.git

Or update an existing repo:
    git remote set-url origin git@github.com-me:<user>/<repo>.git

------------------------------------------------------------
End of document.
