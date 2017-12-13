Note : This is a fork from the CityOfZion/neo-privatenet-docker
I tried to simplify as much as I could, feel free to ask any question on the Neo Discord (@Phyros#3665) or refer to https://github.com/CityOfZion/neo-privatenet-docker

# neo-privatenet-docker

Here we have provided a convenient way to setup a private Neo blockchain with an Ubuntu 16.04 docker image.
Please review Dockerfile for details of software.

This image is meant to skip the overhead of having to wait to get enough gas for smart contract testing on testnet and to bypass the steps of creating your own private chain.

See the section below on extracting Neo and Gas as the private chain in this docker image starts at block height 0.

You will also need to install and configure the neo-gui pc client on your favorite distro. This involves editing the protocol.json file to point the seeds at your docker IP addresses.


## Instructions

First of all, download and install docker :

If you use Windows 10 and have a Pro or Enterprise Edition : https://www.docker.com/get-docker
If you use Windows 10 Home Edition : https://docs.docker.com/toolbox/toolbox_install_windows/

Clone the repository and build the Docker image:

    git clone https://github.com/CityOfZion/neo-privatenet-docker.git
    cd neo-privatenet-docker

Download the neo-cli.zip from https://github.com/neo-project/neo-cli/releases/download/v2.4.1/neo-cli-ubuntu.16.04-x64.zip
Save it in the current directory (neo-privatenet-docker) and rename it "neo-cli.zip"

Build the neo-privnet container in command line (in Docker Start Terminal if you want) :
    
    docker build -t neo-privnet .

(At this point, you should be able to see the container in docker using this command : "docker ps -a")

Start the private network, create a wallet and automatically claim the initial NEO and 48 GAS (takes about 5 minutes):

    ./docker_run_and_create_wallet.sh

This script should start the privatenetwork container and generate a neo-privnet.wif file in the current directory.

---

There is also a turnkey Docker image with the initial 100m NEO and 16.6k GAS already claimed in a ready-to-use wallet available here: https://hub.docker.com/r/metachris/neo-privnet-with-gas/


## Install neo-gui-developer

Download the Neo GUI :

	https://github.com/CityOfZion/neo-gui-developer

In the neo-gui-developer folder, start the neo-gui.sln (with Visual Studio 2017).

In the solution, replace the protocol.json with this one :

      https://github.com/OmarSeb/neo-privatenet-docker/blob/master/configs/protocol.json

Please note the ports listed match the private chain ports in the current docker build.

If you copy the protocol.json file from the link above and replace your neo-gui protocol.json you will only need to find and edit the section that looks like the following:

"SeedList": [
    "127.0.0.1:20333",
    "127.0.0.1:20334",
    "127.0.0.1:20335",
    "127.0.0.1:20336"
],

You will need to change each occurrence of 127.0.0.1 to the IP of the system or vm running your docker image.

List the current docker machine :

	docker-machine ls

It should give the name of your docker machine (By default it's "default")
Show your docker machine IP :

	docker-machine ip default

Copy and replace this ip in the SeedList (from the protocol.json). Don't touch the port!

Replace the config.json file with this one :

     https://github.com/OmarSeb/neo-privatenet-docker/blob/master/configs/config.json


You can now build the solution, the neo-gui should start and be connected to your private network.
If you want to edit something from the protocol.json, I suggest to clean, remove bin/obj folders, and rebuild your solution before starting the neo-gui.exe.

When the neo-gui.exe start, make sure that the line in the bottom left corner of the GUI doesn't show 0s.
 i.e :  Height: 563/563/563 Connected: 4

If it shows 0, it means the neo-gui is not connected to your docker container.

## Access your wallet with Assets !

On the neo-gui, start with creating a wallet :

On top left, click Wallet > New Wallet Database to create a new wallet database.

Now, open the neo-privnet.wif file in the neo-privatenet-docker folder and copy the key.

On the GUI, make a right click > Import > Import from WIF and enter the WIF Private key you just copied.

A new address with all the NEO Tokens from your private network should appear. (Try to make Wallet > Rebuild Index to update it).



