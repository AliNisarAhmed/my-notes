## Azure VMs

- In production, a web server uses two VMs

  - **Web server VM** - where the web server itself is installed
  - **Jump box VM** - a VM that is used to ssh into the web server VM securely

- Web server VM allows all public traffic to only port 8080 (i.e the web server)
- In order to do application updates, installation, system updates etc on the web server VM, we use the Jump-box VM
- So, using the **ssh-agent**, we ssh into the Jump-box VM, and then ssh into the web server.
- The ssh-agent "tunnels" our private key into the web server VM securely to authenticate, thus obviating the need to copy our ssh private key in our Jump-box VM.
- The jump-box VM has a public IP, but all incoming traffic is blocked on that IP using NSG rule and only ssh is allowed.
