## 🪶 JMeter Grid
> Tekton Pipleine for running JMeter distributed testing on a grid of predermined nodes.

![tekton-pipeline](./tekton-pipeline.jpg)

When running, the pipeline will spin up a grid of N agents for running a disrtibuted load test from jmeter on. A sample test etc is found in this monorepo.folder. This project needs `RWM` storage available.  

### 🏃‍♀️ To run ...

1. Login to OCP & create new project for your testing 
```bash
oc login ...
oc new-project minnie-mouse
```

2. Apply the pipleine folder. It has a depndency on the `git-clone` task provided in cluster and adds two new tasks, a workspace and a Pipeline.
```shell
pipeline
├── pipeline
│   └── pipeline.yaml
├── tasks
│   ├── helm-deploy.yaml
│   ├── helm-remove.yaml
│   └── run-jmeter.yaml
└── workspaces
    └── pvc.yaml

oc apply -f pipeline -R
```

3.  Run the pipeline in hte UI or on the terminal. All sensible defaults will spin up a three node grid of agents, run the example test and tear things down after execution
```
tkn pipeline start jmeter-test -w name=source,claimName=jmeter-runs 
```

### 👩‍💻 development
* the chart folder contains the "grid". it uses a shared `RWM` disc to allow data and test cases be copeid to one location for execution. This is packaged as a helm chart
* the `build` folder contains a "borrowed" Containerfile I used to build the image. Credit at top of the file for where it was borrowed...

### What's next?
* add plugins and test data to the image and test further examples
* params on the script 
* triggers on the build
