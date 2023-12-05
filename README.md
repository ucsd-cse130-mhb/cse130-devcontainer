# CSE 130 Dev Container

You can find the container at <https://github.com/orgs/cse130-assignments/packages/container/package/cse130-devcontainer>.

See the [wiki](https://github.com/cse130-assignments/cse130-devcontainer/wiki/Updating-the-Devcontainer) for
instructions on how to update this container.

A repository for a VS Code Development Container for implementing 
CSE 130 programming assignments. For some general info on making 
your own devcontainers see here: 
[https://benmatselby.dev/post/vscode-dev-containers/](https://benmatselby.dev/post/vscode-dev-containers/)

## Prerequisites 

Make sure [VS Code](https://code.visualstudio.com/Download) 
and [Docker](https://docs.docker.com/get-docker/) are installed 
and running.

## Quick start: How to use

The main purpose of a devcontainer is to have a simple way of getting a usable
and predictable development environment regardless of the underlying operating
system.  The devcontainer configuration is part of each CSE130 assignment.

1. In VS Code, select `File -> Open Folder` and navigate to the folder containing 
   your cloned CSE130 assignment. Alternatively, you can use VS Code to clone your 
   assignment repo by selecting `Clone Git Repository` in a new VS Code window.
2. Click on 'Yes I trust the authors' at the popup (if it appears).
3. You should get a popup that says: "Folder contains a Dev Container configuration 
   file." Click `Reopen in Container`.
5. Wait while Docker downloads builds the devcontainer.  This could take a while, 
   and might benefit from a good internet connection.
6. Once the container is built, try opening a terminal on your devcontainer by 
   selecting `Terminal -> New Terminal`. The prompt will probably read something 
   like `vscode ➜ ~ $`.

## Troubleshooting

#### Devcontainer fails to start 

Some versions of the VSCode extension *Remote - Containers* have a bug 
that interrupts automatic installation scripts.  Make sure you are using 
the latest version.

#### `ExitFailure (-9) (THIS MAY INDICATE OUT OF MEMORY).`

This probably means that your docker container does not have enough memory 
allocated to build the Haskell dependencies.  GHC is known to be a memory 
hog!  You will probably have better luck by increasing the maximum memory 
Docker is permitted to use by going to Preferences -> Resources and increasing 
the Memory slider to at least 4GB. 

If your machine has <= 8GB in physical memory, you might be able to get by with 
less by repeatedly re-running the failed `stack install ...` command until all the 
packages manage to install.  See 
[this SO post](https://stackoverflow.com/questions/56496852/problem-building-a-docker-container-with-haskell-stack-how-can-i-ensure-that-ha) 
for inspiration.

#### Apple M1 

Since the devcontainer is an x86_64 image, there are some potential issues getting 
it to run on the new Apple M1 CPU.  The best advice at the moment is to try installing 
Rosetta 2 to emulate the x86_64 architecture when running the devcontainer. 
Run this to install Rosetta. 
```softwareupdate --install-rosetta```
Then download and install [Docker for Mac](https://docs.docker.com/desktop/mac/install/).  
Next, open the (potentially hidden) directory `cs114a-devcontainer-<version>/.devcontainer` and edit `Dockerfile`.
Change 
```FROM haskell:${VARIANT}```
to
```FROM --platform=linux/amd64 haskell:${VARIANT}```

This tells Docker to run the image under emulation (via Rosetta).
