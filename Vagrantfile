Vagrant.configure("2") do |config|
  # Use the "ubuntu/bionic64" box as the base box
  config.vm.box = "ubuntu/bionic64"

  # Configure CPU and memory for the VM
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 8192 # 8GB RAM
    vb.cpus = 4     # 4 CPU cores
  end

  # Configure the VM to use the Wi-Fi interface for internet connectivity
  config.vm.network "public_network", type: "bridge", bridge: "en0: Wi-Fi"

  # Set up a private network for SSH access (optional but useful)
  config.vm.network "private_network", type: "dhcp"

  # Provisioning script to install Zsh, Power10K theme, and enable auto-completion
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y zsh

    # Change the default shell to Zsh
    chsh -s /usr/bin/zsh vagrant

    # Install Oh My Zsh for the vagrant user
    su - vagrant -c 'sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"'

    # Clone the Power10K theme if not already cloned
    if [ ! -d "/home/vagrant/.oh-my-zsh/themes/powerlevel10k" ]; then
      su - vagrant -c 'git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /home/vagrant/.oh-my-zsh/themes/powerlevel10k'
    fi

    # Set Zsh theme to Power10K for the vagrant user
    echo 'ZSH_THEME="powerlevel10k/powerlevel10k"' >> /home/vagrant/.zshrc

    # Install zsh-autosuggestions and zsh-syntax-highlighting plugins
    su - vagrant -c 'git clone https://github.com/zsh-users/zsh-autosuggestions /home/vagrant/.oh-my-zsh/custom/plugins/zsh-autosuggestions'
    su - vagrant -c 'git clone https://github.com/zsh-users/zsh-syntax-highlighting.git /home/vagrant/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting'

    # Update plugins in .zshrc
    sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/' /home/vagrant/.zshrc

    # Source the updated .zshrc for the vagrant user
    su - vagrant -c 'source /home/vagrant/.zshrc'
  SHELL
end
