# How Anchore can help CIS Docker Benchmark

## Introduction

As Docker usage has greatly increased, it has become increasingly important to gain a better understanding of how to securely configure and deploy Dockerized applications. The Center for Internet Security published 1.13 Docker Benchmark, which provides consesus based guidance by subject matter experts for users and organizations to achieve secure Docker usage and configuration. 

We previously published a blog on how Anchore can help achieve NIST 800-190 compliance. This post will detail how Anchore can help with certain sections of CIS Docker Benchmarks 1.13. The publication focuses on five areas that are specific to Docker: 

- Host Configuration
- Docker daemon configuration
- Docker daemon configuration files
- Container Images and Build File
- Container Runtime

Anchore is a service that analyzes Docker images pre-runtime and applies user-defined acceptance policies to allow automated container image validation and certification. Anchore is more commonly used with a CI tool similar to Jenkins in order to streamline container image builds in a more automated fashion. The critical component in helping achieve any sort of compliance are Anchore Policy Bundles. With these, users have full control over what specific policy rules they would like their Docker images to adhere to, and potentially fail or warn users based on the outcome of these evaluations.  

## Scoring Information

A scoring status indicated whether compliance with the given recommendations impacts the assessed target's benchmark score. 

### Scored

Failure to comply with "Scored" recommendations will decrease the final benchark score. Compliance with "Scored" recommendations will increase the final benchmark score. 

### Not Scored

Failure to comply with "Not Scored: recommendations will not decrease the final benchmark score. Compliance with "Not Scored" will not increase the final benchmark score. 

## Profile definitions

The following configuration profiles are defined by this Benchmark:

### Level 1 - Docker

Items in this profile intend to:

- Be practical and prudent
- Provide a clear security benefit
- Not inhibit the utility of the technology beyond acceptable means

### Level 2 - Docker

Items in this profile exhibit one or more of the following characteristics: 

- Are intended for environments or use cases where security is paramount
- Acts as defense in depth measure
- May negatively inhibit the utility or performance of the technology



## (1) Host Configuration

Security tools specific to the Host Configuration are not achievable with Anchore.

## (2) Docker daemon configuration

Security tools specific to the Docker daemon are not achievable with Anchore.


## (3) Docker daemon configuration files

Security tools specific to the Docker daemon configuration are not achievable with Anchore.

## (4) Container Images and Build files

Docker container images and their corresponding Dockerfiles govern how a container will behave when running. It is important to use the appropriate base images, and best practices when creating Dockerfiles to secure your containerized applications and infrastructure. 

### 4.1 Create a user for the container (Scored)

Create a non-root user for the container in the Dockerfile for the container image. It it generally good practice to run a Docker container as a non-root user. 

When creating Dockerfiles make sure the `USER` instruction exists. This can be achieved with an Anchore policy by checking for the `USER` instruction, as well as checking to make sure the effective user is not root.

### 4.2 Use trusted base images for containers (Not Scored)

Ensure that container images come from trusted sources. Official repositories are Docker images curated and optimized by the Docker community or vendor. As an organizational best practice, setting up a trusted Docker registry where your developers are allowed to push and pull images from is seen as secure. Configuration and use of Docker Content trust with Notary is helpful when achieving this. 

Anchore helps with this when built in with a secure CI pipeline. As an example, once an image has been built, it is then scanned and analyzed by Anchore, if it passes Anchore policies it is now safe to be pushed to a designated trusted Docker registry. If the image does not pass Anchore checks, it does not get pushed to a registry. Anchore policies can be set up to make sure base images are coming from trusted registries as well. 

### 4.3 Do not install unnecessary packages in the container (Not Scored)

It is generally a best practice to not install anything outside of the usage scope of the container. By bringing additional software packages that are not utilized, the attack surface of the container is increased. 

Anchore policies get be set to look for only a set list of software packages, or look for a slimmed down version of the base image by checking the `FROM` instruction. By using minimal base images or alpine, not only is the size of the image greatly decreased, the threat surface area of the container is decreased. 

