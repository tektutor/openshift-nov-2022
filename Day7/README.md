# Day 7

## TekTon Overview
- TekTon a CI/CD Framework that works within Kubernetes/OpenShift
- in other words, it's a knative Frameworks that supports CI/CD
- We can install TekTon CI/CD Framework as an Operator or using manifest files(YAML files)
- it is an opensource project but backed by Red Hat
- it is nicely integrated in Kubernetes and OpenShift

## TekTon Operator
- will install many new Custom Resources like Task, TaskRun, Pipeline, PipelineRun, Workspace, etc.,

## What is a Task?
- Task is a series a steps executed one after the other in a top to bottom order
- Each Task is a Pod
- Task has one or more Steps
- Each Step is a Container that runs within the Task Pod

## What is a TaskRun?
- Tasks are executed via TaskRun
- TaskRun provides inputs required for Task to execute
- in other words, TaskRun is running instance of Task

## What is a Pipeline?
- Tekton Pipeline is a series of Tasks executed in a particular orders
- Tasks can be executed in sequence or in paralled based on your need

## What is a PipelineRun?
- PipelineRun executes a Pipeline providing necessary inputs
- PipelineRun is a running instance of a Pipeline

## What are Workspaces?
- Workspaces are basically Volumes(directory)
- Workspaces are used by a Task to retrieve inputs and store outputs
- Workspaces can be mounted from a ConfigMap, Secret, Persistent Volume Claim or an emptyDir volume
