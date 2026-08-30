# New Member Onboarding

## General Setup
If you're new to the PICARD team, you'll need to go through some onboarding steps to be able to contribute.
1. If you haven't already, request SSH Gateway access via [this form](https://wvu.atlassian.net/servicedesk/customer/portal/5/create/638?src=182487431). (Use 157.182.194.132 as the destination IP)
2. Once granted, verify you have access to the gateway. In macOS/WSL, type `ssh <your school username i.e. atb00010>@ssh.wvu.edu`
3. If you haven't already, have Dr. Devine or another team member create an account on the PICARD machine for you.
4. Once your new account is created, from the existing Gateway SSH session, type `ssh <username>@picard.csee.wvu.edu`
5. If you get logged in, you're all set up!

## Further Setup
You can configure SSH to use an identity for PICARD, automatically jump through the WVU gateway, and forward the ports needed to use the web application.
1. In macOS/WSL from the terminal navigate to `~/.ssh/`. If the file `config` doesn't exist there, create it. Edit that file `vim config`. [Basic VIM guide](https://www.freecodecamp.org/news/vim-beginners-guide/)
2. Add the following lines to your `config` file.
```config
Host gateway
	HostName ssh.wvu.edu
	Port 22
	User <your school username>
	IdentityFile ~/.ssh/id_rsa

Host picard
	HostName picard.csee.wvu.edu
	User <your PICARD username>
	Port 22
	IdentityFile ~/.ssh/id_rsa
	ProxyJump gateway

Host picard-tunnel
	HostName picard.csee.wvu.edu
	User <your PICARD username>
	Port 22
	IdentityFile ~/.ssh/id_rsa
	ProxyJump gateway
	SessionType none
	ExitOnForwardFailure yes
	LocalForward 3000 127.0.0.1:3000
	LocalForward 5000 127.0.0.1:5000
```
3. If you you're in the `~/.ssh/` directory and type `ls` and do NOT see a `id_rsa` file, run `ssh-keygen -t rsa`.
4. Run `ssh-copy-id picard` and enter your password(s) as prompted. You may be prompted for the same password several times.
5. Once complete, you'll be able to type `ssh picard` and enter your Gateway password once to connect to the PICARD machine.
6. When PICARD2 is available, run `ssh picard-tunnel` in a dedicated terminal and leave it running. The command opens a forwarding-only connection, so it will not display a remote shell prompt. Visit [http://localhost:3000](http://localhost:3000) to use the application, and press `Ctrl+C` when you are ready to close the tunnel.

PICARD2 currently serves the frontend on port 3000 and its backend API on port 5000. Your browser needs both forwarded ports for the current application to work. We are working to simplify this so a future version will require only one application port.
