

## Software Development Key Challenges 
    How can we ensure that an app works similarly in a variety of environments (Linux , Mac , Windows or any other). One option might be VM but that will be a heavyweight solution. 

    Containers solve this issue as they encapsulate an application and it's depenedencies in such a way that the same computiong environment can run without running into problems.

## Key Terms for cotainer and docker
```
    Container: An isolated, stand-alone unit that encapsulates an application and all its dependencies. It runs consistently in any environment, independently of the host system, without affecting or being affected by it.

    Docker: Docker is an open-source platform designed to simplify the process of building, developing, and running containers. It provides all the software required, along with development capabilities, to build, run, and manage containers efficiently.

    Image: A container image is a lightweight, read-only, executable file that includes everything needed to run a piece of software: the code, runtime, libraries, environment variables, and configurations. It essentially serves as a template for creating containers.

    Containerization: The process of bundling an application together with all its dependencies into a container. This ensures that the application behaves consistently, regardless of where it is executed.

    Orchestration: The automated coordination, scheduling, and management of multi-container deployments running on a cluster of machines. Container orchestration tools include Kubernetes and Docker Swarm.
```

## Containers and Virtual Machines
```
    VMs: Require a full OS for each instance, Containers: Share the host OS kernel.

    VMs: Heavyweight and resource-intensive, Containers: Lightweight and efficient.

    VMs: Slow boot up, Containers: Fast boot time as they don't need to boot a full OS.

    VMs: Offer strong isolation via hypervisor, Containers: Provide process-level isolation.

    VMs: Suitable for diverse OS environments, Containers: Best for uniform OS environments.

    VMs: High overhead due to full OS and hypervisor, Containers: Minimal overhead with shared resources.

    VMs: Larger storage requirements, Containers: Smaller storage footprint.

    VMs: Use hypervisor for virtualization, Containers: Use container engines like Docker or Kubernetes.
```
