## ✅Step-by-Step: Enable kubectl Bash Completion (without sudo)
#1. Install kubectl completion script locally
```bash
mkdir -p ~/.bash_completion.d
kubectl completion bash > ~/.bash_completion.d/kubectl
```
#2. Source it in your .bashrc
Note:- Add this to the bottom of your ~/.bashrc:<br>
```
vi ~/.bashrc
```
```bash
# Enable kubectl completion
if [ -f "$HOME/.bash_completion.d/kubectl" ]; then
  . "$HOME/.bash_completion.d/kubectl"
fi

```
#3. Enable kubectl alias completion (optional)
If you use k as an alias for kubectl, add this too:
```bash
alias k=kubectl
complete -F __start_kubectl k

```
#4. Reload your bash config
```bash
source ~/.bashrc
```

#Now try:
```bash
kubectl ge<TAB>
```
**---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------**

## Step-by-step Instructions (Ubuntu/Debian-based systems):
**1.Install bash-completion (if not already installed):**
```
sudo apt update
sudo apt install bash-completion
```

**2.Enable bash completion in your current shell:**
**Add the following to your ~/.bashrc (if not already present):**
```
source /etc/bash_completion
```

**3.Enable kubectl bash completion:<br>**
**Append this to your ~/.bashrc:**
```
source <(kubectl completion bash)
```
**4.Reload your shell (or source the file):**

```
source ~/.bashrc
```
