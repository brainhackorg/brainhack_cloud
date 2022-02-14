# CONTRIBUTING

- [CONTRIBUTING](#contributing)
    - [Build locally](#build-locally)
        - [Prerequisites](#prerequisites)
            - [node.js](#nodejs)
            - [Hugo](#hugo)
        - [Clone repo and its submodules](#clone-repo-and-its-submodules)
        - [Install javascripts dependencies](#install-javascripts-dependencies)
        - [Serve the website locally](#serve-the-website-locally)

## Build locally

### Prerequisites

#### node.js

A quick version (for Mac and Linux) that will also allow to manage different version of node
is to install node via the "node version manager".

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
nvm install node
```

#### Hugo

- [See the install page](https://gohugo.io/getting-started/installing/)

### Clone repo and its submodules

```
git clone --recurse-submodules
```

If you have only cloned the repo and did not get all the submodules (`docsy` and `docsy`'s own subdmodules),
simply run

```
git submodule update --init --recursive && git submodule update --recursive
```

### Install javascripts dependencies

```
npm install
```

### Serve the website locally
```
hugo server -D
```
