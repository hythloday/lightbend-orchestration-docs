# Overview

Lightbend Orchestration is a developer-centric suite of tools that helps you deploy [Reactive Platform](https://www.lightbend.com/products/reactive-platform) applications to Kubernetes and DC/OS. It provides an easy way to create Docker images for your applications and introduces an automated process for generating Kubernetes and DC/OS resource and configuration files for you from those images. This process helps reduce the friction between development and operations.

## Components

The tooling suite consists of three main parts:

* An sbt plugin, [sbt-reactive-app](https://github.com/lightbend/sbt-reactive-app), which inspects your projects and builds annotated [Docker](https://www.docker.com/) Images.
* A command-line tool, [reactive-cli](https://github.com/lightbend/reactive-cli), which generates configuration for [Kubernetes](https://kubernetes.io/) and [DC/OS](https://dcos.io/).
* An application library, [reactive-lib](https://github.com/lightbend/reactive-lib/), which understands the conventions of the resources generated by the CLI, and can be used by your application to perform service discovery, access secrets, define health and readiness checks, etc. It is automatically included in your application's build by the sbt plugin.

By using these tools, you can leverage the metadata that already exists in your project to easily deploy to Kubernetes without having to manually craft any JSON or YAML files.

## Features

**Akka Cluster Formation**

A service discovery-based approach to Akka Cluster formation that allows you to safely and efficiently form clusters in orchestrated environments. For more information on how this works, be sure to consult the [Akka Cluster Bootstrap](https://developer.lightbend.com/docs/akka-management/current/bootstrap.html) documentation.

**Endpoint Detection & Declaration**

Microservices are detected and HTTP endpoints are automatically declared for you based on the service name. For Kubernetes, this translates to the generation of `Service` and `Ingress` resources. On DC/OS, this means that port specification and [Marathon-lb](https://github.com/mesosphere/marathon-lb) configuration will be generated for you. Additional endpoints can be declared manually as well. Port declaration and configuration for these endpoints is automatically handled when possible but APIs are provided to determine the host and port if your project manually performs the socket binding.

**Docker & JVM Configuration**

The Docker images produced by the tools are configured out of the box to use a lightweight [Alpine Linux](https://alpinelinux.org/) based Docker image. Additionally, if you've declared a memory limit your project will automatically enable the appropriate CGroup JVM options.

**Secrets API**

A simple asynchronous secrets API is provided. Your application simply needs to define the name and key of secrets it is interested in, and then it can use the provided libraries to access the secret values at runtime. This simplifies secret usage as the developer doesn't need to worry about where secrets are; this information is generated for them. Currently, this API is only available for Kubernetes targets.

**Service Location**

A Service Location facility is provided that understands the conventions produced by the tooling and uses them to locate other services. This means you can rely on service discovery to "just work."

**Status**

An application status (health and readiness) facility is provided. By default, applications that use Akka Cluster will not be indicated as "ready" until
they have joined (or formed) a cluster. When combined with [Split Brain Resolver](https://developer.lightbend.com/docs/akka-commercial-addons/current/split-brain-resolver.html),
 this gives you a very reliable mechanism for deploying Akka Cluster-based applications. These facilities are easily extensible by your application so that you can layer additional
health and readiness checks on top of the provided ones.

## Supported Platforms

Currently, the tooling helps you leverage these features on Kubernetes and DC/OS (via Marathon).
