# Part 1: Set up development environment and deploy Radix application

## 1A: Set up development environment and prepare application

### 1: Fork repo
* Create fork of https://github.com/oyron/metrics-ws on **Equinor Playground** GitHub organization
* Prefix repo name with user name (e.g. `oyron-metrics-ws`)
* Add a description to the repo
* Deselect "Copy the main branch only".

### 2: Create GitHub Codespace
* Code -> "Create codespace on main"
* This will create a codespace on a 2-core, 4GB RAM, 32GB storage machine, in Europe West GitHub region, based on the settings in `.devcontainer/devcontainer.json`
* Open the codespace in the browser or in your local VS Code

### 3: Prepare your Radix config branch
When we configure the application in Radix, we need to tell Radix from which branch to load the `radixconfig.yaml`
file. This branch is called the _Radix config branch_. The `radixconfig.yaml` will then define from which branch to build the components (`environments - build - from`). As we move through the steps in this workshop, we will access different git branches to load prepared code. To avoid having to update the Radix config branch to build from different branches, we will instead use a designated branch for Radix config and source code. We will update this branch with the content from the different stepwise branches.

* Open the codespace embedded terminal 
* Switch to the "part 1" branch : `git checkout part-1`
* Create a new branch: `git checkout -b radix`
* Push the branch: `git push --set-upstream origin radix`

### 4: Prepare your sample application 
To avoid conflicting names in Radix and Prometheus, we need to set unique names for each application. The application name will be set on the Radix app registration, and will be used in the code to generate metrics with unique names. 
* Application name: `<Equinor user id>-inventory` (e.g. `oyron-inventory`)
* Set the Radix application name in line 4 in `radixconfig.yaml`
* Commit your change, e.g.: `git commit -m "Set app name"
* Do not push your branch yet. We will do this later to trigger the initial build-deploy in Radix.

### 5: Run the sample application locally
* Run `python -m app.server` from the root directory of the repo
* Verify that you are able to open the application web page in the browser

We will explore the sample application in detail later.

### 6: Build and run the Docker image locally
To verify that Radix will be able to build and run our Docker container, we'll do a local build and then run the container
* `docker build -t inventory .`
* `docker run -p 8080:8080 --rm inventory`
* Verify that you are able to open the application web page in the browser

## Part 1B: Deploy your application to Omnia Radix Playground

### 1: Register your application in Omnia Radix Playground
* Open the Radix Playground console: https://console.playground.radix.equinor.com/applications
* Press "Create new app" button from Playground console
* Populate and submit the form 
  * Name: use the same name as you entered in the `radixconfig.yaml` file
  * GitHub repo: the URL to your fork
  * Config Branch: "radix"
  * Owner: Your Equinor email address
  * Administrators: "All Radix users"
  * WBS: "C.ITD.MA.029"

### 2: Add deploy key and webhook
* Add Radix deploy key and webhook as described in the Create new app workflow
* Verify that you have a (more or less empty) console page for your application

### 3: Push to trigger build & deploy
* Push the commit created in step 1A 4 to trigger a build deploy in Radix: `git push`
* Verify that a Radix Pipeline Job is started
* Verify that the pipeline job completes successfully

After the first build deploy job for an application, it might take a few minutes before the newly created DNS entries become available.

Back to slides: https://oyron.github.io/metrics-docs/#/1/4