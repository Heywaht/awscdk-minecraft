# 📣 Welcome to the December 2022 Minecraft Hackathon

The goal of this project is to make a single pip-installable package that allows anyone
to provision an entire, *cheap*, Minecraft-server-hosting Platform as a Service to their
personal AWS account.

Contributors can expect to come away with an enviable amount of real-world cloud architecture
experience :D

```bash
pip install awscdk-minecraft
```

The project has 4 components.

1. `awscdk-minecraft/` the Python package. Uses AWS CDK to create the infrastructure.
   The other folders in this repository are artifacts that this package includes (and deploys!).
2. `minecraft-platform-frontend/` a React/TypeScript website where users can start/stop a Minecraft server.
   The `awscdk-minecraft` package deploys this as a static website in an S3 bucket.
3. `minecraft-platform-backend-api/` a REST API written with the FastAPI framework that facilitates
   the functionality exposed in the frontend. The `awscdk-minecraft` package deploys this API
   as in a serverless way using AWS Lambda, API Gateway, and Cognito.
4. `awscdk-minecraft-server-deployer/` another AWS CDK package. This package is specifically responsible
   for creating and destroying an EC2 instance with the Minecraft server application running on it.
   This package itself is built into a Docker image. The `awscdk-minecraft` package
   builds the Docker image with this package inside. Then, when users ask the `minecraft-platform-backend-api`
   to create or destroy a Minecraft server. The REST API runs an instance of this Docker image
   which actually carries out the request.

## Contributing

The setup has only been tested for Linux and MacOS, not Windows :(.
If you're on windows, you'll have the best contributor experience using the Windows Subsystem for Linux 2 (WSL2).

## Useful links

> 💡 Click the images to go to each collaboration tool.

> 💡 Bookmark this repository so you can get quick access to these links.

| | |
|:-------------------------:|:-------------------------:|
|<a href="https://app.gather.town/invite?token=f8SJlx7bS9KO6cOWvqIW" target="_blank"><img style="float: left; width:  300px; height: 100%; background-size: cover;" src="./docs/gather-town.png"></a>  <br/>Our virtual park (for remote collab) |  <a href="https://join.slack.com/t/rootskiio/shared_invite/zt-13avx8j84-mocJVx5wFAGNf5wUuy07OA" target="_blank"><img style="float: left; width:  300px; height: 100%; background-size: cover;" src="./docs/slack.png"></a> <br/>Slack, in the `#hackathon` channel |
|<a href="https://www.figma.com/file/LzVP5Ed3i7NQqOkw6YbMVG/Untitled?node-id=0%3A1&t=uW2UsnZVnTNStUjm-1" target="_blank"><img style="float: left; width:  300px; height: 100%; background-size: cover;" src="./docs/roadmap.png"></a> <br/>Roadmap, tasks  |  <a href="https://www.figma.com/file/6y4vDowRkIZPTYOztIxgy7/Minecraft-Architecture?node-id=0%3A1&t=5JKxB5ylSnLLDZ4b-1" target="_blank"><img style="float: left; width:  300px; height: 100%; background-size: cover;" src="./docs/architecture.png"></a> <br/>Architecture|
|<a href="https://d-926768adcc.awsapps.com/start" target="_blank"><img style="float: left; width:  300px; height: 100%; background-size: cover;" src="./docs/aws-console-login.png"></a> <br/>`mlops-club` AWS account login page  |  <a href="https://docs.rootski.io/" target="_blank"><img style="float: left; width:  300px; height: 100%; background-size: cover;" src="./docs/rootski.png"></a> <br/>Similar project with reference code / resources|

### How do I run this project locally?

TL;DR, install `node` and `just`.

```
# install "just"; it's like "make", but less frustrating
brew install just

# install the project's python packages and pre-commit
just install
```

Alternatively, without `brew`:
```
curl --proto '=https' --tlsv1.2 -sSf https://just.systems/install.sh | bash -s -- --to <DEST IN YOUR PATH>
```

where `DEST IN YOUR PATH` refers to a directory that is present in your `$PATH` environment variable. For example, you might have in your `~/.bashrc` the line `PATH=~/bin:$PATH` to look for programs in `~/bin` first, which would be the "DEST" supplied above.

You also need `node` to execute any code related to AWS CDK, which you can install with `brew install nvm` and `nvm install 18`.

### How do I add code?

#### Branching strategy: trunk-based development with feature branches

We use pull requests. Create new branches based on `trunk` for experimentation, then open a PR for it.
You don't have to wait until you want to merge code to open a PR. For this project, the main purpose of doing PRs
is to share knowledge and get early feedback on your ideas.

#### Linting

Passing the `pre-commit` checks isn't a huge deal. They are mostly for your own benefit to prevent you
committing things to the repo that you don't want. You can always override `pre-commit` by running

```bash
# run all of the quality checking tools against your code
just lint
```

```bash
# skip the quality checking tools locally
git commit -m "I really want to commit this large file" --no-verify
```

#### Git configuration

> 📌 Note: you may want to use a different email/username for this repository than
> you typically use on your development machine. You can set your git settings locally
> like so:

```bash
git config --local user.email my-personal-email@gmail.com
git config --local user.user my-github-username
```
#### Notes on commits

DON'T COMMIT...

- credentials. Feel free to put them in a `.env` file, but make sure it's gitignored!
- large files (large CSV, ML model weights, C binaries, video, etc.)
  use git LFS rather than committing it directly.
- unformatted code

The pre-commit hooks setup for this repo when you ran `just install` will remind you
about these each time you commit :)
