
# Maestro Challenge 🚗💻🎶
- [About](#about)
- [Sample Scenarios](#sample-scenarios)
    - [Provided Sample Scenario - Truck Fleet](#truck-fleet)        
    - [Other Sample Scenarios](#other-sample-scenarios)
- [Hack Coaches](#hack-coaches)

**Do you want to be the next maestro of the next generation of vehicle software? The time is now!**

Imagine yourself as the **maestro**. You are not just writing code. You are composing a masterpiece that will drive the future of transportation. Your work will ensure that every component, from the engine control unit to the infotainment system, works in perfect harmony. So, step up to the podium, take a deep breath, and let your creativity flow. The stage is set for you to become the maestro of in-vehicle software 🚗💻🎶.

**Come hack with us!**

![Maestro](docs/diagrams/orchestra_picture.jpg)
>Photo by <a href="https://unsplash.com/@gwundrig?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Manuel Nägeli</a>.

## About

We supply both a global orchestrator using  [Eclipse Symphony](https://github.com/eclipse-symphony/symphony) and an in-vehicle stack with in-vehicle software orchestrators. Your assignment is to utilize our global orchestrator and the in-vehicle stack to construct your own scenario or replicate the scenarios provided in our [Sample Scenarios](#sample-scenarios). 


The tech stack in this challenge showcases complex in-vehicle services and workloads, utilizing the vehicle’s computing resources and capabilities, as well as other in-vehicle applications. Furthermore, it demonstrates how a fleet of vehicles can be managed with consistent, end-to-end workflows. 

Enjoy the process of bringing your vision to life!

## Prerequisites

### Computer

* **Kubernetes**
  
  [Eclipse Symphony](https://github.com/eclipse-symphony/symphony) runs on Kubernetes. To host your own Symphony instance, you'll need a Kubernetes cluster such as [Azure AKS](https://azure.microsoft.com/en-us/products/kubernetes-service), [k3s](https://k3s.io/) or [minikube](https://minikube.sigs.k8s.io/docs/).

* **Development machine**

  Any recent laptop running Windows 11, Linux, or MacOS should work. WSL 2 on Windows also works.
  Recommended software stack:
  * [Visual Studio Code](https://code.visualstudio.com/)
  * [Git Bash](https://git-scm.com/) 
  * [Docker Desktop (Optional)](https://www.docker.com/products/docker-desktop/)
  * [Helm 3](https://helm.sh/)

* **Agent deployment target**

  Any devices with a TCP/IP stack, or is attached to the development machine is a potential agent deployment target. Symphony offers agents for x86, amd64, arm7 and arm64 CPUs.



## Sample Scenarios

Here is a list of potential scenarios your team could develop. Feel free to invent your own scenarios too. Let your creativity shine ☀️ and have fun!

<a id="truck-fleet"></a>

### Provided Sample Scenario - Truck Fleet 🚚🚛🚚🚛🚚

### Truck Fleed Scenario Overview

The fleet management system allows each truck to report its cargo state at designated site offices, where it can receive new shipment tasks based on logistical needs. This system also downloads an optimized route plan to the truck, which is customized according to cargo requirements, regulatory restrictions, and driver schedules. For example, a cold chain truck might have its internal temperature adjusted automatically based on the new shipment’s needs, while the route plan ensures timely deliveries by factoring in real-time traffic, weather, and road conditions. This dynamic configuration and route planning maximize fleet efficiency, allowing for safe, compliant, and cost-effective transport of sensitive or high-priority cargo.

#### Automated docking workflow

When a park pulls off at a site, an automated workflow is triggered to:
1.	**Register** the truck with the site.
2.	**Cargo State Reporting:** Trucks with IoT sensors and telematics can automatically report cargo conditions (temperature, humidity, weight, etc.) when they stop at the designated site. This helps in tracking cargo quality and condition, ensuring compliance with regulatory requirements, and preventing spoilage or damage.
3.	**Dynamic Task Assignment:** Upon reporting, the truck may receive new shipment tasks based on logistics demands or updated customer requirements. This enhances fleet utilization and helps maintain timely deliveries.
4.	**Configuration Updates:** For specialized trucks (e.g., refrigerated or hazardous material trucks), dynamic adjustments to their configuration, such as setting new temperature ranges or other parameters, can be necessary to align with new cargo requirements. This capability supports effective cold chain management, adapting to different cargo needs without requiring the driver to make manual adjustments.

#### Simulated Field Office
The simulated field office provides a fleet orchestrator based on [Eclipse Symphony](https://github.com/eclipse-symphony/symphony) as well as a web portal. It also creates a MQTT broker through which your simulated trucks use to connect to the field office.  

#### Simulated Truck

The simulated truck is a Docker container that you can extend in several ways:
* Add custom logic to apply new configurations and software updates to your in-vehicle software stack. For example, you can implement custom logic to interact with an in-vehicle orchestrator, such as [Eclipse Ankaios](./eclipse-ankaios/README.md) and [Eclipse BlueChi](./eclipse-bluechi/README.md), to apply these updates.
* Add custom logic to handle stages in the automated docking workflow. This could be as simple as printing a message to the console or connecting to actual sensors and devices to read or set settings.

#### Run the use case

Please refer to [this document](./eclipse-symphony/README.md) for instructions on running this scenario.

You can also refers to our [YouTube paylist](https://www.youtube.com/playlist?list=PL1Gb3CbZUUEpCs1C3NUnZYoZ_tyWVOm-r) for video tutorials.

### Other Sample Scenarios
- [Smart Trailer](./scenarios/smart_trailer_use_case/README.md)
- Leverage OpenAI to enhance the vehicle’s user experience. You could develop an application that uses OpenAI’s GPT model to power an in-vehicle virtual assistant.

- Consider using OpenAI to enhance our software orchestrators, effectively creating intelligent orchestrators! OpenAI could optimize workload placements across compute nodes and/or cloud based on factors such compute usage, memory usage, network coverage, and latency.

- Create a web user interface application, such as a dashboard, that displays the various workloads' health.

## Hack Coaches
Who can be contacted for this challenge for questions. (Slack-handle)

- Eclipse Symphony: Filipe Prezado (Slack handle: @fprezado)
- Eclipse Symphony: Haishi Bai (Slack handle: @Haishi Bai)

