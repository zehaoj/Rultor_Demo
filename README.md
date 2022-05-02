# Rultor Demo

## How Rultor works?
Rultor does the following steps:
1. Reads the `.rultor.yml` YAML configuration file from the root directory of your repository.
2. Gets automated build execution command from it, for example bundle test.
3. Checks out your repository into a temporary directory on one of its servers.
4. Merges pull request into master branch.
5. Starts a new Docker container and runs the build execution command in it, for example bundle test.
6. If everything is fine, pushes modified master branch to GitHub.
7. Reports back to you, in the GitHub pull request.

## How to make Rultor work on your repo?
1. Add Rultor as your Collaborator.
In the `Settings` of your repo, under `Collaborator`, add Rultor to give it write access to your repo.

<p align="center">
    <img width="250" alt="Screen Shot 2022-05-02 at 12 49 53" src="https://user-images.githubusercontent.com/42339734/166223506-7dac42e0-4ad0-4495-81b0-d3c1dacea9b2.png">
</p>

2. Add a `.rultor.yml` file to the root folder of the repo. Detailed tutorial on how to write it is in the next section.
3. Use command containing keyword like 'merge', 'deploy' or 'release' in the PR to trigger Rultor.

<p align="center">
    <img width="500" alt="Screen Shot 2022-05-02 at 12 59 04" src="https://user-images.githubusercontent.com/42339734/166223791-ecf68b1c-eea0-4718-94c4-46cf4eef34cf.png">
</p>

## How to write `.rultor` file?
### Architect
You may wish to define a role of an architect in your project, who will supervise all merge/release/deploy commands. No command of that kind will be executed without his confirmation:
```
architect:
  - zehaoj
  - ......
```
This is enough to tell Rultor to ask for confirmation before running a build.

### Merge, Deploy, Release
Three commands `merge`, `deploy` and `release` are configured similarly in `.rultor.yml`.

The list of Github accounts able to give commands to Rultor is specified in commanders. By default, only Github repository collaborators can give commands. Configured commanders don't replace collaborators. In other words, Github collaborators and accounts mentioned here are allowed to give commands.

```
merge: # or "deploy" or "release"
  commanders:
    - jeff
    - walter
  env:
    MAVEN_OPTS: "-XX:MaxPermSize=256m -Xmx512m"
  script:
    - "sudo apt-get install graphviz"
    - "mvn clean install"
```
