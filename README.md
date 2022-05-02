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

## How to write `.rultor.yml` file?

Rultor is configured solely through the YAML .rultor.yml file. You need to write everything you want Rultor to do in this file. 
This file is mandatory for Rultor to run, but all contents are optional.
Below we explained some YAML instructions we used for this project. For the complete instruction reference, you can see [here](https://doc.rultor.com/reference.html).
### Architect
This instruction define a role of an architect in your project, who will supervise all merge/release/deploy commands. No command of that kind will be executed without his confirmation:
```
architect:
  - zehaoj
  - ......
```

### Merge, Deploy, Release
These three commands `merge`, `deploy` and `release` are configured similarly in `.rultor.yml`. They define configurations and operations to do during merge, deploy and release.
Below is an example of such instructions.

The list of Github accounts able to give commands to Rultor is specified in commanders. 
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

`commanders` contains a list of Github accounts who are able to give that command. By default, only Github repository collaborators can give commands. Configured commanders don't replace collaborators. In other words, Github collaborators and accounts mentioned here are allowed to give commands.

`env` specifies environment variables that have to be configured, as an associative array with names of variables as keys.

`script` contains executable scripts that you want to run in the container in this task. They will be executed one by one. If any of them fails, execution stops and this task will fail.