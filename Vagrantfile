Vagrant.configure("2") do |config|

  # Bento Ubuntu 24.04 Image
  config.vm.box = "bento/ubuntu-24.04"

  # Hostname
  config.vm.hostname = "ubuntu24-bento-vm"

  # Private network (Windows Host <-> VM communication)
  config.vm.network "private_network", ip: "192.168.56.20"

  # Forward Flask dashboard port to the Windows host
  config.vm.network "forwarded_port", guest: 5000, host: 5000

  # Forward Streamlit app port
  config.vm.network "forwarded_port", guest: 8501, host: 8501

  # VirtualBox configuration
  config.vm.provider "virtualbox" do |vb|
    vb.name = "ubuntu24-bento-devops"
    vb.memory = 2048
    vb.cpus = 2
  end

  # Provision the VM for the Heart-diseaseprediction project
  config.vm.provision "shell", inline: <<-SHELL
    set -e

    export DEBIAN_FRONTEND=noninteractive
    PROJECT_DIR="/vagrant/Heart-diseaseprediction"
    VENV_DIR="$HOME/heartenv"

    apt-get update
    apt-get install -y python3 python3-venv python3-pip build-essential

    if [ -d "$PROJECT_DIR" ]; then
      rm -rf "$VENV_DIR"
      python3 -m venv "$VENV_DIR"
      "$VENV_DIR/bin/pip" install --upgrade pip
      "$VENV_DIR/bin/pip" install -r "$PROJECT_DIR/requirements.txt"
    fi
  SHELL
end
