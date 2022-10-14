
## Introduction

This repository contains a public release of ByteDance microservice cluster data for the benefit of research and the academic community.
The `dataset` directory contains four JSON files corresponding to four real clusters at ByteDance. 
We provide the following information for each cluster:

 * List of services, containers, and machines;
 * Mapping from containers to services;
 * Request CPU and memory for containers of each service; 
 * Information on whether a service container can be placed on a machine.
 * Assignment of containers to machines;
 * Total CPU and memory for each machine;
 * Amount of traffic between each pair of services.

For the consideration of data security, the service names, container names, and machine IPs are desensitized, 
Furthermore, a cluster's total amount of resources is normalized to 1.0.

## Data Format
#### File Overview

The related information of a cluster is packed into a JSON file. The file contains three objects, 
`ServiceList`, `MachineList` and `TrafficList`:
 * `ServiceList`: The list of service objects, which contains the information on the names, 
   CPU resource demand, memory resource demand of each service, 
   the containers for each service, and the list of machines on which each service container is allowed to be placed. 
 * `MachineList`: The list of machine objects, which contains the information on the IPs, total CPU/memory of each machine,
   as well as the current assignment of containers to machines.
 * `TrafficList`: The list of traffic relation objects, which contains each service pair's traffic amount.

```
{
  "ServiceList": [...],
  "MachineList": [...],
  "TrafficList": [...]
}
```

#### ServiceList Object
The value of the `ServiceList` object is an array that contains a list of service objects. 
A service object contains the information of a service. More specifically, it contains five key-value pairs, which are: 

 * `Service`: The name of the service;
 * `RequestCPU`: CPU resource demand of each container of this service;
 * `RequestMem`: Memory resource demand of each container of this service;
 * `ContainerList`: A service might have multiple containers. 
   The value of `ContainerList` is an array that contains the names of the containers for this service;
 * `CompatibleMachines`: The IPs of the machines on which the container of this service can place. 
   For convenience, `*` refers to the container of this service that can be placed on any cluster machine.

```
{
  "ServiceList": [
    {
      "Service": "Service0",
      "RequestCPU": 8.117132520207078e-05,
      "RequestMem": 1.8755062243777602e-05,
      "ContainerList": [
        "Container0",
        "Container1",
        "Container2"
      ],
      "CompatibleMachines": [
        "0.0.0.0",
        "0.0.0.1",
        "0.0.0.2"
      ]
    },
    {
      "Service": "Service1",
      "RequestCPU": 8.378562959874982e-06,
      "RequestMem": 1.3435203607804559e-05,
      "ContainerList": [
        "Container5"
      ],
      "CompatibleMachines": *
    }
    ...
  ],
  "MachineList": [...],
  "TrafficList": [...]
}
```

#### MachineList Object
The value of the `MachineList` object is an array that contains a list of machine objects. 
A machine object contains the information of a machine. More specifically, it contains four key-value pairs, which are: 

 * `MachineIP`: The IP (Or name) of the Machine;
 * `TotalCPU`: Total CPU resource of this machine;
 * `TotalMem`: Total memory resource of this machine;
 * `InitialDeployingContainers`: The list of containers that are placed on this machine. This was the actual assignment of containers to machines when the traces were collected.

```
{
  "ServiceList": [...],
  "MachineList": [
    {
      "MachineIP": "0.0.0.0",
      "TotalCPU": 0.0021940336525405144,
      "TotalMem": 0.0014276112945759984,
      "InitialDeployingContainers": [
        "Container0",
        "Container20",
        "Container33"
      ]
    },
    {
      "MachineIP": "0.0.0.1",
      "TotalCPU": 0.0021940336525405144,
      "TotalMem": 0.0014276112945759984,
      "InitialDeployingContainers": [
        "Container333",
        "Container334",
        "Container335",
        "Container336",
      ]
    },
    ...
  ],
  "TrafficList": [...]
}
```

#### TrafficList Object

The value of the `TrafficList` object is an array that contains a list of traffic relation objects. 
A traffic relation object contains the names of a service pair and the amount of traffic between them: 

 * `Service1`: One service of this service pairs;
 * `Service2`: Another service of this service pairs;
 * `Traffic`: Total amount of traffic between this service pair.

```
{
  "ServiceList": [...],
  "MachineList": [...],
  "TrafficList": [
    {
      "Service1": "Service19",
      "Service2": "Service407",
      "Traffic": 0.0003700360288627911
    },
    {
      "Service1": "Service53",
      "Service2": "Service117",
      "Traffic": 1.50341148549145e-05
    },
    ...
  ]
}
```

## Security

If you discover a potential security issue in this project, or think you may
have discovered a security issue, we ask that you notify Bytedance Security via our [security center](https://security.bytedance.com/src) or [vulnerability reporting email](sec@bytedance.com).

Please do **not** create a public GitHub issue.

## License

This project is licensed under the [Apache-2.0 License](LICENSE).

