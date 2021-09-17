$script = <<-'SCRIPT'
echo "Installing pre-requisites(takes few minutes ...)"
sudo bash -c 'apt-get update  >/dev/null 2>&1 ' 
sudo bash -c 'apt-get install docker.io -y  >/dev/null 2>&1 '
sudo bash -c 'snap install kubectl --classic  >/dev/null 2>&1 '
sudo bash -c 'curl -s https://fluxcd.io/install.sh | sudo bash >/dev/null 2>&1 '
sudo bash -c 'curl -s https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash  >/dev/null 2>&1 '
echo "Preparing Kubernetes Cluster ..."
k3d cluster create gitopskubernetes --servers 3
kubectl get nodes
SCRIPT

Vagrant.configure("2") do |config|
       config.vm.box = "bento/ubuntu-20.04"
       config.ssh.insert_key = false
       config.hostmanager.enabled = true
       config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
       config.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", 4096]
     vb.customize ["modifyvm", :id, "--cpus", 4]
 end

  (  ["GITOPS"] ).each_with_index do |role, i|
    name = "%s"     % [role, i]
    addr = "10.0.11.%d" % (100 + i)
    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network "private_network",ip: addr
      node.vm.provision "shell", inline: $script
        end
    end

end
