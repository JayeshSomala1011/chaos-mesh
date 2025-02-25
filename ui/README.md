<!-- markdownlint-disable-file MD033 -->
<!-- markdownlint-disable-file MD041 -->

<img src="../static/logo.svg" width="450" alt="Chaos Mesh Logo" />

# UI

A Web UI for Chaos Mesh. Powered by ⚛️ [Create React App](https://github.com/facebook/create-react-app).

- [How to develop](#how-to-develop)
  - [Main technologies](#main-technologies)
  - [Bootstrap](#bootstrap)
    - [Prepare](#prepare)
    - [Install deps](#install-deps)
    - [Start](#start)
  - [Packages](#packages)
    - [mui-extends](#mui-extends)
- [License](#license)

## How to develop

The following content can help you understand the overall structure of UI and how to develop it.

### Main technologies

<div style="display: flex; align-items: center;">
  <a href="https://www.typescriptlang.org/">
    <img src="https://cdn.worldvectorlogo.com/logos/typescript.svg" height="45" alt="TypeScript" />
  </a>
  <a href="https://reactjs.org/" style="margin-left: 15px;">
    <img src="https://cdn.worldvectorlogo.com/logos/react-2.svg" height="50" alt="React" />
  </a>
  <a href="https://redux.js.org/" style="margin-left: 15px;">
    <img src="https://cdn.worldvectorlogo.com/logos/redux.svg" height="40" alt="Redux" />
  </a>
  <a href="https://mui.com/" style="margin-left: 15px;">
    <img src="https://cdn.worldvectorlogo.com/logos/material-ui-1.svg" height="35" alt="Material UI" />
  </a>
</div>

We use Typescript + React + Redux + Material UI as the main technologies. If you are not familiar with them, please
read their documentation first.

Also, we use monorepo to manage the whole UI codebase. Here is the general structure:

```sh
ui
├── app
│   ├── src
│   │   ├── @types
│   │   ├── api
│   │   ├── components
│   │   ├── i18n
│   │   ├── images
│   │   ├── lib
│   │   ├── pages - place all landing pages
│   │   ├── reducers
│   │   ├── slices - [Redux Tookit createSlice](https://redux-toolkit.js.org/api/createSlice)
├── packages
│   ├── mui-extends
```

One is **app**, which describe the whole UI interface, and the other is **packages**, which provide more complete and independent functionalities for app use.

### Bootstrap

#### Prepare

If you haven't installed the nodejs and golang environment, checkout [https://nodejs.org/en/download/](https://nodejs.org/en/download/) and [https://golang.org/](https://golang.org/).

And we use [Yarn 1](https://classic.yarnpkg.com/en/) as the dependency management. Maybe we will migrate to Yarn 2 in the future, but not now.

#### Install deps

Into the `ui` folder, run:

```sh
yarn bootstrap
```

This command will install all deps the UI needed.

> Note:
>
> It will also build the API server binary by default, please add `--compact` if you don't want it.

Then, you need to provide an API server as a proxy, it will pass into an env var which named: `REACT_APP_API_URL`. There are three ways to get it:

- **From a remote deployed Chaos Mesh Dashboard**

  If you have Chaos Mesh deployed in a remote cluster, you can use the dashboard service URL as the proxy.

- **From a local deployed Chaos Mesh Dashboard**

  When the cluster is local (E.g., [kind](https://kind.sigs.k8s.io/) or [minikube](https://minikube.sigs.k8s.io/)), you can use [Port Forwarding](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) to access it:

  ```sh
  kubectl port-forward -n chaos-testing svc/chaos-dashboard 2333:2333
  ```

- **From local dashboard server**

  There have two ways to run chaos-dashboard server in your terminal:

  - `cd .. && go run cmd/chaos-dashboard/main.go`
  - `cd .. && ./images/chaos-dashboard/bin/chaos-dashboard` (Already build in `yarn bootstrap`)

  One is real-time, the other needs to be compiled before use. The compiled bundles an extra Swagger docs into the binary file.

#### Start

We already place a one-step script to start the UI:

```sh
# cross-env REACT_APP_API_URL=http://localhost:2333 BROWSER=none react-scripts start
yarn workspace @ui/app start:default
```

Or if you want to specify the `API_URL`:

```sh
REACT_APP_API_URL=xxx BROWSER=none yarn workspace @ui/app start
```

Then open <http://localhost:3000> to view it in the browser.

### Packages

#### mui-extends

This package extends many of mui's components for use in the UI. It will use `tsc` to compile the code, simply run:

```sh
yarn workspace @ui/mui-extends build
```

to build them.

We provide [storybook](https://storybook.js.org/) for previewing the components, you can run:

```sh
yarn workspace @ui/mui-extends build && \
yarn workspace @ui/mui-extends storybook
```

to open it.

## License

Same as Chaos Mesh.
