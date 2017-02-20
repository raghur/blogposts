<!--
PostId: 2061811919130731355
Title    : Kubernetes cluster visualizer
Labels   : kubernetes, visualization, tips
Format	 : markdown
Published: true
-->

[GCP live k8s visualizer](https://github.com/brendandburns/gcp-live-k8s-visualizer) is a cool project to visualize
your k8s cluster. As pods, services & other resources come in and go out, it shows a visual map of your cluster

![gcp live k8s visualizer](https://i.imgur.com/fcrtlwI.gif){width=100%}

The animation above is from [this guide](http://kubecloud.io/guide-setting-up-visualizer-for-kubernetes/).
Followed the instructions there in but ran into javascript null errors ![error](https://i.imgur.com/SyAhXFG.png){width=100%}.

Anyway, took a look this morning and while it was a simple fix, I'm not sure why the kube api is returning null for something
that's a collection?

Anyway, here's the [fork with the fix applied](https://github.com/raghur/gcp-live-k8s-visualizer) which should let you run the visualizer against your cluster.

I also made it show the Deployment (since ReplicationControllers are apparently superseded by ReplicaSets). While the job
of the ReplicaSet is the same as the earlier RC, the preferred method to scale is via the Deployment.
The visualization's a little weird - but basically I have a Deployment showing up where earlier a ReplicationController would
have been.
![here goes](https://i.imgur.com/RZhUhU4.png){width=100%}

