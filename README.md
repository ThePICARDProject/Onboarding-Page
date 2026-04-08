# New Member Onboarding
Currently, the PICARD capstone groups are focused on building out the [Frontend](https://github.com/ThePICARDProject/481-FrontEnd) and [Backend](https://github.com/ThePICARDProject/API-backend) of the project.

## General Setup
If you're new to the PICARD team, you'll need to go through some onboarding steps to be able to contribute.
1. If you haven't already, request SSH Gateway access via [this form](https://wvu.atlassian.net/servicedesk/customer/portal/5/create/638?src=182487431). (Use 157.182.194.132 as the destination IP)
2. Once granted, verify you have access to the gateway. In macOS/WSL, type `ssh <your school username i.e. atb00010>@ssh.wvu.edu`
3. If you haven't already, have Dr. Devine or another team member create an account on the PICARD machine for you.
4. Once your new account is created, from the existing Gateway SSH session, type `ssh <username>@picard.csee.wvu.edu`
5. If you get logged in, you're all set up!

## Further Setup
If you'd like, you can configure SSH to use an identity for PICARD, rather than a password.
1. In macOS/WSL from the terminal navigate to `~/.ssh/`. If the file `config` doesn't exist there, create it. Edit that file `vim config`. [Basic VIM guide](https://www.freecodecamp.org/news/vim-beginners-guide/)
2. Add the following lines to your `config` file.
```config
Host gateway
	HostName ssh.wvu.edu
	Port 22
	User <your school username>
	IdentityFile ~/.ssh/id_rsa
	LocalForward 5080 localhost:5080

Host picard
	HostName picard.csee.wvu.edu
	User <your school username>
	port 22
	IdentityFile ~/.ssh/id_rsa
	ProxyJump gateway
	LocalForward 5080 localhost:5080
```
3. If you you're in the `~/.ssh/` directory and type `ls` and do NOT see a `id_rsa` file, run `ssh-keygen -t rsa`.
4. Run `ssh-copy-id picard` and enter your password(s) as prompted. You may be prompted for the same password several times.
5. Once complete, you'll be able to type `ssh picard` and enter your Gateway password once to be fully SSH (and port tunneled) to the PICARD machine. 

# Onboarding Portal
As of April 2026, the PICARD 2026.08 team is working on an onboarding portal which will serve as an interface and centralized location for important information and onboarding procedures for the PICARD project. The portal is in early active development. 