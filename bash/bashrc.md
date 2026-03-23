# start SSH-agent automatically if none is running, otherwise set environment
```bash
SSH_ENV="$HOME/.ssh/environment"

function start_agent {
    echo "Start new ssh-agent..."
    eval "$(ssh-agent -s)" > /dev/null
    echo "export SSH_AUTH_SOCK=$SSH_AUTH_SOCK" > "${SSH_ENV}"
    echo "export SSH_AGENT_PID=$SSH_AGENT_PID" >> "${SSH_ENV}"
    chmod 600 "${SSH_ENV}"
    # Add key 
    ssh-add ~/.ssh/id_ed25519
    echo "export SSH_IDENTITY=\"$(ssh-add -l)\"" >> "${SSH_ENV}"
}

if [ -f "${SSH_ENV}" ]; then
    . "${SSH_ENV}" > /dev/null
    # check if process is still running
    ps -p $SSH_AGENT_PID > /dev/null 2>&1 || {
        start_agent
    }
    # check if identity is still present, useful for WSL
    if ! ssh-add -l | grep "$SSH_IDENTITY" > /dev/null 2>&1; then
       ssh-add ~/.ssh/id_ed25519
    fi
else
    start_agent
fi
```
