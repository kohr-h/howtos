# How to Use Git LFS

## Convention

All directories are assumed to be relative to some parent directory, and commands are assumed to be issued in the parent directory unless specified otherwise.

## Setting up a Git server

1.  Create a directory, e.g., `lfstest.git`.
2.  Inside `lfstest.git`, run `git init --bare`.

## Setting up a Git LFS server

1.  Install the [Git LFS test server software](https://github.com/git-lfs/lfs-test-server) in a location on your `PATH`.
2.  Create a directory `lfsfiles`.
3.  Set some environment variables:
    
    |                   |                    |                                                       |
    | ----------------- | ------------------ | ----------------------------------------------------- |
    | Name              | Value              | Comments                                              |
    | `LFS_HOST`        | `localhost:8080`   | Or some other free and accessible port                |
    | `LFS_LISTEN`      | `tcp://:8080`      | Same port as above                                    |
    | `LFS_ADMINUSER`   | \<admin username\> | Root user for server management                       |
    | `LFS_ADMINPASS`   | \<admin password\> |                                                       |
    | `LFS_CONTENTPATH` | `lfsfiles`         | Where files are stored. Better use the absolute path. |
    

4.  Start the server by running `lfs-test-server`.
5.  Access the web interface at `localhost:8080/mgmt`, using the admin credentials.
6.  Create a new user for your test LFS instance.

## Setting up a Git repo with LFS

1.  Create a new directory, e.g., `lfstest`, and `cd` into it.
2.  Initialize a repo by running `git init`.
3.  Initialize LFS by running `git lfs install`.
4.  Point LFS to the test server by running `git config lfs.url http://localhost:8080/`.
5.  Add your local Git server as a remote by running `git remote add origin ../lfstest.git`.

## Testing the setup

1.  Create some large file `largefile.bin`.
2.  Tell Git LFS to track that file by invoking `git lfs track largefile.bin`.
3.  Stage and commit the big file and the new `.gitattributes` with `git add .` and `git commit -m "My commit message"`.
4.  Push the commit.
5.  Enter your user credentials for the LFS server.
6.  The meta-info will be pushed to the Git repo, while the large file is stored in the `lfsfiles` directory by the server.
7.  Changes to the big file and other files explicitly tracked (also works with wildcards) will be reflected in the `lfsfiles` storage, while the actual Git repo in `lfstest.git` remains small.

## Cloning a project using Git LFS

Running `git clone` on a project using Git LFS will fail partly and leave the repo in a dirty state that needs to be fixed, unless the `lfs.url` Git configuration is set globally.
To avoid this trouble, you can skip the download of the LFS managed files by defining the `GIT_LFS_SKIP_SMUDGE` environment variable:

1.  Clone the repository into a new directory, e.g., `lfstest2`, by running `GIT_LFS_SKIP_SMUDGE=1 git clone lfstest.git lfstest2` (assuming a BASH shell).
2.  In the `lfstest2` directory, run `git config lfs.url http://localhost:8080/`.
3.  Pull the big files by invoking `git lfs pull`.
