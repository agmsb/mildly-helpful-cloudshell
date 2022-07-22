# mildly-helpful-cloudshell

Meh, mildly helpful info about Cloud Shell and also just some Linux stuff i wish people taught me earlier

## Helpful tools I use that are already there - it's free real estate baby!
```
- gcloud - you already know
- kubectl - interact with kubernetes
- kubectx - switch between kubernetes cluster contexts automatically
- kubens - switch between kubernetes namespaces in your current kubernetes cluster
- bq - query big
- gs - storage stuff
- terraform - create (or destroy - or dry run, whatever floats your boat) cloud resources
- helm  - deploy templatized k8s apps
- hey - fancy traffic generator
- curl - kirkland brand way to issue http requests - gets the job done
- docker - build containers
- skaffold - automate building and deploying k8s for dev
- git - manage your git repos
- gh - manage your GitHub repos
- vi (lol jk) - lol
- tmux - multiplex terminal (run stuff in background, display multiple terminals)
- screen - run stuff in background
- jq - query json documents
```

## Customizing Cloud Shell on startup (the fastest way to customize IMO)
```
vi ~/.customize_environment
```
Now insert whatever trouble you want to wreak, like...
```
# cat but fancy syntax highlighting
yes | apt install bat

# fancy display of your directory hierarchy
yes | apt install tree

# jq but for yaml
yes | apt install yq
```
Or installing your own version of gcloud!


## Tmux - it's not flying, it's falling with style
```
$ tmux
```
Then _press ctrl + b before these:_

* drop a panel below: "
* drop a panel to the side: %
* navigate your panels: arrow key for direction you want

also type `exit` to kill a panel (or any session really)

## Simulate traffic

### need to show constant response (IE canary demo)?
```
# change sleep to a hundreth of a second to really wreak havoc
while true; do curl $ENDPOINT; sleep 1; done
```
then `ctrl + c` to kill (anything really)

### need to generate traffic for a dashboard (IE i don't care about seeing the response, just need me some metrics)?

you can use tmux for this too. or a dash of `screen` will do it. [here](https://linuxize.com/post/how-to-use-linux-screen/). though - for this you're better off just using a GCE VM.

## Weird gcloud tips

Grab a project number (and see how you can use `flattened` as an output format and pipe to `awk`)
```
gcloud projects describe $PROJECT_ID --format=flattened | awk 'FNR == 7 {print $2}'
```

Grab latest cluster version in GKE (and see how you can use `value` as an output format)
```
gcloud beta container get-server-config --region us-central1 --format='value(validMasterVersions[0])'
```

## General Linux tips (general linux)
```
# Get your working directory
pwd

# Go home (roger)
cd ~

# Go up one in your working directory 
cd ..

# Go up two in your working directory
cd ../..

# Go up three in...you get the point
cd ../../..

# When typing directory or filename, autocomplete using tab

# Need to see your files in your working directory?
ls -al

# Need to see disk usage?
df

# History?
history

# Err that's too much, let me just type what I think I was looking for in history
ctrl+r and type away my friend - hit enter if you hit the jackpot

# see a file
cat

# create an empty file
touch $FILENAME

$ create a file with stuff

cat << EOF >> /path/to/filename
here is fancy file stuff
EOF
```
