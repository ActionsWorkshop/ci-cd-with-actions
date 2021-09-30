# Satellite Workshop Instructions: CI/CD with GitHub Actions

![image](https://user-images.githubusercontent.com/25735209/112306850-45f48c80-8cc6-11eb-903a-f38713b0a062.png)

:bookmark: The following instructions will take on the assumption that you will be completing the steps on the GitHub UI. Feel free to use the terminal or any other GUIs you are comfortable with.

# Workflow 1: Set up CI workflow

**Workflow 1** will focus on creating a CI workflow for our [react app](react-app.md). 
At the end of this exercise we will learn -
1. How to create a new CI workflow 
2. About Workflow templates 
3. Configuring Action triggers and type of triggers 
4. About Job strategies 
5. Adding a status badge for a workflow

## Fork ci-cd-with-actions repository

1. Log into your GitHub account and fork [ci-cd-with-actions](https://github.com/ActionsWorkshop/ci-cd-with-actions). 

    - Open the [ci-cd-with-actions](https://github.com/githubsatelliteworkshops/ci-cd-with-actions) in a browser
    - Sign in to your GitHub account from top right if not signed-in already.
    - Fork this repository into your user account (using `Fork` option on top right of the screen). This repository will get forked as `<username>/ci-cd-with-actions`
    
    This repo contains a starter project for react.js for which we will be automating CI and CD flow. It also contains the instructions for the workshop.
  

## Add a CI Workflow

1. In the forked `<username>/ci-cd-with-actions` repository, go to **Actions** tab and you will see a page to "Get started with GitHub Actions". You will find suggested starter workflows for this repository here.

2. Choose **Node.js** -> "Setup this Workflow"
  <img width="575" alt="Screenshot 2021-09-27 at 2 50 50 PM" src="https://user-images.githubusercontent.com/86035/134881207-5d10db2f-3376-4d4f-a238-1dd0db2c22a1.png">
  
3. You can choose to name the file as "ci.yml". The file that you are creating on `main` branch is - 
    - `.github/workflows/ci.yml`
      - This will be the workflow file taking care of building and testing your source code
    - Observe the components of the workflow file
        - `name:` Name of the workflow
        - `on:` Trigger for the workflow
        - `jobs` Only one job is there -> `build`
        `build` job has 
        - `runs-on` 
        - `strategy matrix`
        - `steps` to checkout the repo, setup Node, install denedencies, build and test.
     
4. Use `Start Commit` button to commit this file as such directly to the `main` branch.
<img width="1440" alt="Screenshot 2021-09-27 at 2 58 32 PM" src="https://user-images.githubusercontent.com/86035/134882550-4afb61a8-3166-4b29-b904-6718f3387622.png">

5. :tada: Go to Actions tab to see the first workflow running
    - Under `All workflows`, you will find your new workflow `Node.js CI.
    - Select the workflow to see the list of runs. 
    - Open the top most run that got triggered and you can click on the `Matrix: build` to see the jobs running or completed.
    <img width="358" alt="image" src="https://user-images.githubusercontent.com/58769601/134165721-2cc1cdb7-e11f-489b-a001-f0daefbc9f63.png">

## Make changes to the CI workflow to customize for our scenario.

1. Go to `.github/workflows/ci.yml` and enter edit mode by clicking the pencil :pencil: icon
   - To make the workflow name specific to our app, change the name of the workflow to "React App CI".
   - For our app, we want to trigger the CI on push and pull_request events not only for main branch but also all the branches starting with `releases\`. Let's specify these in `on:`
   - Rename the `build` job id to `build-test` as it is does both build and test
   - Provide a name also for this job as "Build and Test". When you start typing "name", use Ctrl+space in the yaml editor for Intellisense. 
           
   💡 When you are editing your workflow, on the right hand side you will find `Marketplace` and `Documentation` tabs. You can check the documentation tab for information on how to make these changes in the workflow 
  
2. Click on "Start Commit" and commit your changes to the workflow.
 
   <details>
        <summary><b>Click here to view the contents of the yaml file to copy:</b></summary>

    ```yaml
    name: React App CI

    on:
      push:
        branches: 
        - main 
        - releases/*
      pull_request:
        branches: 
        - main 
        - releases/*

    jobs:
      build-test:
        name: Build and test

        runs-on: ubuntu-latest

        strategy:
          matrix:
            node-version: [10.x, 12.x, 14.x, 15.x]
            # See supported Node.js release schedule at https://nodejs.org/en/about/releases/

        steps:
        - uses: actions/checkout@v2
        - name: Use Node.js ${{ matrix.node-version }}
          uses: actions/setup-node@v1
          with:
            node-version: ${{ matrix.node-version }}
        - run: npm ci
        - run: npm run build --if-present
        - run: npm test
        
    ```
   </details>
   - :warning: `yaml` syntax relies on indentation, please make sure that this is not changed

## Create a Pull request to add a status badge to ReadMe
1. In the `Actions` tab, click on the `React App CI` workflow and you will find a `Create status badge` option.  ![image](https://user-images.githubusercontent.com/58769601/134166769-8a306f70-a037-44cf-96fb-550719ae980b.png)
2. Click that and `Copy the status badge markdown`
3. Edit the ReadMe file and paste this markdown on the top of the file. 
4. Commit the readMe file by choosing to create a new branch and start a pull request. Press `Propose Change` and Open a pull request against `main` as the base branch.
5. Go to the Pull request and :tada: and wait for the CI being run as `Checks` in PR conversation tab.
![image](https://user-images.githubusercontent.com/58769601/134167827-963cb4ca-0714-46f3-b43e-72841d085587.png)
6. After all the checks have passed, merge the PR, :tada: the CI status badge will be there on the repository's home page.
![image](https://user-images.githubusercontent.com/25735209/112309132-fbc0da80-8cc8-11eb-9461-d20709478351.png)

### [Click here to view the solution yml for Exercise 1](./solution/ci.yml)

## [Click here to get started with Workflow 2](./workshop_instructions2.md)
