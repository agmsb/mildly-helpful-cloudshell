# mildly-helpful-cloudshell

Meh, mildly helpful info about Cloud Shell and also just some Linux stuff i wish people taught me earlier

## Helpful tools I use that are already there - it's free real estate baby!
```
- gcloud
- kubectl
- kubectx
- kubens
- bq
- gs
- terraform
- helm 
- hey
- curl
- docker
- skaffold
- git
- gh
- vi (lol jk)
- screen
- tmux
```

## Customizing cloud shell on startup (the fastest way to customize)

```
vi ~/.customize_environment

```
Now insert whatever trouble you want to wreak, like...

```
yes | sudo apt install bat
```

## Tmux - it's not flying, it's falling with style
```
$ tmux
```

Then _press ctrl + b before these:_

* drop a panel below: "
* drop a panel to the side: %
* navigate your panels: arrow key for direction you want

also type `exit` to kill a panel (or any session really)

## run traffic

### need to show constant response (IE canary demo)?
```
# change sleep to a hundreth of a second to really wreak havoc
while true; do curl $ENDPOINT; sleep 1; done
```
then `ctrl + c` to kill (anything really)

### need to generate traffic for a dashboard (IE i don't care about seeing the response, just need me some metrics)?

a dash of `screen` will do it. (here)[https://linuxize.com/post/how-to-use-linux-screen/]. though - just use a GCE VM. 
