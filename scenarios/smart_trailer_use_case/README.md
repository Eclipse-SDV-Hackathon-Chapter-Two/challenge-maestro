# Smart Trailer Scenario
The system detects that a smart trailer is being connected to the vehicle. A signal is raised to the orchestrator to start up the necessary providers and applications to manage the smart trailer. These include a Digital Twin Provider, which exposes signals from the trailer to higher-level applications, and a Smart Trailer application, which consumes these signals. The provided smart trailer application prints the value of a property it subscribes to, the `TrailerWeight`.

The first diagrams in the [Eclipse Ankaios](../../eclipse-ankaios/README.md) and [Eclipse BlueChi](../../eclipse-bluechi/README.md) depict this provided sample scenario.

The in-vehicle-stack is started with Eclipse Chariott, Ibeji, Agemo, Freyja, and an Eclipse Mosquitto MQTT broker inside the orchestrator environment.

## Dynamic Orchestration
This use case demonstrates a simple example of dynamic orchestration. A [script](../../in-vehicle-stack/scenarios/smart_trailer_use_case/scripts/) (one implemented for each orchestrator) will be run to monitor Ibeji and detect when the trailer is connected. The script will continuously poll for the "Trailer Connected" Digital Twin Provider, and print "NotFound" until it is started. You can simulate the trailer being connected to the vehicle by starting the ["Trailer Connected" Digital Twin Provider](../../in-vehicle-stack/scenarios/smart_trailer_use_case/digital_twin_providers/trailer_connected_provider/). This provider will register itself with Ibeji, the script will detect this change, and start up the ["Trailer Properties" Digital Twin Provider](../../in-vehicle-stack/scenarios/smart_trailer_use_case/digital_twin_providers/trailer_properties_provider/) and the [Smart Trailer Application](../../in-vehicle-stack/scenarios/smart_trailer_use_case/applications/smart_trailer_application/). This shows a simple example of reacting to an event in the vehicle by starting up other workloads.

## Trailer Applications
The Trailer Properties Provider supports the ManagedSubscribe operation so that the Smart Trailer Application can specify that it wants to receive the `TrailerWeight` property's value every 10 seconds. Using the provided configuration, Freyja is configured to sync the `TrailerWeight` every 3 seconds to a mocked cloud endpoint (which will log the signal data on standard output). See the [Cloud Connectivity Doc](../../docs/in-vehicle-stack/azure-cloud-connection.md) for instructions to synchronize signals to an Azure Digital Twins instance in the cloud. The [Digital Twin Model](../../in-vehicle-stack/scenarios/smart_trailer_use_case/digital-twin-model/dtdl/trailer.json) defines the digital twin model for the trailer, which is used as a reference for the Digital Twin Providers and Applications.

