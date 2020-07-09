# FAST Appliance

FAST Appliance is a toolkit for chaining API calls into pipelines.


Each pipeline is defined in a file in the `./pipelines` directory, and templates
are held in the `./templates` directory.

Templates can be called within the pipeline. In the following example the `cata`
template is rendered by POSTing to `http://templates/cata`.

```yaml
title: example_b
stages:
  - task: Request
    url: http://templates/cata
    options:
      method: POST
    body: $
```
The hostname is defined by the service name in `docker-compose.yml`.

## To Run

```bash
$ git clone https://github.com/zinkem5/fast-docker
$ git clone https://github.com/zinkem5/fast-http-pipeline
$ git clone https://github.com/zinkem5/fast-appliance

$ cd fast-appliance
$ docker-compose build
$ docker-compose up
```

## Verify Installation

Use postman to send the template parameters to the pipeline example endpoint

`example_b` is defined in `example_b.yaml` in the `./pipelines` folder.
The title of the template is the `title` in the pipeline file.

```
POST http://localhost/example_b

{
   "name": "your name"
}
```

expected response:

```
hello your name
```
