# Copyright (c) Tailscale Inc & AUTHORS
# SPDX-License-Identifier: BSD-3-Clause

TS_ROUTES ?= ""
SA_NAME ?= tailscale
TS_KUBE_SECRET ?= tailscale

rbac:
	@sed -e "s;{{TS_KUBE_SECRET}};$(TS_KUBE_SECRET);g" role.yaml | kubectl apply -f -
	@sed -e "s;{{SA_NAME}};$(SA_NAME);g" rolebinding.yaml | kubectl apply -f -
	@sed -e "s;{{SA_NAME}};$(SA_NAME);g" sa.yaml | kubectl apply -f -

sidecar:
	@kubectl delete -f sidecar.yaml --ignore-not-found --grace-period=0
	@sed -e "s;{{TS_KUBE_SECRET}};$(TS_KUBE_SECRET);g" sidecar.yaml | sed -e "s;{{SA_NAME}};$(SA_NAME);g" | kubectl create -f-

userspace-sidecar:
	@kubectl delete -f userspace-sidecar.yaml --ignore-not-found --grace-period=0
	@sed -e "s;{{TS_KUBE_SECRET}};$(TS_KUBE_SECRET);g" userspace-sidecar.yaml | sed -e "s;{{SA_NAME}};$(SA_NAME);g" | kubectl create -f-

proxy:
	kubectl delete -f proxy.yaml --ignore-not-found --grace-period=0
	sed -e "s;{{TS_KUBE_SECRET}};$(TS_KUBE_SECRET);g" proxy.yaml | sed -e "s;{{SA_NAME}};$(SA_NAME);g" | sed -e "s;{{TS_DEST_IP}};$(TS_DEST_IP);g" | kubectl create -f-

subnet-router:
	@kubectl delete -f subnet.yaml --ignore-not-found --grace-period=0
	@sed -e "s;{{TS_KUBE_SECRET}};$(TS_KUBE_SECRET);g" subnet.yaml | sed -e "s;{{SA_NAME}};$(SA_NAME);g" | sed -e "s;{{TS_ROUTES}};$(TS_ROUTES);g" | kubectl create -f-
