# Guide to Getting Started with Kata Containers on Raspberry Pi

```shell
sudo rm /dev/loop*

wget wget https://github.com/kata-containers/kata-containers/releases/download/3.2.0/kata-static-3.2.0-arm64.tar.xz
sudo tar -xf kata-static-3.2.0-arm64.tar.xz -C /

sudo ln -s /opt/kata/bin/kata-monitor /usr/local/bin
sudo ln -s /opt/kata/bin/kata-runtime /usr/local/bin
sudo ln -s /opt/kata/bin/containerd-shim-kata-v2 /usr/local/bin
sudo ln -s /opt/kata/bin/kata-collect-data.sh /usr/local/bin

ls /usr/local/bin

wget https://github.com/containerd/containerd/releases/download/v1.6.8/containerd-1.6.8-linux-arm64.tar.gz
sudo tar -xf containerd-1.6.8-linux-arm64.tar.gz -C /usr/local

ls /usr/local/bin

sudo mkdir -p /etc/containerd
sudo su
containerd config default > /etc/containerd/config.toml 
exit

sudo vim /etc/containerd/config.toml

`
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes."kata-qemu"]
          runtime_type = "io.containerd.kata.v2"
          privileged_without_host_devices = true
`

sudo vim /etc/systemd/system/containerd.service
sudo systemctl start containerd
sudo systemctl enable containerd
sudo systemctl status containerd

sudo mkdir -p /etc/kata-containers
sudo vim /opt/kata/share/defaults/kata-containers/configuration.toml

`
internetworking_model = "none"
disable_new_netns = "true"
`

image = "docker.io/library/busybox:latest"
sudo ctr image pull "$image"
sudo ctr run --runtime "io.containerd.kata.v2" --rm -t "$image" test-kata uname -r
결과 > 6.1.38

```
