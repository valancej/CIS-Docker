# How Anchore can help CIS Docker Benchmark

## Introduction

As Docker usage has greatly increased, it has become increasingly important to gain a better understanding of how to securely configure and deploy Dockerized applications. The Center for Internet Security published 1.13 Docker Benchmark, which provides consesus based guidance by subject matter experts for users and organizations to achieve secure Docker usage and configuration. 

We previously published a blog on how Anchore can help achieve NIST 800-190 compliance, this post will detail how Anchore can help with CIS Docker Benchmarks 1.13. The publication focuses on five areas: 

- Host Configuration
- Docker daemon configuration
- Docker daemon configuration files
- Container Images and Build File
- Container Runtime

Anchore is a service that analyzes Docker images and applies user-defined acceptance policies to allow automated container image validation and certification. The critical component in helping achieve any sort of compliance are Anchore Policy Bundles. With these, users have full control over what specific policy rules they would like their Docker images to adhere to. 

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



## Host Configuration

Security tools specific to the Host Configuration are not achievable with Anchore.

## Docker daemon configuration

Security tools specific to the Docker daemon are not achievable with Anchore.


## Docker daemon configuration files

Security tools specific to the Docker daemon configuration are not achievable with Anchore.

## Container Images and Build files



## Container Runtime