### 4.4 Scan and rebuild the images to include security patches (Not Scored)

Images should be scanned frequently. If vulnerabilites are discovered within images, they should be patched/fixed, rebuilt, and pushed to the registry for instatiation. 

Anchore scans can be conducted as part of a normal CI pipeline, doing this ensures the frequency of scans is in-line with image builds. Anchore vulnerability feeds are consistently being updated with newer vulnerabilities as they are made available to the public. By watching image repositories and tags within Anchore, webhook notifications can be configured to alert the appropriate teams when new vulnerabilites are impactful to a watched image or tag. 

Anchore policy checks during the CI pipeline can be set up to stop container images with vulnerable software packages from ever reaching a trusted registry.

### 4.5 Enable Content trust for Docker (Scored)

Enable content trust for Docker and use digital signatures with a tool like Notary to ensure that only trusted Docker images can be pushed to registry. 

While this is not directly enforceable by Anchore, setting up Anchore policy checks within a CI pipeline to only sign images that have passed an evaluation is part of a secure CI best practice. 

### 4.6 Add HEALTHCHECK instruction to the container image (Scored)

Add the `HEALTHCHECK` instruction within your Dockerfiles. This ensures the engine will periodically check the running container against that instruction. Based on the output of the healthcheck, docker could exit a non-working container and instantiate a new one. 

Anchore policy checks can be configured to ensure the `HEALTHCHECK` instruction is present within a Dockerfile.

### 4.7 Do not use update instructions alone in the Dockerfile (Not Scored)

Make sure to not use update instruction alone or in a single line within a Dockerfile. Doing this will cache the update layer, and potentially could deny a fresh update when the Docker image is built again. 

Anchore policy checks can be configured to look for regular expressions specific to an update instruction alone or in a single line. Following this, a warning notification could be sent out. 

### 4.8 Remove setuid and setgid permissions in the images (Not Scored)

Remove setuid and setgid permission in the images to prevent escalation attacks in the containers. 

Anchore policy checks can be set to only allow setuid and setgid permission on executables which need them. These permissions could be removed during build time by explicitly stating the following in the Dockerfile:

`RUN find / -perm +6000 -type f -exec chmod a-s {} \; || true`

### 4.9 Use COPY instead of ADD in Dockerfile (Not Scored)

Use the `COPY` instruction instead of the `ADD` instruction in Dockerfiles.

Anchore policy checks can be setup to warn when `ADD` instruction in present in a Dockerfile.

### 4.10 Do not store secrets in Dockerfiles (Not Scored)

Do not store secrets in Dockerfiles.

Anchore policy checks can be configured to look for secrets (AWS keys, API keys, or other regular expressions) that may be present within an image.


### 4.11 Install verified packages only (Not Scored)

Verify authenticity of packages before installing them in the image. 

Since Anchore can inspect the Dockerfile, policy checks can be configured to only allow allowed packages to be installed during a Docker build. 

## (5) Container Runtime

Although Anchore focuses in mainly pre-runtime, there are countermeasures that can be taken during the build stage prior to instatiation to help mitigate container runtime threats. 

### 5.6 Do not run ssh within containers (Scored)

SSH server should not be running within the conatiner. 

Anchore policies can be configured to check for exposed port 22. 

### 5.7 Do not map privileged ports within containers (Scored)

The TCP/IP port number below 1024 are considers privileged ports. Normal users and processes are not allowed to use them for various security reasons. 

Anchore policies can be configured to check for these exposed ports. 

### 5.8 Only open needed ports on container (Scored)

Dockerfile for container images should only defined needed ports for container usage. 

Anchore policies can be configured to check that the needed exposed ports are open.


## Conclusion

The above findings outline which sections of the CIS Docker Benchmark can achieved with Anchore and Anchore policies. It is highly recommended that other tools be used in combination to achieve and secure CI image pipeline in order to accomplish a more complete CIS Docker Benchmark score. 