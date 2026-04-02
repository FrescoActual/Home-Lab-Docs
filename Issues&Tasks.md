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
## [Issue #7](https://github.com/users/FrescoActual/projects/1?pane=issue&itemId=163211825)
I wasn't going to write anything since this process was incredibly easy, but I'll give a little bit of information. I wanted Tailscale so I could connect to my homelab devices from anywhere. I learned about this at HOU.SEC.CON; the speaker said this was essential. 

Tailscale is a platform that will enable connectivity between devices, regardless if they are on the same network or not. Tailscale acts like a VPN, but uses WireGuard protocols. 

Set up was so easy, it's as easy as downloading it onto a device (mobile, laptop, desktop, local server, cloud-based server) and log into Tailscale. It really is that simple. I was prepared for configuring different settings, but that is not needed at all. I won't talk about the steps in detail, but I will link [a video](https://www.youtube.com/watch?v=sPdvyR7bLqI), in case I am not making sense. 
## [Issue #8](https://github.com/users/FrescoActual/projects/1?pane=issue&itemId=164168852) Put together mini server rack
I got a 10" 8U mini server rack for my home lab, and I have been excited to put it together. The [server rack](https://www.amazon.com/GeeekPi-Cabinet-Equipment-RackMate-Rackmount/dp/B0FBFDZD4C/ref=sr_1_1?crid=3G1IXYGP7JEGM&dib=eyJ2IjoiMSJ9.uexH0SuPJTHUgYz_oh6TBRE1IuBKegqnrwgMQFW64YUfOo2dEcBo5EGCkwbFq6dTRAmOkaPfAnvDOf6nxV9huxAKlgXR33PBvKjKw5_EZae9ogMm7GJUMaZ9fmxzRAFMUfTRBuL85Z0crTYqZzTcSk_5Uf1w6QWS79LmH9kKd847gJ6WkNlqjsisgtb04WOo7qAEspHAOo_8IkxdpztkjogQrEP3aTcbbfYT_F20goI.jsVVde0IinnjXxsORAxmdPEwRjrY2k7OsztOLAfEXk0&dib_tag=se&keywords=deskpi%2Bt1&qid=1774840199&sprefix=deskpi%2B%2Caps%2C178&sr=8-1&th=1) that I got was from Geeekpi off of Amazon. I think this mini server rack is good for people, like me, starting home labs for themselvs because it is much cheaper and smaller than a full sized server rack.

What I plan on doing with this server rack includes:
- 1U Shelf for a [switch](https://www.amazon.com/Ethernet-Unmanaged-Shielded-Replacement-TL-SG108E/dp/B00K4DS5KU/ref=sr_1_1?crid=2W2IPGP6HEBD2&dib=eyJ2IjoiMSJ9.yna0eVcSOiEiQ_Xq8M4vRut_8_zSCol1-dtcXf6MNle0FK11GC0mK6XbMxOQ1uyenlPOjtJZtK_JNIEN-RJbrZ6LlJ5kkdcDZ31GW39TxLlg-CRO4zVFtQHP2qoKZQOTJOwxwVFARxPqTO4Fvst_7cOb0G4ioUmsZNa_tBM9CYg6nnqCfYZqezTks0kBptMHWD-Y7LSi3E6e-yIOgTa-2WYFzRF6SgelmA365_WISpo.r8v-7suXfgFeys479bxEHGQi41Ouk9ZPwn8vbk7XfOw&dib_tag=se&keywords=tp%2Blink%2Btl-sg108e&qid=1774840772&sprefix=tp%2Blink%2Btl-sg108e%2Caps%2C178&sr=8-1&th=1)

- 1U Patch Panel
- 1U PDU (Power Distribution Unit)
- 3U open space for NAS storage bay

That only uses 6U out of the 8U. I am thinking of getting my server PC and swapping out the motherboard for a Mini ITX motherboard, as I have seen some tiny shelfs for Mini ITX boards for a 10" rack. Whatever I decide, I am really excited to start adding stuff into the rack!
## [Issue #9](https://github.com/users/FrescoActual/projects/1?pane=issue&itemId=170378609) Install Docker and Portainer
Docker has been great with running our local AI model, but I want to have more containers and a way to maintain all of them with a nice GUI. Luckily, I learned about Portainer, a management platform for Docker apps. 

1. First thing you want to do before any project is `sudo apt update && sudo apt upgrade`, unless your distro has different commands for updating packages.  
2. After that, if you don't have Docker already, you can install it with `curl -sSL https://get.docker.com | sh`. You can check if it installed properly with `docker --version`. 
3. Once Docker is installed, you want to add your user to the Docker group with `sudo usermod -aG docker $USER`. I explain this command in the [AI Pi Issue](#issue-5-create-raspberry-pi-5-that-runs-ai-locally). You may need to reboot the device to have these changes take effect. 
4. Now that Docker is all configured, we'll get Portainer to have a nice GUI to work with when we are dealing with containers. We start with `sudo docker pull portainer/portainer-ce:linux-arm`, the `linux-arm` section is what I need since I am running this on a Raspberry Pi 5, and Raspberry Pi 5 uses that ARM chip. This is how I did it, but I believe you are able to replace `linux-arm` with `latest` to automatically grab the right image for your architecture. This command pulls the Portainer Docker container. 
5. To actually create the container from the Portainer image, we will run this small, short command.. `sudo docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:linux-arm`
6. To access the GUI:
    - On the Pi: try `http://raspberry.pi.local:9000` or `http://localhost:9000`.
    - On a network device:  `http://IP_OF_YOUR_PI:9000`.
7. Now you should be able to create a new account for Portainer! 

## [Issue #10](https://github.com/users/FrescoActual/projects/1?pane=issue&itemId=170378746) Configure Pihole
Pihole is a great addition to any network. With simple installation and light-weight requirements, this can be a sweet addition to a home lab without taking up resources.

1. You can get Pihole running on a Docker container, but you can also download it onto a  [supported operating systems](https://docs.pi-hole.net/main/prerequisites/#supported-operating-systems). I chose a LXC running Ubuntu Server.
2. With Proxmox, I created an LXC with Ubuntu Server with a ***static IP address*** then installed Pihole using `curl -sSL https://install.pi-hole.net | bash`.
3. After it installation, a blue screen setup page will pop up in the terminal. It's fairly straight forward, but you can follow [this video](https://youtu.be/oX4NqFisC5Y?si=ww8p9rs4Yj1u-jeI&t=333) if you are unsure. 
4. To get the benefits of Pihole, you must set your DNS server to the IP address of the Pihole. You can set this per device, or set it up for your network via the router. DNS servers need to be static, that's why we set the LXC to have a static IP. 
5. You can check out the queries Pihole blocks and other settings via `http://IP_OF_YOUR_LXC:admin` if you selected to install the web admin page during Pihole setup. 