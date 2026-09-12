require "yaml"

cfg = YAML.load_file(File.join(__dir__, "config.yaml"))
cluster = cfg.fetch("cluster")
vm_cfg = cfg.fetch("vm")
network = cluster.fetch("network")

Vagrant.configure("2") do |config|
  config.vm.box = vm_cfg.fetch("box")

  cluster.fetch("control_planes").times do |i|
    name = "ai-k8s-cp#{i + 1}"
    ip = "#{network}.#{cluster.fetch("control_plane_start_ip") + i}"

    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network "private_network", ip: ip
      node.vm.provider "virtualbox" do |vb|
        vb.name = name
        vb.cpus = vm_cfg.fetch("control_plane").fetch("cpus")
        vb.memory = vm_cfg.fetch("control_plane").fetch("memory")
      end
    end
  end

  cluster.fetch("workers").times do |i|
    name = "ai-k8s-worker#{i + 1}"
    ip = "#{network}.#{cluster.fetch("worker_start_ip") + i}"

    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network "private_network", ip: ip
      node.vm.provider "virtualbox" do |vb|
        vb.name = name
        vb.cpus = vm_cfg.fetch("worker").fetch("cpus")
        vb.memory = vm_cfg.fetch("worker").fetch("memory")
      end
    end
  end
end
