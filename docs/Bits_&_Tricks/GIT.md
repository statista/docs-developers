# GIT

## Common Problems

* Local Git-Connection doesn't work any longer (did work before, especially on Windows Linux Subsystem)

    ```
    exec ssh-agent bash
    ssh-add ~/.ssh/bitbucket
    ssh -T git@bitbucket.org
    ```