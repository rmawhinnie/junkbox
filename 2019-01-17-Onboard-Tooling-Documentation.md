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

### Worlkflows

Typical data center operations and processes involve taking multiple actions across various systems. To capture and automate these operations, StackStorm uses workflows. A workflow strings atomic actions into a higher level automation, and orchestrates their executions by calling the right action, at the right time, with the right input. It keeps state, passes data between actions, and provides reliability and transparency to the execution.

### Rules

StackStorm uses rules and workflows to capture operational patterns as automations. Rules map triggers to actions (or workflows), apply matching criteria and map trigger payloads to action inputs.

### Sensors

Sensors are a way to integrate external systems and events with StackStorm. Sensors are pieces of Python code that either periodically poll some external system, or passively wait for inbound events. They then inject triggers into StackStorm, which can be matched by rules, for potential action execution.

## xStream Pack

[https://github.emcrubicon.com/ent-sre/stackstorm-xstream](https://github.emcrubicon.com/ent-sre/stackstorm-xstream)

Internally developed Stackstorm pack for interacting with the Xstream API

* Development Ongoing

* Provides tools to query all xstream Resources

* Provides Workflow for creating VM, and passing in CloudInit data

### Actions

#### Xstream.get 

Get Details on single resourceGUI: {Stackstomrm URL}/#/actions/xstream.get

CLI: st2 run xstream.get 

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

        *  Storage

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

#### xstream.list

Get list of all configuration items in a  resourceGUI: {Stackstomrm URL}/#/actions/xstream.list

CLI: st2 run xstream.list 

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

        *  Storage

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

#### xstream.vm.setvm

Create VM on Xstream

GUI: {Stackstomrm URL}/#/actions/xstream.setvm

CLI: st2 run xstream.list 

Required Parameters:

* Private_key : xstream secret key

* Public_key: xstream public key

* Vmspec: Json Serlization of Rerource Spec parameters

* Cloudinitspec: Json Serlization of Customizations

* Tenant_id: Tenant ID that you wish to pull

* Xstream_url: Xstream URL

### Worlfows

#### Xstream.vm.buildmulti

Create VM on Xstream

GUI: {Stackstomrm URL}/#/actions/xstream.vm.buildmulti

CLI: st2 run xstream.vm.buildmulti 

Required Parameters:

* Vms - Json list of vms to build

Tenant: Name of Tenant to be used in creating VM

ServiceOffering: Name of Compute Service Offering

ComputeProfile: The compute Profile Name to be utilized

Site: Name of the Site to create the VM in

Template: Name of the VM Temaplte to be referenced in creating VM

VmName: The display name for this VM

Description: Description of VM

NumCpu: The number of virtual CPUs to add to this VM

RamAllocatedMB: The amount of RAM to allocate to this VM in megabytes

Disks: List of Disks to be added to VM

   - Storage: Name of storage pool to pull from

     StorageProfile: Storage Profile Name

     CapacityKB: Disk capacity in Kilobytes

     DeviceKey: Disk Key from VM

Networks: List of Networks to attach to VM

  -  NetworkName: Network Name where NIC is going to connect to (required)

     AdapterType: Type of Network Card to be utilized

        - 0: Undefined

        - 1: VirtualE1000

        - 2: VirtualPcNet32

        - 3: VirtualVmxnet2

        - 4: VirtualVmxnet3

        - 5: VirtualVmxnet

        - 6: VirtualE1000

     DeviceKey: NIC device key in the template, or 0 if the new NIC should be added

     MacAddress: NIC MAC address

Customizations: Cloud Init Specs to be passed to VM for build"

#### Xstream.vm.build

Build a single VM on xstream

GUI: {Stackstomrm URL}/#/actions/xstream.vm.build

CLI: st2 run xstream.vm.build 

Required Parameters:

* Tenant: Name of Tenant to be used in creating VM

* ServiceOffering: Name of Compute Service Offering

* ComputeProfile: The compute Profile Name to be utilized

* Site: Name of the Site to create the VM in

* Template: Name of the VM Temaplte to be referenced in creating VM

* VmName: The display name for this VM

* Description: Description of VM

* NumCpu: The number of virtual CPUs to add to this VM

* RamAllocatedMB: The amount of RAM to allocate to this VM in megabytes

* Group: The business group name for this VM   

* Disks: List of Disks to be added to VM

      - TenantID: TenantID to be use by Disk

        TargetSite: SiteID that that Disk will be built in

        Storage: Name of storage pool to pull from

        Profile: Storage Profile Name

        CapacityKB: Disk capacity in Kilobytes

        DeviceKey: Disk Key from VM"

*   Networks: List of Networks to attach to VM

      - TenantID: TenantID to be use by Disk

        NetworkName: Network Name where NIC is going to connect to (required)

        AdapterType: Type of Network Card to be utilized

          - 0: Undefined

          - 1: VirtualE1000

          - 2: VirtualPcNet32

          - 3: VirtualVmxnet2

          - 4: VirtualVmxnet3

          - 5: VirtualVmxnet

          - 6: VirtualE1000e

        DeviceKey: NIC device key in the template, or 0 if the new NIC should be added

        MacAddress: NIC MAC address"

* Customizations: Cloud Init Specs to be passed to VM for build

