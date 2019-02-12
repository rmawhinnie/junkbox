---
title: Onboard Tooling Documentation
layout: post
author: rmawhinnie
permalink: /onboard-tooling-documentation/
source-id: 1NPkT3PWKXQ1zBh9Fqfk3ZQxEquJ-AQXmha_VvukDwRQ
published: true
---
Onboard Tooling Documentation

VM build workflow for creating VMs in Nebraska in a repeatable automated manner. 

Requirements:

* Linux VMs Template with cloud-init library installed

* Virtustream User with issued key-pair

* Access to Stackstorm Server to call workflow

## Stackstorm

[https://stackstorm.com/](https://stackstorm.com/)

In its simplest form, StackStorm is a robust engine for IFTTT operations, allowing you to set conditional logic for anything.

* Provides platform to run complex workflows.

* Can be integrated with a wide variety of different data sources, and is extensible using bash/python

* Can be access via CLI, GUI or API

### Packs

A "pack" is the unit of deployment for integrations and automations that extend StackStorm. Typically a pack is organized along service or product boundaries e.g. AWS, Docker, Sensu etc. A pack can contain Actions, Workflows, Rules, Sensors, and Aliases. StackStorm content is always part of a pack, so it's important to understand how to create packs and work with them.

### Actions

Actions are pieces of code that can perform arbitrary automation or remediation tasks in your environment, for example.

* restart a service on a server

* create a new cloud server

* send a notification or alert via email or SMS

* send a notification to a slack channel

* snapshot a VM

### Workflows

Typical data center operations and processes involve taking multiple actions across various systems. To capture and automate these operations, StackStorm uses workflows. A workflow strings atomic actions into a higher level automation, and orchestrates their executions by calling the right action, at the right time, with the right input. It keeps state, passes data between actions, and provides reliability and transparency to the execution.

### Rules

StackStorm uses rules and workflows to capture operational patterns as automations. Rules map triggers to actions (or workflows), apply matching criteria and map trigger payloads to action inputs.

### Sensors

Sensors are a way to integrate external systems and events with StackStorm. Sensors are pieces of Python code that either periodically poll some external system, or passively wait for inbound events. They then inject triggers into StackStorm, which can be matched by rules, for potential action execution.

## onboarding Pack

https://github.emcrubicon.com/Tenants/stackstorm-onboarding

Internally developed Stackstorm pack for interacting with the Xstream API

* Development Ongoing

* Provides tools to query all xstream Resources

* Provides Workflow for creating VM, and passing in cloudInit data

### Actions

#### onboarding.get 

Get Details on single resourceGUI: {Stackstorm URL}/#/actions/onboarding.get

CLI: st2 run https://github.emcrubicon.com/Tenants/stackstorm-onboarding.get 

Required Parameters:

* Private_key : xstream secret key

* Public_key: xstream public key

* Resource: Resource type that you wish to query

    * Valid Resources

        * Profile

        * ResourcePool

        * VirtualSwitch

        * HardwareTemplate

        * VirtualMachine

        * Hypervisor

        * Storage

        * Site

        * Network

        * ServiceOffering

        * User

        * TaskInfo

        * ComputeCluster

        * Tenant

        * HypervisorHost

* Tenant_id: Tenant ID that you wish to pull

* Xstream_url: Xstream URL

* Id: Resource ID to query

* Name: Resource Name to query

#### onboarding.list

Get list of all configuration items in a  resourceGUI: {Stackstomrm URL}/#/actions/onboarding.list

CLI: st2 run onboarding.list 

Required Parameters:

* Private_key : xstream secret key

* Public_key: xstream public key

* Resource: Resource type that you wish to query

    * Valid Resources

        * VirtualMachine

        * ComputeCluster

        * HypervisorHost

        * Hypervisor

        * HardwareTemplate

        * Network

        * Profile

        * ResourcePool

        * Site

        * Storage,

        * TaskInfo

        * Tenant

        * User

        * VirtualSwitch

        * ServiceAllocation

        * ServiceOffering

        * Profile

* tenant_id: Tenant ID that you wish to pull

* xstream_url: xstream URL

#### onboarding.vm.setvm

Create VM on Xstream

GUI: {Stackstomrm URL}/#/actions/onboarding.setvm

CLI: st2 run onboarding.setvm 

Required Parameters:

* Private_key : xstream secret key

* Public_key: xstream public key

* Vmspec: Json Serlization of Rerource Spec parameters

* Cloudinitspec: Json Serlization of Customizations

* Tenant_id: Tenant ID that you wish to pull

* xstream_url: xstream URL

### Workflows

#### onboarding.vm.buildmulti

Create multiple VMs on Xstream

GUI: {Stackstorm URL}/#/actions/onboarding.vm.buildmulti

CLI: st2 run onboarding.vm.buildmulti 

Required Parameters:

* github_repo - name of the github repo where specfile is store

* spec_file - name of the yaml file to build from

#### onboarding.vm.build

Build a single VM on xstream

GUI: {Stackstorm URL}/#/actions/onboarding.vm.build

CLI: st2 run onboarding.vm.build 

Parameters:

* Tenant: Tenant Name that you will be creating VMs inside of

*  ServiceOffering: The name of the compute/storage/network service offering from which this VM will be created.

* ComputeProfile:

* ServiceAllocation:

* Site: The geographic location for the VM.

* Template: The name of the hardware template youare using to create this VM.

* VmName: The user-defined name for the VM.

* Description: An optional field to describe the purpose of the VM within your environment.

* Group: 

*   NumCpu: The nuber of Cpus to allocate to the VM

* CpuHotAddEnabled

* CpuHotRemoveEnabled

* RamAllocatedMB: The amount of Virtual Memory to allocate to VM

* MemoryHotAddEnabled

* GuestOsType: The name of the OS that will be loaded into this VM.

* Disks:

    - Storage: datastore-23

      Profile: Storage profile name

      CapacityKB: Amount of KB per disk

      DeviceKey: Disk ID to assign this storage inside of VM (default to 2000 for first exsisting disk, 0 for new disk)

* Networks:

    - NetworkName: vxw-dvs-33-virtualwire-53-sid-100032-monitoring-nsx-tss-vm.sss.mnss01

      AdapterType: Type(s) of NIC card(s). Valid NIC types : Undefined = 0, VirtualE1000 = 1, VirtualPcNet32 = 2, VirtualVmxnet2 = 3, VirtualVmxnet3 = 4, VirtualVmxnet = 5, VirtualE1000e = 6

      DeviceKey:  NIC ID to assign this network inside of VM (default to 2000 for first exsisting disk, 0 for new nic)

      MacAddress: If required, Macaddress to impress on VM

*   Customizations: See [cloudinit](https://cloudinit.readthedocs.io/en/latest/) spec for valid options

# **Install**

If git

st2 pack install <git> 

If debian:

sudo apt-get install stackstorm-xstream

# **Configuration**

st2 pack config xstream

# **Development**

Use the [stackstorm-devel](https://github.emcrubicon.com/virtustream-stackstorm/stackstormdev) repo to stand up a stackstorm instance and add your local copy of the repo.

1. Edit mount file

2. vagrant reload

3. vagrant ssh

4. Follow configuration steps above.

