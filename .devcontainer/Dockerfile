FROM python:latest

RUN apt-get update && \
    apt-get full-upgrade -y && \
    pip install --upgrade pip && \
    apt-get install -y \
    zsh \
    fonts-powerline \
    sudo \
    vim && \
    rm -rf /var/lib/apt/lists/*

RUN ln -sf /usr/share/zoneinfo/Africa/Casablanca /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# Install Oh My Zsh and Powerlevel10k theme
RUN curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh | bash -s -- --unattended && \
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k

# Set default shell to zsh for root
RUN chsh -s /bin/zsh root

RUN useradd -m -s /bin/zsh dev && \
    echo "dev ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/dev

# Copy Powerlevel10k configuration file
COPY .p10k.zsh /home/dev/.p10k.zsh
COPY .p10k.zsh /root/.p10k.zsh

# Install Oh My Zsh and Powerlevel10k theme for 'dev' user
USER dev
RUN curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh | bash -s -- --unattended && \
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /home/dev/.oh-my-zsh/themes/powerlevel10k && \
    echo 'source $HOME/.oh-my-zsh/themes/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc && \
    echo '[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh' >> ~/.zshrc && \
    echo 'export PATH=$PATH:/home/dev/.local/bin' >> ~/.zshrc

# Install Invoke
RUN pip install invoke