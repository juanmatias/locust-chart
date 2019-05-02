# Locust Helm Chart

This is a templated deployment of [Locust](http://locust.io) for Distributed Load
testing using Kubernetes. (based on so0k's work))

## Notes:

This is a kind of fork of the official chart. I didn't forked it because the project there includes all the charts, not only this one... and people at the project didn't answered my questions about create a PR.... so here it is, use at your own risk ;)

## Why this new chart?

The original chart is stucked into sample tasks. To add new tasks you need to rebuild the image. This is not convenient thinking in the K8s way of doing things.

Here you can set the locust_tasks file into the values file, so to modifiy tasks you just modify the `values.yaml` file and upgrade the deploy.

## Original doc

I added here the original [README.md](./REAMDE_original.md) just in case.

## Installing the chart

You can follow the original installing steps. Beside them you have a simple modification here: in the `values.yaml` file you have a new key: `configMap.tasksfiles.task001.py`... under this key you set your tasks file contents.

Sample:

```
configMap:
  tasksfiles:
    task001.py: |
      import json
      from locust import HttpLocust, TaskSet, task

      class Grasshoppers(TaskSet):
            def on_start(self):
              self.client.verify = False

            @task
            def get_api_Amenities_filters(self):
              self.client.get("/api/Amenities/filters", name="/api/Amenities/filters")

      class AwesomeUser(HttpLocust):
         task_set = Grasshoppers
         min_wait = 1 * 1000
         max_wait = 5 * 1000
```

That's it... enjoy it!
