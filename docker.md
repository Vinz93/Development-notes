# Docker

## Concepts

### Image
An **image** is a lightweight, stand-alone, **executable package** that **includes everything needed to run** a piece of software, including the code, a runtime, libraries, environment variables, and config files.

### Container
A container is a **runtime instance of an image**—what the image becomes in memory when actually executed. It runs completely **isolated** from the host environment by default, only accessing host files and ports if configured to do so.

Containers run apps natively on the host machine’s kernel. They have *better **performance** characteristics than virtual machines* that only get virtual access to host resources through a hypervisor. Containers can get native access, each one running in a discrete process, taking no more memory than any other executable.
## Dockerfile
