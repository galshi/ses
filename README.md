# Usage

```bash
docker run -it --rm -v <host source file path>:/src docker.io/galshi/ses:segger_6.22a_sdk_15.3.0_mesh_sdk_4.2.0 /ses/bin/emBuild -D MESH_SDK=/mesh_sdk -D NRF_SDK=/sdk -config \"Release\" /src/test-project.emProject
```