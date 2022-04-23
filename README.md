## Segger Embedded Studio Docker Image

Basic image to allos Segger Embedded Studio (SES) builds from Docker.

I am using this for continuous integration (CI) of SES projects.

The SES tools are installed into the image under `/ses`. For my purposes, I use `/ses/bin/emBuild`.

Example:

```docker run -it josschne/ses /ses/bin/emBuild```

Prints help information...


```docker run -it josschne/ses /ses/bin/emBuild -v <host source file path>:/src -config 'Release' /src/test-project.emProject```

Performs a build...

Done while performing work for [Dojo Five](https://dojofive.com)


## Usage

```bash
docker run -it --rm -v <host source file path>:/src ghcr.io/galshi/ses/Segger_ARM_build:segger_6.22a_sdk_15.3.0_mesh_sdk_4.2.0 /ses/bin/emBuild -D MESH_SDK=/mesh_sdk -D NRF_SDK=/sdk -config \"Release\" /src/test-project.emProject
```