## Run the use case
Once you've chosen an orchestrator and gone through their environment setup steps, please refer to [Ankaios's Dev Environment README](../../eclipse-ankaios/README.md#startup-check-before-development) or [BlueChi's Dev Environment README](../../eclipse-bluechi/README.md#running-the-smart-trailer-example-with-bluechis-devcontainer) for instructions on running this scenario.

## Hack Challenge - Extend the use case
Take a look at the source code for the Digital Twin Model, Digital Twin Providers, and Applications used in this example and add to them or use them as a reference to create your own! See the [references](#useful-references-for-creating-and-enhancing-sample-scenarios) to help guide you as well.

When developing new workloads to run in the orchestrator environments, it is recommended to:
1. Write the code for your workload in the language of your choice.
1. Build a container image for it.
1. Push it to a container registry. You can create an [Azure Container Registry](../../docs/azure/azure_container_registry_instructions.md#aditional-information) with your [Azure Subscription](../../docs/azure/azure_code_redeem_instructions.md)
1. Follow the orchestrator-specific instructions for plugging your container image into the environments.
    - If you are using Eclipse Ankaios, please see [Workload Development](../../eclipse-ankaios/README.md#workload-development) section.
    - If you are using Eclipse BlueChi, please see the [Managing Workloads](../../eclipse-bluechi/README.md#managing-workloads) section.

If you need a basic template or idea about how to create an own containerized application for this challenge, checkout the [sample_trailer_connected_provider](in-vehicle-stack/sample_trailer_connected_provider/README.md) and treat is as a basic template for creating an own containerized application. You can extend the template with real logic and create your own application by modifying it.

Here are a few suggested ways to extend this use case, but feel free to use your imagination to come up with your own as well!

- Extend the smart trailer application to adjust some body control or powertrain functions based on the weight of the trailer to ensure a smooth trip.

- Create a web UI that displays the signals which are synced to the cloud.

- Create your own application which leverages vehicle signals to implement a different use case, such as a data collector.

## Useful References for Creating and Enhancing Sample Scenarios
This section offers guidance for creating a new sample scenario or extending the [Provided Sample Scenario](#provided-sample-scenario). While it does not provide a comprehensive list of resources, it aims to steer you in the right direction. If this section does not provide the guidance you need, please refer to the respective project’s documentation. See [Projects Involved](#projects-involved) for the project links.

### In-Vehicle Stack
If you are using the [In-Vehicle Stack](./in-vehicle-stack/README.md), these references may be useful.

- [How do I override a configuration file for a service in the in-vehicle stack?](./docs/in-vehicle-stack/config-overrides.md)
- What is Chariott's default Service Discovery's URI?
    > The default URI for the Service Discovery is `http://0.0.0.0:50000`. Please see Chariott's FAQ for more info on [Service Discovery](#chariott).
- After I pushed a newly created image to my container registry, how do I get the orchestrator to start my container image?
    > If you are using Eclipse Ankaios, please see [Workload Development](./eclipse-ankaios/README.md#workload-development) section. If you are using Eclipse BlueChi, please see the [Managing Workloads](./eclipse-bluechi/README.md#managing-workloads) section.
- After I created a new component or edited an existing component, why should I push its image to a container registry?
    > Pushing an image to the container registry is the recommended approach because it enables the orchestrators to easily reference that image, pull it from the container registry and run it as a container.
- After I decided which orchestrator to use, why should I use run its devcontainer using VSCode?
    > Running the devcontainer environment through VSCode offers an interactive approach for development. However, this is just a suggestion. You also have the option to use a devcontainer without VSCode. The devcontainer environment guarantees that you have the necessary tools to interact with each orchestrator and the in-vehicle software stack. While you can choose to develop on your local machine, you would need to install the required tools yourself. Please see [Install additional software](https://code.visualstudio.com/docs/devcontainers/create-dev-container#_install-additional-software) if you wish to install additional software into your VSCode devcontainer.
- After I edited the configuration file of a service, what should I do next?
    > You will need to restart the smart trailer scenario. If you are using Eclipse Ankaios, please see [Workload Development](./eclipse-ankaios/README.md#workload-development) section. If you are using Eclipse BlueChi, please see the [Managing Workloads](./eclipse-bluechi/README.md#managing-workloads) section.

### General
This section contains general references that may provide guidance to the scenario you choose.

#### Configuration Files
- [Which configuration files can I override?](./docs/in-vehicle-stack/config-overrides.md#how-to-override-configuration-for-in-vehicle-stack-containers)

#### Ibeji
- [How do I create an in-vehicle digital twin model?](https://github.com/eclipse-ibeji/ibeji/blob/main/docs/tutorials/in_vehicle_model/README.md)
    > This helps you construct or enhance an in-vehicle digital twin model, and enables you to reference your in-vehicle digital twin model in your code.
- [How do I create a digital twin provider or add additional capabilities to an existing provider?](https://github.com/eclipse-ibeji/ibeji/blob/main/docs/tutorials/provider/README.md)
    > A digital twin provider exposes a subset of the in-vehicle's hardware capabilities. This enables digital twin consumers to utilize that subset. Also you may find that you want to add additional capabilities, such as a new in-vehicle signal, to an existing digital twin provider. When you add new capabilities to a provider, such as a new in-vehicle signal, you will need to update the mapping client's configuration or the mapping service that you are using with Freyja. See the [Freyja FAQ](#freyja) for more details. Please see the digital twin provider [smart trailer properties' source code](./in-vehicle-stack/scenarios/smart_trailer_use_case/digital_twin_providers/trailer_properties_provider/src/main.rs) for a digital twin provider example.
    - [How do I build a container image for my digital twin provider?](https://github.com/eclipse-ibeji/ibeji/blob/main/samples/container/README.md#provider)
        > You will need to build a container image if you are updating the smart trailer digital twin provider's source code or creating your own digital twin provider.
    - [If my digital twin provider is running in a container, how do I override its configuration file?](https://github.com/eclipse-ibeji/ibeji/blob/main/samples/container/README.md#run)
        > You do not need to rebuild the container image if you are overriding your digital twin provider's configuration file.
    - [How do I use the Managed Subscribe module?](https://github.com/eclipse-ibeji/ibeji/blob/main/samples/managed_subscribe/README.md)
        > Using the Managed Subscribe module and dynamic topics in your digital twin provider allows your digital twin consumers to specify the frequency at which they want to receive updates. Please see the digital twin provider [smart trailer properties' source code](./in-vehicle-stack/scenarios/smart_trailer_use_case/digital_twin_providers/trailer_properties_provider/src/main.rs) for an example.
- [How do I create a digital twin consumer?](https://github.com/eclipse-ibeji/ibeji/blob/main/docs/tutorials/consumer/README.md)
    > A digital twin consumer is a software entity that interfaces with the digital representation of the in-vehicle hardware components. Please see the digital twin consumer [smart trailer application's source code](./in-vehicle-stack/scenarios/smart_trailer_use_case/applications/smart_trailer_application/src/main.rs) for a digital twin consumer example.
    - [How do I build a container image for my digital twin consumer?](https://github.com/eclipse-ibeji/ibeji/blob/main/samples/container/README.md#consumer)
        > You will need to build a container image if you are updating the smart trailer digital twin consumer's source code or creating your own digital twin consumer.
    - [If my digital twin consumer is running in a container, how do I override its configuration file?](./docs/in-vehicle-stack/config-overrides.md)
        > You do not need to rebuild the container image if you are overriding your digital twin consumer's configuration file.

#### Chariott
- [How can I use the Service Discovery to register and discover other applications/services?](https://github.com/eclipse-chariott/chariott/blob/main/service_discovery/README.md)
    > An application can utilize Chariott's Service Discovery to register with the system and enable other applications to discover it through the Service Discovery system. This can also be used to discover other components in the in-vehicle-stack like Ibeji.

#### Freyja
- [How do I configure new mappings for Freyja's in-memory mapping client?](https://github.com/eclipse-ibeji/freyja/blob/main/mapping_clients/in_memory_mock_mapping_client/README.md)
    > Adding a new in-vehicle signal requires you to configure a new mapping for that in-vehicle signal.
- [How do I override the mapping configuration?](https://github.com/eclipse-ibeji/freyja/blob/main/docs/config-overrides.md)
- [How do I sync in-vehicle signals to the cloud?](./docs/in-vehicle-stack/azure-cloud-connection.md)
    > You may want to sync your in-vehicle signals to a cloud digital representation of your in-vehicle.

### Eclipse Ankaios

- [Quickstart](https://eclipse-ankaios.github.io/ankaios/0.2/usage/quickstart/)
- [User documentation](https://eclipse-ankaios.github.io/ankaios/0.2/)
- [Working with the startup configuration](https://eclipse-ankaios.github.io/ankaios/0.2/reference/startup-configuration/)
- [Architecture](https://eclipse-ankaios.github.io/ankaios/0.2/architecture/)
- [Control interface examples](https://github.com/eclipse-ankaios/ankaios/tree/v0.2.0-rc1/examples)
- [Suggestions for improvement & Feedback](https://github.com/eclipse-ankaios/ankaios/discussions)

## Getting Started

Please note that it is not necessary to use both software orchestrators. You can choose either one to implement your scenario.

### Need to know
- You need basic knowledge about containerization technologies and tools (e.g. Docker, Podman), but if you have not dealt with it yet, don't worry, just check out a little tutorial (https://docs.docker.com/get-started/) to get a basic understanding of containers and you are prepared.
- Basic skills to deal with distributed systems

### Prebuilt Container Images

Please see the instructions on the [Azure Container Registry documentation](./docs/azure/azure_container_registry_instructions.md) for pulling prebuilt container images for the in-vehicle software stack.

### Eclipse Ankaios Orchestrator

If you have decided to use Ankaios, you will find an easy to use development environment in the subfolder [eclipse-ankaios](./eclipse-ankaios/README.md),
which you can use for all maestro challenges.

### Eclipse BlueChi Orchestrator

If you have decided to use BlueChi, you will find an easy to use development environment in the subfolder [eclipse-bluechi](./eclipse-bluechi/README.md),
which you can use for all maestro challenges.

### Azure Subscription

Please see the [SDV Hackathon Azure Pass Code Instructions](./docs/azure/azure_code_redeem_instructions.md) for redeeming an Azure pass code if you are interested in using resources on Azure.

## In-Vehicle Software Stack Overview

The in-vehicle stack comprises Eclipse Ibeji, Eclipse Agemo, Eclipse Freyja, and Eclipse Chariott. This stack enables a universal vehicle model to be used across different vehicles, dynamic management of vehicle signal topics for publishing and subscribing, synchronization of in-vehicle signals to a cloud-based digital twin, and the development of applications without the need for specific knowledge about the location of the resources they use.

We provide two software orchestrators, Ankaios and BlueChi, to orchestrate the in-vehicle stack. Feel free to choose either for this hackathon challenge.

## Projects Involved

### In-Vehicle Software Stack
- [Eclipse Agemo](https://github.com/eclipse-chariott/Agemo): Agemo incorporates a Pub Sub Service, a [gRPC](https://grpc.io/docs/what-is-grpc/introduction/) service that facilitates publish/subscribe operations for in-vehicle applications, including but not limited to Eclipse Ibeji and Eclipse Chariott. This service has the capability to register with Chariott, enhancing its discoverability by other applications such as Eclipse Ibeji. It offers dynamic creation and management of topics.

- [Eclipse Chariott](https://github.com/eclipse-chariott/chariott): Chariott operates as a [gRPC](https://grpc.io/docs/what-is-grpc/introduction/) service, offering a unified interface for application interaction. Chariott enables [Service Discovery](https://github.com/eclipse-chariott/chariott/blob/main/service_discovery/README.md), allowing provider applications to promote their capabilities by registering with Chariott’s service registry. Consumer applications in need of specific resources and capabilities can discover them via Chariott’s service registry.

- [Eclipse Freyja](https://github.com/eclipse-ibeji/freyja/): Freyja enables seamless synchronization between the vehicle’s digital twin and its cloud-based digital twin. This synchronization allows for a consistent and unified digital representation of the vehicle across both platforms.

- [Eclipse Ibeji](https://github.com/eclipse-ibeji/ibeji): Ibeji is designed with the goal of enabling a digital depiction of the vehicle’s state and capabilities. It achieves this through an adaptable, open, and dynamic architecture that provides access to the vehicle’s hardware, sensors, and capabilities. This extensible framework allows for a comprehensive and accurate representation of the vehicle’s current status and potential functionalities.

### In-Vehicle Software Orchestrators
- [Eclipse Ankaios](https://eclipse-ankaios.github.io/ankaios): Ankaios provides workload and container orchestration for automotive High Performance Computing (HPC) software . While it can be used for various fields of applications, it is developed from scratch for automotive use cases and provides a slim yet powerful solution to manage containerized applications. It supports various container runtimes with Podman as the first one, but other container runtimes and even native applications can be supported. Eclipse Ankaios is independent of existing communication frameworks like SOME/IP, DDS, or REST API.

- [Eclipse BlueChi](https://github.com/containers/bluechi): BlueChi is a systemd service controller intended for multi-node environments with a predefined number of nodes and with a focus on highly regulated ecosystems such as those requiring functional safety. Potential use cases can be found in domains such as transportation, where services need to be controlled across different edge devices and where traditional orchestration tools are not compliant with regulatory requirements.