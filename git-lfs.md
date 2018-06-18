# How to use Git LFS

## Date

2018-03-23

## Purpose

Document how to work with Git LFS, using local test servers.

## Convention

All directories are assumed to be relative to some parent directory, and commands are assumed to be issued in the parent directory unless specified otherwise.

## Setting up a Git server

- Create a directory, e.g., `lfstest.git`.
- Inside `lfstest.git`, run `git init --bare`.

## Setting up a Git LFS server

- Install the [Git LFS test server software](https://github.com/git-lfs/lfs-test-server) in a location on your `PATH`.
- Create a directory `lfsfiles`.
- Set environment variables:
  - `set LFS_HOST=localhost:8080` (or the port of your choice)
  - `set LFS_LISTEN=tcp://:8080` (use same port as above)
  - `set LFS_ADMINUSER=<admin username>` (for the maintenance interface)
  - `set LFS_ADMINPASS=<admin password>`
  - `set LFS_CONTENTPATH=lfsfiles` (rather use absolute path)
- Start the server by running `lfs-test-server`.
- Access the web interface at `localhost:8080/mgmt`, using the admin credentials.
- Create a new user for your test LFS instance.

## Setting up a Git repo with LFS

- Create a new directory `lfstest`.
- In that directory,
  - initialize a repo by running `git init`,
  - initialize LFS by running `git lfs install`,
  - point LFS to the test server by running `git config lfs.url http://localhost:8080/`,
  - add your Git server as a remote by running `git remote add origin ../lfstest.git`,
  - create some large file `largefile.bin`,
  - tell Git LFS to track that file by invoking `git lfs track largefile.bin`,
  - stage and commit big file and the new `.gitattributes` file using `git add .` and `git commit -m "My commit message"`,
  - push the commit,
  - enter your user credentials at the LFS server.
- The meta-info will be pushed to the Git repo, while the large file is stored in the `lfsfiles` directory by the server.
- Changes to the big file and other files explicitly tracked (also works with wildcards) will be reflected in the `lfsfiles` storage, while the actual Git repo in `lfstest.git` remains small.

## Cloning a project using Git LFS

- Running `git clone` on a project using Git LFS will fail partly and leave the repo in a dirty state that needs to be fixed, unless the `lfs.url` Git configuration is set globally.
- To avoid this trouble, you can skip the download of the LFS managed files by defining the `GIT_LFS_SKIP_SMUDGE` environment variable:
  - clone the repository by running `GIT_LFS_SKIP_SMUDGE=1 git clone lfstest.git lfstest2` (assuming a BASH terminal),
  - in the `lfstest2` directory, run `git config lfs.url http://localhost:8080/`,
  - pull the big files by invoking `git lfs pull`.
