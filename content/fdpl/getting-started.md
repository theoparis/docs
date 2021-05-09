---
title: "Getting Started"
description: "Getting started with Flowtr Deploy"
---

## Prerequisites

- Node >= 15
- A kubernetes cluster ([see minikube](https://minikube.sigs.k8s.io/docs/))

## Installation

```bash
git clone https://github.com/creepinson/fdpl
cd fdpl
```

## Getting Started

The configuration file for fdpl is made with typescript and is located at `fdpl.ts` in the root of the project.

### Configuration Example

```typescript
import type { InputConfig } from "@flowtr/fdpl";

const config: InputConfig = {
    database: {
        type: "sqlite",
        uri: `sqlite://${process.cwd()}/data/fdpl.sqlite`,
    },
    apps: [
        {
            name: "web",
            build: {
                image: "nginx:alpine",
            },
            expose: [
                {
                    port: {
                        from: 80,
                    },
                    host: "test.localhost",
                },
            ],
        },
    ],
};

export default config;

```

### Configuration Breakdown

The first section in the configuration specifies the location of the database. You can use postgresql, mysql, and sqlite as the database type.

The next section is where you define your apps. Each app must have a name. The build configuration for your app must at least have an docker image specified. In this example, we've put `nginx`, which is a web server with a demo website applied by default.

The expose option of an app lets you *expose* services to the internet using [Caddy](https://caddyserver.com). If you do not want to expose a service using a domain, but you want to expose it locally then you can set the host property to `false`.

## Deploying

Currently you need to have ts-node-dev globally installed for this to work. (`npm i -g ts-node-dev`)

```bash
ts-node-dev src/cli.ts deploy
```

This will deploy all of the apps you have specified in fdpl.ts to the kubernetes cluster. 

## Debugging

You can view the full kubernetes cluster with [Lens](https://k8slens.dev/), in case you need more information or access to the apps that were deployed.
