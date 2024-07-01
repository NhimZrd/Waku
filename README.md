# Waku

**Disassembly of the Waku project + node installation instructions.**

**Plan:**
- About the project;
- Investor disassembly;
- Server requirements;
- Node installation instructions;
- Conclusions

Waku is a set of messaging protocols designed for a decentralized network. It allows users to communicate with each other without the need for centralized intermediaries, while providing private and secure communication. It is important to note that Waku is part of a larger initiative to create a decentralized internet. Its main goal is to give users more control over their communication in the online environment and reduce the influence of large corporations on the internet.

Vitalik Buterin recognized Waku as the successor to Whisper, the original messaging protocol on the Ethereum network. In his blog titled "Make Ethereum Cypherpunk Again," Vitalik discusses the broader technological vision within which Ethereum exists and supports Waku as an implementation of the decentralized messaging protocol conceived by Gavin Wood.

Buterin directly supports the project, and this already says a lot. Let's go through Waku's social media accounts:
On Twitter, representatives of such Tier-1 funds as **Delphi Digital(invested in Celestia), Paradigm(invested in Blast, Uniswap), a16z(invested in EigenLayer) are subscribed to the project.** Vitalik himself is also subscribed to the account, which once again confirms his involvement in the project. Almost 5 thousand people are subscribed to the account (date of writing the review: March 15, 2024).

The Discord of the project does not have a large number of participants and serves more as a platform to help users.

The project has a Github, which contains instructions on how to install the node in various ways and various material for developers. The nwaku node on GitHub has shown continuous progress, with 46 releases since November 2020, indicating continuous improvement efforts from a strong development team. Also, Waku has a beautiful and simple website that is easy to interact with and contains all the information you need to familiarize yourself with the project in English.

**Let's move on to the Waku node installation instructions.**
**System requirements for VPS server:**

- 2 GB RAM or more;
- 2 CPU cores or more;
- 60 GB SSD or more;
- Ubuntu 20.04

**You will also need a wallet that holds at least 0.1 Ethereum Sepolia. This is necessary to enable transactions on the network. + Register an account with Alchemy. For this node, I strongly recommend creating a new wallet and saving the data from it in a secret place.**

**Detailed instructions for installing the node:**

1. open a program to connect to your rented server, connect to it. The first command is to update the server software: ``sudo apt update && sudo apt upgrade -y``.
2. Install the necessary utilities:
   ``apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y``
4. Install Docker: ``sudo apt install docker.io`` . During the installation process, confirm everything by pressing Y.
5. After installation, check the Docker version with the command: ``docker --version``. It is necessary that the Docker version is at least version 24.0.5.
6. Install Docker Compose with the command:
   ``sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose``
7.Execute the command: ``sudo chmod +x /usr/local/bin/docker-compose``.
8.Check the version of Docker Compose with the command: ``docker-compose --version``. It is necessary that the Docker Compose version is at least version 2.24.5.
9.Download the Waku node software to the server: ``git clone https://github.com/waku-org/nwaku-compose``.
10. Change the directory: ``cd nwaku-compose``.
11. Now we need to go to [Alchemy](https://dashboard.alchemy.com/) and create a new application there, with the help of which we will be able to monitor the node's operability. To do this, log in to Alchemy and go to the "Apps" section.
12. Create a new app by clicking on the "+ Create new app" button:    ![изображение](https://github.com/Mozgiii9/Waku/assets/74683169/6b076e3a-d121-4554-b444-8dbfc19448dd)
13. Select the Ethereum Sepolia network as in the picture. Also set a name for the app and add a description to the app if necessary:
    ![rrrrrrrrrr](https://github.com/Mozgiii9/Waku/assets/74683169/218b9846-43fa-4816-8e24-ec5e8c39e6ad)
14. Now here we need the following parameter: API key -> HTTPS line. In the future we will insert this in the Waku node config:
    ![rrrrrrrrrr](https://github.com/Mozgiii9/Waku/assets/74683169/83c7cdd4-9a47-4dbe-9aed-8e5a5cea45a3)
15. Now we need to get test Ethereum Sepolia. Faucet links: [1](https://sepoliafaucet.com/),[2](https://www.infura.io/faucet/sepolia),[3](https://sepolia-faucet.pk910.de/).Go to any convenient faucet, get Ethereum Sepolia.The [official documentation](https://docs.waku.org/guides/nwaku/run-docker-compose) for setting up the Waku node states that you will need at least 0.1 ETH Sepolia.
    **WARNING: As it turns out, a large amount of ETH Sepolia does not play a role in the operation of Waku node. You just need to have at least 0.1 ETH Sepolia in your wallet to ensure that the node works properly.
17. ETH Sepolia has been obtained and the application in Alchemy has been created.Now we need to change the config of the Waku node on the server.Let's run the command: ``cp .env.example .env``.
18. Next, run the command: ``nano .env``
19. Set new parameters in the config. You can navigate through the file using the arrows on the keyboard. In the "ETH_CLIENT_ADDRESS" field insert your link from the Alchemy application (see point 15). In the "ETH_TESTNET_KEY" field, enter the private key of your Ethereum Sepolia wallet.In the "RLN_RELAY_CRED_PASSWORD" field, think of and enter a password.
**FILL THE DATA IN THE CONFIG CAREFULLY, DO NOT ALLOW EXTRA QUOTES, SPACES AND OTHER SYMBOLS!DO IT AS THE PHOTO:** ![image](https://github.com/Mozgiii9/Waku/assets/74683169/fe785805-d3c3-4b16-9dbf-fcbe0f4ca126)
20. After making changes to the .env file you need to save it.To do this, press CTRL + X, then press Y, then press Enter on your keyboard.The changes should be saved.
21. Enter the command: ``./register_rln.sh`` and wait for it to complete. There may be an error, do not pay attention. Noda should still work.
22. Open ports. To do this, enter the commands one by one:

``sudo ufw enable``;

``sudo ufw allow 22``# SSH access;

``sudo ufw allow 3000``# Grafana dashboard;

``sudo ufw allow 8545``# Ethereum JSON-RPC;

``sudo ufw allow 8645``# Waku REST API;

``sudo ufw allow 9005``# discv5 routine;

``sudo ufw allow 30304`` libp2p communication

18. Final command, run node: ``docker-compose up -d`` . Your terminal should look something like this:
![изображение](https://github.com/Mozgiii9/Waku/assets/74683169/0d05e8d8-840d-4b58-a696-9a7f6f19e910)
19. Check your application in Alchemy. It should display the metrics there:
![изображение](https://github.com/Mozgiii9/Waku/assets/74683169/f425b78d-7012-40cc-bbad-4f7cb6ee6799)

If you have done everything correctly, your Waku node should be working properly. You can monitor its performance in Alchemy under "Apps".

**Conclusions:**
- The project has strong Tier-1 investors;
- Vitalik Buterin is a Twitter follower of the project and is likely an investor as well;
- The project actively interacts with the Ethereum ecosystem;
- Interesting idea that the project realizes;
- Developers are actively working on the project(46 updates since Nov 2020);
- Roadmap for 2024 indicates plans to incentivize operators, suggesting an innovative approach to attracting and expanding the community(Possible airdrop for node runners?).
