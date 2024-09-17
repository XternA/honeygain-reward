# Honeygain Pot 🐝🍯

[![Static Badge](https://img.shields.io/badge/GitHub-blue?style=flat&logo=github)](https://github.com/XternA/honeygain-pot)
[![Static Badge](https://img.shields.io/badge/License-purple?style=flat&logo=github)](https://github.com/XternA/honeygain-pot?tab=License-1-ov-file)
[![Docker Pulls](https://img.shields.io/docker/pulls/xterna/honeygain-pot?logo=docker&label=Docker%20Pulls)](https://hub.docker.com/r/xterna/honeygain-pot)
[![Docker Stars](https://img.shields.io/docker/stars/xterna/honeygain-pot?logo=docker&label=Docker%20Stars)](https://hub.docker.com/r/xterna/honeygain-pot)
[![Docker Image Version (tag)](https://img.shields.io/docker/v/xterna/honeygain-pot?style=flat&logo=docker&label=Version)](https://hub.docker.com/r/xterna/honeygain-pot/tags)
[![Docker Image Size](https://img.shields.io/docker/image-size/xterna/honeygain-pot?logo=docker&label=Image%20Size&color=red)](https://hub.docker.com/r/xterna/honeygain-pot/tags)
[![GitHub Repo stars](https://img.shields.io/github/stars/XternA/honeygain-pot?style=flat&logo=github&label=Stars&color=orange)](https://github.com/XternA/honeygain-pot)
[![Donate](https://img.shields.io/badge/Donate-PayPal-blue.svg?style=flat&logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=32DCQ65QM5FNE)

### Containerised docker image for [Honeygain](https://bit.ly/3x6nX1S) lucky pot 🍯
>**Note:** This is an unofficial build and comes with no warranty of any kind. By using this image you also agree to Honeygain's T&C.

This is a simple Docker image for installing Honeygain's Lucky Pot auto-claim script as a container.

#### If you like this project, don't forget to leave a star. ⭐

## Pulling Image 🐋
**64-Bit** architecture and **ARM**:
```sh
docker pull xterna/honeygain-pot
```
**ARM (32-Bit)** is supported:
```
docker pull xterna/honeygain-pot:arm32v7
```

# Overview 🐝
[**Honeygain-Pot**](https://bit.ly/3x6nX1S) 🍯 is a script (bot) powered by NodeJS, JavaScript and Shell scripting to automatically claim your lucky pot bonus daily from [**Honeygain**](https://bit.ly/3x6nX1S)🐝.

The script is designed to be run in a docker environment, allowing it to be deployed alongside the Honeygain docker container.

It uses very minimal resources, resulting in the CPU utilisation staying at idle **0%** the entire time unless logging into the website.
```
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT   MEM %     NET I/O         BLOCK I/O     PIDS
33d34f74cd0e   honeygain-pot   0.20%     3.02MiB / 320MiB    0.94%     3.3MB / 206kB   0B / 43.6MB   3
```
> This script comes pre-bundled with [**Income Generator**](https://github.com/XternA/income-generator). A tool which consolidates and earns passive income from multiple sources.

# Features 🚀
- Automatically log in and claim daily lucky pot.
- Find out the remaining time before the next claim.
- Set up the timer and auto-wait for the duration.
- On ready to reclaim, repeat the cycle.
- If an error occurs, will cool down and re-attempt.
- Works with `Honeygain` and `JumpTask` mode.

### Output 🖥️
This is what the script looks like when you inspect the output.
```
------------ Honeygain Pot Auto Claim ------------
Starting login process...
Logging in as example@abc.com
Logged into Honeygain 🐝
--------------------------------------------------

Earning with Honeygain wallet 💰
Claimed 100 credits ✅
Won today 100 credits 🪙
Earned today 157.43 credits 🍯
Waiting for next available pot to claim 🍯
Time remaining: 23 hours 26 minutes 42 seconds ⏱️
.
.
.
Time remaining: 0 hours 0 minutes 0 seconds ⏱️
Ready to claim again ✅
```

# Usage 📃
Define the following environment variable to bootstrap the image.

| Variable | Description | Mandatory |
| :--- | :--- | :---: |
| **EMAIL**     | Your Honeygain email address    | YES |
| **PASSWORD**  | Your Honeygain password         | YES |

Or supply credentials in a Dotenv `.env` file.
```markdown
EMAIL=<email_address>
PASSWORD=<password_credential>
```

# Docker Deployment 🐋
### Compose
File: `compose.yml`
```yaml
services:
  honeygain-pot:
    container_name: honeygain-pot
    image: xterna/honeygain-pot
    restart: unless-stopped
    environment:
      - EMAIL=$EMAIL
      - PASSWORD=$PASSWORD
    dns:
      - 1.1.1.1
      - 8.8.8.8
```

With Honeygain app docker image.
```yaml
services:
  honeygain:
    container_name: honeygain
    image: honeygain/honeygain
    restart: always
    command: -tou-accept -email $EMAIL -pass $PASSWORD -device $<name_to_identify_device>
    dns:
      - 1.1.1.1
      - 8.8.8.8

  honeygain-pot:
    container_name: honeygain-pot
    image: xterna/honeygain-pot
    restart: unless-stopped
    environment:
      - EMAIL=$EMAIL
      - PASSWORD=$PASSWORD
    dns:
      - 1.1.1.1
      - 8.8.8.8
    depends_on:
      - honeygain
```

Execute where compose file is located.
```yaml
docker compose up -d
```

ℹ️ **Note:** If you apply resource limits such as CPU and RAM you need to set the following bare minimum:
```
  - cpu=0.8
  - mem_limit=350m
```
The script won't be able to run properly and will constantly timeout if the CPU and RAM limit is set lower than recommended. This is only required during the boot-up phase where it needs to spin up a headless browser to connect to the site. Resource is most intensive only during this phase.

### CLI
Using environment variable or Dotenv `.env` defined e.g.
```sh
docker run -d --restart unless-stopped --name honeygain-pot -e EMAIL=$HONEYGAIN_EMAIL -e PASSWORD=$HONEYGAIN_PASSWORD xterna/honeygain-pot
```

Directly passing credentials.
```sh
docker run -d --restart unless-stopped --name honeygain-pot -e EMAIL=example.gmail.com -e PASSWORD=pass123 xterna/honeygain-pot
```
This will start the application in the background. The alias assigned is `honeygain-pot`.

# Like my work? 🫶
Donations are warmly welcomed no matter how small and thank you very much. 😌
- **Bitcoin (BTC)** - `bc1qq993w3mxsf5aph5c362wjv3zaegk37tcvw7rl4`
- **Ethereum (ETH)** - `0x2601B9940F9594810DEDC44015491f0f9D6Dd1cA`
- **Binance Smart Chain (BSC)** - `0x2601B9940F9594810DEDC44015491f0f9D6Dd1cA`
- **Solana (SOL)** - `Ap5aiAbnsLtR2XVJB3sp37qdNP5VfqydAgUThvdEiL5i`
- **PayPal** - [@xterna](https://paypal.me/xterna)

# Disclaimer :warning:
This script is not affiliated with or endorsed by Honeygain. Use it at your own risk and responsibility.

The author does not provide any assurances, whether explicit or implicit, regarding the accuracy, completeness, or appropriateness of this script for specific purposes. The author shall not be held accountable for any damages, including but not limited to direct, indirect, incidental, consequential, or special damages, arising from the use or inability to use this script or its accompanying documentation, even if the possibility of such damages has been communicated.

By choosing to utilize this script, you acknowledge and assume all risks associated with its use. Additionally, you agree that the author cannot be held liable for any issues or consequences that may arise as a result of its usage.
