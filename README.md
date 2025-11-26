# github-actions-container-builder
Automated build and GHCR publishing of Docker and Apptainer containers

### Motivation
Containers are an incredible tool for software reproducibility in academia, allowing users to use software easier and quicker than ever before with greatly reduced dependency management. Many HPC systems and workflow managers such as Nextflow now support either Docker or Apptainer (formerly Singularity).

Docker is an excellent tool but is not always available as it requires root access. Apptainer is much better suited for HPC systems and can run Docker images directly, so in many cases is a 'drop-in' replacement.

However, Apptainer cannot run Docker images natively (OCI mode) in most HPC systems yet. Instead, it first needs to convert the Docker image to an Apptainer-native format. This conversion is extremely disk-bound and can take a long time, especially on slower physical storage. Many HPC usage guidelines recommend users perform the conversion in a dedicated memory-backed filesystem which speeds up the process tremendously, but this is not common knowledge.

Therefore, when writing software, it is in the author's best interests to create both native Docker and Singularity containers for seamless support under both container systems. In many cases, this can prevent a costly 20 minute conversion and allow near-instant startup of containers under Singularity on first pull.

This Github Action provides tools to create paired Docker and Singularity containers natively for your chosen image from one Dockerfile, which are then stored on the GitHub Container Registry (GHCR).

### Usage
You will need a Dockerfile.
<tbc>

### Alternative works
I was having issues with a custom Docker image, but much of my pipeline uses standalone tools which can be directly found on `conda`. For these kinds of containers, [biocontainers](https://biocontainers.pro/registry) and [seqera containers](https://seqera.io/containers/) both support Docker and Singularity builds.

### Example downstream usage
In Nextflow:
```java
process PROCESS_NAME {
  container "${ workflow.containerEngine == 'singularity' ?
    'oras://<SINGULARITY_URL_HERE>' :
    '<DOCKER_URL_HERE>' }"

  ...
}
```
