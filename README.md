# New Member Onboarding

> **Note (2026):** PICARD has been rewritten. The current product is the
> **new generation** — a Django REST + React app with a Celery/Redis queue and
> a standalone Spark cluster (repo: `ThePICARDProject/picard_new_generation`).
> The old `API-backend` (.NET) and `481-FrontEnd` repos are reference only.
> This portal documents the new generation.

## General Setup
If you're new to the PICARD team, you'll need to go through some onboarding steps to be able to contribute.
1. If you haven't already, request SSH Gateway access via [this form](https://wvu.atlassian.net/servicedesk/customer/portal/5/create/638?src=182487431). (Use 157.182.194.132 as the destination IP)
2. Once granted, verify you have access to the gateway. In macOS/WSL, type `ssh <your school username i.e. atb00010>@ssh.wvu.edu`
3. If you haven't already, have Dr. Devine or another team member create an account on the PICARD machine for you.
4. Once your new account is created, from the existing Gateway SSH session, type `ssh <username>@picard.csee.wvu.edu`
5. If you get logged in, you're all set up!

## Running the stack
The new generation runs entirely via Docker Compose:
```bash
git clone git@github.com:ThePICARDProject/picard_new_generation.git
cd picard_new_generation/PICARD2
cp backend/.env.example backend/.env   # fill in secrets + GitHub OAuth app
docker compose up --build
```
Then open <http://localhost:3000> and sign in with GitHub. See the
[Setup page](pages/setup.html) of the portal for the full walkthrough
(env vars, OAuth app, ports, and local-dev-without-Docker).

## Further Setup (SSH identity)
If you'd like, you can configure SSH to use an identity for PICARD, rather than a password.
1. In macOS/WSL from the terminal navigate to `~/.ssh/`. If the file `config` doesn't exist there, create it. Edit that file `vim config`. [Basic VIM guide](https://www.freecodecamp.org/news/vim-beginners-guide/)
2. Add the following lines to your `config` file (forward the new-gen ports — frontend `3000`, API `5000`).
```config
Host gateway
	HostName ssh.wvu.edu
	Port 22
	User <your school username>
	IdentityFile ~/.ssh/id_rsa

Host picard
	HostName picard.csee.wvu.edu
	User <your school username>
	Port 22
	IdentityFile ~/.ssh/id_rsa
	ProxyJump gateway
	LocalForward 3000 localhost:3000
	LocalForward 5000 localhost:5000
```
3. If you're in the `~/.ssh/` directory and type `ls` and do NOT see an `id_rsa` file, run `ssh-keygen -t rsa`.
4. Run `ssh-copy-id picard` and enter your password(s) as prompted. You may be prompted for the same password several times.
5. Once complete, you'll be able to type `ssh picard` and enter your Gateway password once to be fully SSH (and port tunneled) to the PICARD machine.

# Onboarding Portal
This portal is the canonical onboarding reference for the PICARD new generation.
It covers the [architecture](pages/architecture.html), the three layers
([presentation](pages/presentation.html),
[application](pages/application.html),
[execution](pages/execution.html)), and [setup](pages/setup.html). The
Parameter Sweep Engine is a documented placeholder pending the Fall 2026 group
deliverable.
