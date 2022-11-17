# Day 9

## You might be interested in my blog
<pre>
https://medium.com/tektutor/openshift-ci-cd-with-tekton-faa88ba45656
</pre>


## Lab - Creating a Tekton pipeline with Tasks executed in sequence and parallel


Create a pipeline.yml with below content
```
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: second-pipeline
spec:
  tasks:
    - name: pipeline-task-1
      taskRef:
        kind: Task
        name: task-1
        
    - name: pipeline-task-2
      runAfter:
        - pipeline-task-1
      taskRef:
        kind: Task
        name: task-2
        
    - name: pipeline-task-3
      runAfter:
        - pipeline-task-1
      taskRef:
        kind: Task
        name: task-3
        
    - name: pipeline-task-4
      runAfter:
        - pipeline-task-2
        - pipeline-task-3
      taskRef:
        kind: Task
        name: task-4
        
    - name: pipeline-task-5
      runAfter:
        - pipeline-task-4
      taskRef:
        kind: Task
        name: task-5
        
    - name: pipeline-task-6
      runAfter:
        - pipeline-task-5
      taskRef:
        kind: Task
        name: task-6
```

You can create the pipeline in our Openshift cluster
```
oc apply -f pipeline.yml
```

You may now run the pipeline via pipelinerun
```
tkn pipeline start second-pipeline --showlog
```
