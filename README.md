# FAST Appliance

FAST Appliance is a toolkit for chaining API calls into pipelines.

# Run the public AMI on AWS EC2 (experimental)

1. Goto the EC2 console, region us-west-2
2. Goto Instances and Launch an Instance
3. Search 'fast appliance' or use the AMI
  `ami-06921b5e46656f26d`
4. Open up port 80 in the security group
  **no encryption or authorization is provided**
5. Launch Instance
6. List the configured pipelines

`$ curl http://your.aws.compute.amazonaws.com/
["example_a","example_b"]
`

### pipelines

Configure custom pipelines by placing files in
`/home/ubuntu/fast-appliance/pipelines`.

Files are specified yaml with a `title` and a `stages` property.
POSTing to the appliance with the title in the path will invoke the pipeline using
the POSTed JSON body.

### templates

FAST templates may be placed in the
`/home/ubuntu/fast-appliance/templates`
directory.

These fast templates can be called with `url: http://templates/template_name`
in the pipeline configuration YAML.

Template parameters can be inserted into the POST body using JSON Path.

More information about specifying HTTP calling stages at
https://github.com/zinkem5/fast-http-pipeline

# Configure automated workflows

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
