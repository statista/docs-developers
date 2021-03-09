# GIT

## Common Problems

* Local Git-Connection doesn't work any longer (did work before)

    ```
    exec ssh-agent bash
    ssh-add ~/.ssh/bitbucket
    ssh -T git@bitbucket.org
    ```