# New Member Onboarding

## General Setup
If you're new to the PICARD team, you'll need to go through some onboarding steps to be able to contribute.
1. If you haven't already, request SSH Gateway access via [this form](https://wvu.atlassian.net/servicedesk/customer/portal/5/create/638?src=182487431). (Use 157.182.194.132 as the destination IP)
2. Once granted, verify you have access to the gateway. In macOS/WSL, type `ssh <your school username i.e. atb00010>@ssh.wvu.edu`
3. If you haven't already, have Dr. Devine or another team member create an account on the PICARD machine for you.
4. Once your new account is created, from the existing Gateway SSH session, type `ssh <username>@picard.csee.wvu.edu`
5. If you get logged in, you're all set up!

## Further Setup
> **STOP: Leave the PICARD machine before continuing.** Do not run any command in this section on the PICARD machine or the WVU gateway. Complete **every step below in a macOS or WSL terminal on your own computer**. Open a new local terminal, or type `exit` until you have returned to your own computer.

From your own computer, you can configure SSH to use an identity for PICARD, automatically jump through the WVU gateway, and forward the ports needed to use the web application.
1. **On your own computer—not PICARD or the gateway—**navigate to `~/.ssh/`. Create or open the `config` file with Nano by running `nano config`. In Nano, press `Ctrl+O`, then `Enter` to save; press `Ctrl+X` to exit.
2. Still in Nano on your own computer, add the following lines to your `config` file.
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
3. Back in the terminal on your own computer, if you type `ls` in `~/.ssh/` and do **not** see an `id_rsa` file, run `ssh-keygen -t rsa` on your own computer.
4. From your own computer, run `ssh-copy-id picard` and enter your password(s) as prompted. You may be prompted for the same password several times.
5. From your own computer, type `ssh picard` and enter your Gateway password once to connect to the PICARD machine.
6. When PICARD2 is available, open a dedicated terminal on your own computer, run `ssh picard-tunnel`, and leave it running. The command opens a forwarding-only connection, so it will not display a remote shell prompt. Visit [http://localhost:3000](http://localhost:3000) to use the application, and press `Ctrl+C` when you are ready to close the tunnel.

PICARD2 currently serves the frontend on port 3000 and its backend API on port 5000. Your browser needs both forwarded ports for the current application to work. We are working to simplify this so a future version will require only one application port.
