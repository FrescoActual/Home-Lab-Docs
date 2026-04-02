# Proxmox Setup
This is the documentation about what I want to do with Proxmox and what I have done to keep track of changes, as well as share my experience.
## :desktop_computer: System Specs
- **CPU**: AMD Ryzen 5 2600 (6 cores, 12 thread, base clock @ 3.4GHz)
- **GPU**: AMD Radeon RX 580 Armor 8GB OC (8GB GDDR5)
- **RAM**: G.Skill 16GB (8GBx2, 2133 MT/s)
- **Storage**: 500GB :broken_heart:

| <span style="font-size:1.3em;">VM Name<span/> | <span style="font-size:1.3em;">Desc.<span/> | <span style="font-size:1.3em;">vCPU<span/> | <span style="font-size:1.3em;">RAM<span/> | <span style="font-size:1.3em;">Storage<span/> | <span style="font-size:1.3em;">Network<span/> | 
|---------|-------|------|-----|---------|---------|
| MCserver | Ubuntu Server running a Minecraft server | 4 CPUs | 5 GB | 10 GB | Bridge

| <span style="font-size:1.3em;">LXC Name<span/> | <span style="font-size:1.3em;">Desc.<span/> | <span style="font-size:1.3em;">vCPU<span/> | <span style="font-size:1.3em;">RAM<span/> | <span style="font-size:1.3em;">Storage<span/> |
| ---------|-------|------|-----|---------|
| pihole | Ubuntu Server with Pihole installed | 2 CPUs | 1 GB | 4 GB |
