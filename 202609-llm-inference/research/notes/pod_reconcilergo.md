---
title: pod_reconciler.go
id: pod_reconcilergo
tags:
- llmd-router-nonk8s-backends-54b774
- locus-topology-and-k8s-shims
- k8s-pod-discovery
- inferencepool
- pod-readiness
- llmd-kv-cache-routing-c665d5
created: '2026-09-11T21:26:35.034517Z'
updated: '2026-09-14T13:10:42.810697Z'
source: https://raw.githubusercontent.com/llm-d/llm-d-router/main/pkg/epp/controller/pod_reconciler.go
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T21:26:35.014210Z'
fetch_provider: crawl4ai
status: evergreen
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'llm-d-router''s default Kubernetes Pod discovery (pkg/epp/controller/pod_reconciler.go)
  is a plain controller-runtime Reconciler, not a separate ''inference-pool-discovery''
  plugin (no such k8s discovery plugin exists in this repo as of Sep 2026 — the only
  pluggable EndpointDiscovery implementation is file-discovery, used to replace Kubernetes
  mode entirely). PodReconciler watches all corev1.Pod objects with an event predicate
  that admits a Pod only if Datastore.PoolLabelsMatch(pod.GetLabels()) is true (InferencePool.spec.selector
  label match). On reconcile, updateDatastore() gates entry into the routing datastore
  on exactly two conditions ANDed together: podutil.IsPodReady(pod) (aggregate corev1.PodReady
  condition == ConditionTrue, and pod.DeletionTimestamp.IsZero()) and PoolLabelsMatch(pod.Labels)
  again. If either fails, the pod is deleted from the datastore; otherwise Datastore.PodUpdateOrAddIfNotExist(ctx,
  pod) is called. There is no check anywhere in this reconcile path on pod.Status.PodIP
  emptiness/validity, pod.Spec.NodeName, or pod.Status.Phase.'
---

```
/*
Copyright 2025 The Kubernetes Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
*/

package controller

import (
	"context"
	"fmt"

	corev1 "k8s.io/api/core/v1"
	apierrors "k8s.io/apimachinery/pkg/api/errors"
	ctrl "sigs.k8s.io/controller-runtime"
	"sigs.k8s.io/controller-runtime/pkg/client"
	"sigs.k8s.io/controller-runtime/pkg/controller"
	"sigs.k8s.io/controller-runtime/pkg/event"
	"sigs.k8s.io/controller-runtime/pkg/log"
	"sigs.k8s.io/controller-runtime/pkg/predicate"

	logutil "github.com/llm-d/llm-d-router/pkg/common/observability/logging"
	"github.com/llm-d/llm-d-router/pkg/epp/datastore"
	podutil "github.com/llm-d/llm-d-router/pkg/epp/util/pod"
)

type PodReconciler struct {
	client.Reader
	Datastore       datastore.Datastore
	RunOnNonLeaders bool
}

func (c *PodReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {
	logger := log.FromContext(ctx)
	if !c.Datastore.PoolHasSynced() {
		logger.V(logutil.TRACE).Info("Skipping reconciling Pod because the InferencePool is not available yet")
		// When the inferencePool is initialized it lists the appropriate pods and populates the datastore, so no need to requeue.
		return ctrl.Result{}, nil
	}

	logger.V(logutil.VERBOSE).Info("Pod being reconciled")

	pod := &corev1.Pod{}
	if err := c.Get(ctx, req.NamespacedName, pod); err != nil {
		if apierrors.IsNotFound(err) {
			c.Datastore.PodDelete(req.Name)
			return ctrl.Result{}, nil
		}
		return ctrl.Result{}, fmt.Errorf("unable to get pod - %w", err)
	}

	if err := c.updateDatastore(ctx, pod); err != nil {
		return ctrl.Result{}, fmt.Errorf("failed to update datastore for pod %s - %w", req.NamespacedName, err)
	}
	return ctrl.Result{}, nil
}

func (c *PodReconciler) SetupWithManager(mgr ctrl.Manager) error {
	filter := predicate.Funcs{
		CreateFunc: func(ce event.CreateEvent) bool {
			pod := ce.Object.(*corev1.Pod)
			return c.Datastore.PoolLabelsMatch(pod.GetLabels())
		},
		UpdateFunc: func(updateEvt event.UpdateEvent) bool {
			oldPod := updateEvt.ObjectOld.(*corev1.Pod)
			newPod := updateEvt.ObjectNew.(*corev1.Pod)
			return c.Datastore.PoolLabelsMatch(oldPod.GetLabels()) || c.Datastore.PoolLabelsMatch(newPod.GetLabels())
		},
		DeleteFunc: func(de event.DeleteEvent) bool {
			pod := de.Object.(*corev1.Pod)
			return c.Datastore.PoolLabelsMatch(pod.GetLabels())
		},
		GenericFunc: func(ge event.GenericEvent) bool {
			pod := ge.Object.(*corev1.Pod)
			return c.Datastore.PoolLabelsMatch(pod.GetLabels())
		},
	}
	needLeaderElection := !c.RunOnNonLeaders
	return ctrl.NewControllerManagedBy(mgr).
		For(&corev1.Pod{}).
		WithEventFilter(filter).
		WithOptions(controller.Options{NeedLeaderElection: &needLeaderElection}).
		Complete(c)
}

func (c *PodReconciler) updateDatastore(ctx context.Context, pod *corev1.Pod) error {
	logger := log.FromContext(ctx)
	if !podutil.IsPodReady(pod) || !c.Datastore.PoolLabelsMatch(pod.Labels) {
		logger.V(logutil.DEBUG).Info("Pod removed or not added")
		c.Datastore.PodDelete(pod.Name)
		return nil
	}
	return c.Datastore.PodUpdateOrAddIfNotExist(ctx, pod)
}

```

