1)Windows पर WSL enable करो

wsl --install -d Ubuntu-24.04

2)Ubuntu के अंदर package update करो

sudo apt update && sudo apt upgrade -y

3)Install Git

sudo apt install git -y

4)Docker Installation (Ubuntu on WSL)
i)sudo apt update && sudo apt upgrade -y
ii)Required Packages : sudo apt install ca-certificates curl gnupg lsb-release -y
iii)Add Docker GPG Key : sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
iv)Add Docker Repository : echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
v)Install Docker Engine + Compose : 
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
vi)Add User to Docker Group :
sudo usermod -aG docker $USER
wsl --shutdown
vii)Verify Installation:
docker --version
docker compose version


