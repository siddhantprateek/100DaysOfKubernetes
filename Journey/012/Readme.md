<<<<<<< HEAD
## Day 12

# What is k9s?
[K9s](https://k9scli.io/) is a **terminal-based tool that visualises the resources within your cluster and the connection between those.** It helps you to access, observe, and manage your resources. "K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.
=======
## Day 12 

 # K9s
>>>>>>> 894dd6b75a1c61e2d737966d30418f8761c0b9d4

Managing your kubernetes cluster through kubectl commands is difficult. 

<<<<<<< HEAD
## Getting Started

The configuration for K9s is kept in your home directory under .k9s $HOME/.k9s/config.yml.
=======
- You have to understand how the different components interact, specifically, what resources is linked to which other resource
- Using a UI usually abstracts all the underlying resources, not allowing for the right learning experience to take place + forcing your to click buttons
- You do not have full insights on the resources within your cluster.

These are some of the aspects that K9s can help with.


K9s is a **terminal-based tool that visualizes the resources within your cluster and the connection between those.** It helps you to access, observe, and manage your resources. "K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources." 


### Getting k9s Setup

* MacOS
```
 # Via Homebrew
 brew install derailed/k9s/k9s
 # Via MacPort
 sudo port install k9s
```

* Linux
```
 # Via LinuxBrew
 brew install derailed/k9s/k9s
 # Via PacMan
 pacman -S k9s
```

* Windows
```
# Via scoop
scoop install k9s
# Via chocolatey
choco install k9s
```

#### Building From Source
    
- K9s is currently using go v1.14 or above. In order to build K9 from source you must:

    1. Clone the repo
    2. Build and run the executable
```
make build && ./execs/k9s
```
>>>>>>> 894dd6b75a1c61e2d737966d30418f8761c0b9d4
