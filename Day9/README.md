# Day 9

## You might be interested in my blog
<pre>
https://medium.com/tektutor/openshift-ci-cd-with-tekton-faa88ba45656
</pre>


## ⛹️‍♂️ Lab - Creating a Tekton pipeline with Tasks executed in sequence and parallel
```
cd ~/openshift-nov-2022
git pull

cd Day9/pipelines

oc apply -f second-pipeline.yml
oc create -f pipelinerun.yml
tkn pr ls
tkn pr logs -f --last
```

Expected output
<pre>
(jegan@tektutor.org)$ oc apply -f second-pipeline.yml 
task.tekton.dev/task-1 configured
task.tekton.dev/task-2 configured
task.tekton.dev/task-3 configured
task.tekton.dev/task-4 configured
task.tekton.dev/task-5 configured
task.tekton.dev/task-6 configured
pipeline.tekton.dev/second-pipeline unchanged
(jegan@tektutor.org)$ oc create -f pipelinerun.yml 
pipelinerun.tekton.dev/second-pipelinerun-4gd9q created
(jegan@tektutor.org)$ tkn pr logs -f --last
[pipeline-task-1 : step-1] Second Pipeline

[pipeline-task-2 : step-1] Second Pipeline
[pipeline-task-3 : step-1] Second Pipeline
[pipeline-task-3 : step-1] Task 3 - Step 1


[pipeline-task-4 : step-1] Second Pipeline

[pipeline-task-5 : step-1] Second Pipeline

[pipeline-task-6 : step-1] Second Pipeline
</pre>
