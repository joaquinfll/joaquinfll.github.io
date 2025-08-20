# Network observability with `LLDP`

## What is `LLDP`?

## What information provides?

## How to enable it in `OpenShift`?

There are different capacities and to enable LLDP in OpenShift.

### Reception only with `NMState`
You can enable `LLDP` with `NMState` to receive information being published by external network devices, this is presented in the OpenShift API, CLI and WebUI.

- Install nmstate-operator
- Deploy an nmstate-operator
- Apply a dynamic `NodeNetworkConfigurationPolicy`

```
apiVersion: nmstate.io/v1
kind: NodeNetworkConfigurationPolicy
metadata:
  name: enable-lldp
spec:
  capture:
    ethernets: interfaces.type=="ethernet"
    ethernets-up: capture.ethernets | interfaces.state=="up"
    ethernets-lldp: capture.ethernets-up | interfaces.lldp.enabled:=true 
  desiredState:
    interfaces: "{{ capture.ethernets-lldp.interfaces }}"

```

### Transmission with `lldpd`

Running an additional `lldpd` or `lldpad` daemon in each node of the cluster enables the transmission of node information to external network devices, this helps to network engineers with understanding where the devices are connected to, the name of the system and other details.
To run the daemon there are different approaches
- As a DaemonSet
  - this limits to running the service once the cluster is deployed and only covers day-2 operations
- As a service deployed from an RPM in a RedHat CoreOS layer
  - this increases complexity creating an additional build step when using out-of-cluster layer build and managing cluster updates.
- As a containerized service deployed as a MachineConfig
  - this allows for applying the configuration as day-1 and day-2 if generated as an ignition file, helping with troubleshooting when deploying new clusters

#### Container build

```
FROM registry.access.com/ubi9/ubi:latest

RUN dnf install -y lldpd

ENNTRYPOINT ["lldpd", "-d"]
CMD ["-k"]
```

#### Butane Systemd Quadlet Service

lldpd-service.bu
```
variant: openshift
version: 4.18.0

metadata:
    name: lldpd-service
    labels:
        machineconfiguration.openshift.io/role: worker
systemd:
  units:
    - name: lldpd.service
storage:
  files:
    - path: /etc/containers/systemd/lldpd.container
      mode: 0644
        inline: |
          [Unit]
          Description=LLDP Daemon
          After=network.target
          [Container]
          Image=quay.io/xyz/lldp-daemon:version
          [Service]
          Restart=always
          [Install]
          WantedBy=multi-user.target

```

#### Transpiling Butane 

- podman run --interactive -rm quay.io/coreos/butane:release --pretty --strict -r < lldpd-service.bu > lldpd-service-ignition.ign
- podman run --interactive -rm quay.io/coreos/butane:release --pretty --strict < lldpd-service.bu > lldpd-service-machineconfig.yaml

## References

https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/kubernetes_nmstate/index#virt-example-enabling-lldp-policy_k8s-nmstate-updating-node-network-config
https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html