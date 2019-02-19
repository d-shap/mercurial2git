# Mercurial to git synchronization utility
Docker image for mercurial to git synchronization utility.

The purpose of this image is to push commits from remote mercurial repository to remote git repository.

Suppose organization has internal mercurial repository and all employees push commits to this repository.
And this organization decides to make some part of this repository public - to publish some code to, for example, GitHub.
To avoid two repository maintenance, and to prevent mistakes like pushing to the wrong repository mercurial2git synchronization could be used.

In this case all employees still push their commits to internal mercurial repository.
And synchronization job synchronizes mercurial repository with git repository. 

This docker container automates synchronization routine.

**Note.**
This is one-way synchronization.
Git repository is a view of mercurial repository.
No changes should be made in git repository.
The only one way something can be placed in git repositry - is through mercurial repository.

Only specific branches are synchronized.
For example, mercurial default branch is synchronized with git master branch.
Or only release branches are synchronized, without any feature branches.

## Installation
### Installation from docker image
Pull docker image.

Proceed to configuration.

### Installation from source
Pull project sources from version control system.

Make **build** executable:
``` 
sudo chmod u+x ./build
```

Execute **build**:
```
sudo ./build
```

Proceed to configuration.

### Configuration
```
sudo docker run \
    --rm \
    -e MERCURIAL_REPO_URL='http://localhost/mercurial/repository' \
    -e GIT_REPO_URL='https://user:password@github.com/user/repository.git' \
    -e MERCURIAL_BRANCH_NAME_1='default' \
    -e GIT_BRANCH_NAME_1='master' \
        mercurial2git
```
| Parameter               | Value                              |
| :---------------------- | :--------------------------------- |
| MERCURIAL_REPO_URL      | Mercurial repository URL           |
| GIT_REPO_URL            | Git repository URL                 |
| MERCURIAL_BRANCH_NAME_1 | First mercurial repository branch  |
| GIT_BRANCH_NAME_1       | First git repository branch        |
| MERCURIAL_BRANCH_NAME_2 | Second mercurial repository branch |
| GIT_BRANCH_NAME_2       | Second git repository branch       |
| ...                     | ...                                |
| MERCURIAL_BRANCH_NAME_N | Last mercurial repository branch   |
| GIT_BRANCH_NAME_N       | Last git repository branch         |

Git branch name defaults to mercurial branch name and can be omitted.
```
sudo docker run \
    --rm \
    -e MERCURIAL_REPO_URL='http://localhost/mercurial/repository' \
    -e GIT_REPO_URL='https://user:password@github.com/user/repository.git' \
    -e MERCURIAL_BRANCH_NAME_1='default' \
    -e GIT_BRANCH_NAME_1='master' \
    -e MERCURIAL_BRANCH_NAME_2='featureX' \
        mercurial2git
```
In this example the `default` mercurial branch is synchronized with the `master` git branch, and the `featureX` mercurial branch is synchronized with the `featureX` git branch.

If e-mail is used as a git user name then **GIT_REPO_URL** parameter should look like this:
```
sudo docker run ... -e GIT_REPO_URL='https://user%40example.com:password@github.com/user/repository.git' ...
```

It is possible not to specify user password in the command.
In this case `-it` argument shold be added to the run command.
```
sudo docker run ... -it ...
```

During the synchronization user will be prompted to specify the password.

## Cron job
Copy **./usr/bin/m2g** to **/usr/bin** folder:
```
sudo cp ./usr/bin/m2g /usr/bin
```

Make **m2g** executable:
```
sudo chmod a+x /usr/bin/m2g
```

Edit **/usr/bin/m2g** and specify synchronization repository URLs and branches.

Create cron job:
```
sudo crontab -l | { cat; echo "0 1 * * * /usr/bin/m2g <repository>"; echo ""; } | sudo crontab -
```

# Donation
If you find my code useful, you can [bye me a coffee](https://www.paypal.me/dshapovalov)
