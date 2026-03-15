# Issues/Tasks
Listing out my issues from my project here so I can go further explain issues on Github Projects. 
## [Issue #1](https://github.com/users/FrescoActual/projects/1?pane=issue&itemId=162703667) Create a backup of Ubuntu Server
Previously, I was using Ubuntu Server on my old PC that was running a Minecraft server. While this was fun, I felt like having an old gaming PC just for a Minecraft Server was a waste of resources. So I decided I wanted to try out Proxmox to get some hands-on with a type 1 hypervisor. I still wanted my Minecraft server, so I needed to create a backup of the Ubuntu Server image. I went with Clonezilla for creating the backup, and I was able to create the backup successfully. 
## [Issue #2](https://github.com/users/FrescoActual/projects/1/views/1?pane=issue&itemId=163195155) Install Proxmox on server PC
With the backup created successfully, I needed to install Proxmox onto the server PC. Fairly straightforward, I just booted off the USB that contained the Proxmox image, went through the installation process, and successfully installed Proxmox. Only extra thing I had to do was reserve an IP address for Proxmox so that DHCP wouldn't change the IP and cause issues.
## [Issue #3](https://github.com/users/FrescoActual/projects/1?pane=issue&itemId=163208213) Restore old server PC image as a VM
Now that I have Proxmox installed, I want to create a VM of my old server PC's image that has all the files for my Minecraft server. 
1. To start, I uploaded a clonezilla iso file to Proxmox (proxmox > local > ISO Images) and launched the VM.
2. I needed to change the boot oder for the Minecarft server VM so that the clonezilla iso (on the Optical Drive) would load.
3. I also needed to add the storage device that had my backup image (VM > Hardware > Add USB device) so that clonezilla could read it.
4. I went through the lengthy process of using clonezilla to restore my backup.
5. After successfully restoring the image, I changed the boot order again so Optical Drive wasn't priority #1, and removed the USB storage device, then I restarted the VM. It booted straight into my Ubuntu Server image with no issue.
   - I had to configure proper network settings since I moved from WiFi to Ethernet. I changed the netplan config to reflect the correct settings for Ethernet, and I was able to recieve a network connection. I also had to reserve an IP for this VM, since DHCP would cause issues with the Minecraft server.
## [Issue #5](https://github.com/users/FrescoActual/projects/1/views/1?pane=issue&itemId=163211227) Create Raspberry Pi 5 that runs AI locally
This next piece of my home lab was actually pretty fun to do and fairly simple to setup. I wanted a local LLM (Large Language Model) with Ollama and Docker (for web GUI) for the privacy of keeping information local. I hope to use this project for other projects, like pen testing in a secure, ethical environment with VMs.
### **Requirements**
- Raspberry Pi 5 16GB (For better efficiency)
- Raspberry Pi  OS
- Internet
### Getting Ollama onto the Raspberry Pi
1. The first step was obvious have a Raspberry Pi. When my professor showed this project off at our Cybersecurity Club meeting, he was using a Raspberry Pi 4, but he recommended a Pi 5. Luckily, I have two available. I used my Raspberry Pi 5 16GB with Raspberry Pi OS for this project.
2. The second step was to install Ollama onto the Pi, this was easy. Single command into the terminal, `curl -fsSL https://ollama.com/install.sh | sh`. To verify it is working, use the command `ollama --version` to print out the current version of Ollama that is installed.
3. Next, to actually test Ollama is working, we want to get some models to run. From what I've read on, smaller models like `llama3.2:3b`, `phi3`/`phi3:mini`, or `gemma:2b` are good for running on Raspberry Pis, since they are smaller models with smaller parameters. This means these models require less RAM and compute power making them better for SBCs (Single-board Computers) like the Raspberry Pi. I went with the phi3 model, and the command to pull those models to your Pi is `ollama pull phi3`, or replace `phi3` with whatever model you desire. Optionally, you can test the models right now by running `ollama run phi3`, the command prompt will change to `>>>` to accept AI prompts, which then you can test the model. Crtl+C or `/exit` to exit the model.
### Installing Docker for Web GUI
4. Cool, sweet, we got the AI model running. While the terimanl interface works, a web GUI provides a better user experience and allows access from other devices on the local network. Next step was to get the webUI so that we can have a nicer layout for our prompts. To do this, we need to install Docker with another simple command, `curl -fsSL https://get.docker.com | sh`.
5. We need to add ourselves to the Docker group with `sudo usermod -aG docker $USER`. Then restart the Pi.
   - The `usermod` portion is for changing system configs for a certain user, `-aG` flags are for appending and identifying the next parameter as a group. `docker` is the group, and `$USER` is us, the user running the command.
6. To install the Open WebUI for this project, we will need to create the container using these settings below. The issue I ran into was not running it with the `--network=host` line. This line tells the container to use the same network as the host, which then allows Open WebUI to talk directly with the Ollama API that running on port 11434.
  ```
  docker rm -f open-webui
  
  docker run -d \
    --network=host \
    -e OLLAMA_BASE_URL=http://127.0.0.1:11434 \
    -v open-webui:/app/backend/data \
    --name open-webui \
    --restart always \
    ghcr.io/open-webui/open-webui:main
  ```
7. After giving the container time to start up, you should be able to access `http://IP_OF_YOUR_PI:8080` and start using the Pi! 😎

