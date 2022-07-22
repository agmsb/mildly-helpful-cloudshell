# mildly-helpful-cloudshell

Meh, mildly helpful info about `Cloud Shell` and also just some Linux stuff i wish people taught me earlier

## Helpful tools I use that are already there - it's free real estate baby!
```
My favorites:
- gcloud - you already know
- kubectl - interact with kubernetes
- kubectx - switch between kubernetes cluster contexts automatically
- kubens - switch between kubernetes namespaces in your current kubernetes cluster
- terraform - create (or destroy - or dry run, whatever floats your boat) cloud resources
- helm  - deploy templatized k8s apps
- hey - fancy traffic generator
- curl - kirkland brand way to issue http requests - gets the job done
- docker - build containers
- skaffold - automate building and deploying k8s for dev

Other cool ones I guess:
- bq - query big
- gs - storage stuff
- git - manage your git repos
- gh - manage your GitHub repos
- vi (lol jk) - lol
- tmux - multiplex terminal (run stuff in background, display multiple terminals)
- screen - run stuff in background
- jq - query json documents
```

## Customizing `Cloud Shell` on startup (the fastest way to customize IMO)
```
# This runs as root lol so be careful i guess
vi ~/.customize_environment
```
Now insert whatever trouble you want to wreak, like...
```
# bat - cat but fancy syntax highlighting
yes | apt install bat

# tree - fancy display of your directory hierarchy
yes | apt install tree

# yq - i hope you never have to query yaml but here ya go
VERSION=v4.2.0
BINARY=yq_linux_amd64
wget https://github.com/mikefarah/yq/releases/download/${VERSION}/${BINARY}.tar.gz -O - |\
  tar xz && mv ${BINARY} /usr/bin/yq
```
Or installing your own version of `gcloud`!

## You could also just build your own container image to run in Cloud Shell

If you're not familiar with building container images, the `cloudshell` CLI wraps the process of building the container image nicely:

```
cloudshell env create-custom-image your-cloudshell-repo

cd $HOME/your-cloudshell-repo && rm Dockerfile

cat << EOF >> Dockerfile
FROM gcr.io/cloudshell-images/cloudshell:latest
# Install all the stuff in the example above
# bat - cat but fancy syntax highlighting
yes | apt install bat
# tree - fancy display of your directory hierarchy
yes | apt install tree
# yq - i hope you never have to query yaml but here ya go
VERSION=v4.2.0
BINARY=yq_linux_amd64
wget https://github.com/mikefarah/yq/releases/download/${VERSION}/${BINARY}.tar.gz -O - |\
  tar xz && mv ${BINARY} /usr/bin/yq
EOF

cloudshell env build-local

git commit -a -m "Initial custom environment check-in."

git push origin master

cloudshell env push
```

## Demoing multiple things using `Tmux` - it's not flying, it's falling with style
Tmux is a fancy terminal multiplexer thingy; i use tmux to show multiple things going on at once.

To use `tmux` for this you can access its key bindings by _pressing ctrl + b before these keys:_

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

you can use tmux for this too. or a dash of `screen` will do it - which for some reason i find more intuitive. [here](https://linuxize.com/post/how-to-use-linux-screen/). though - for this you're better off just using a GCE VM.

## Need to demo mostly code or config?

`Cloud Shell` comes with the open source editor [Theia](ide.cloud.google.com) installed! Go ahead, click!

Fanciest things include:
- Nice version control support (bye `git`)
- Cloning sample repositories by example 
- Cloud Code - it does so much lol
- Cloud Code for GKE - see all clusters in your `kubeconfig`, and all resources in those clusters (that you have permission to see ofc)
- Cloud Code for Cloud Run - see your services
- Cloud Code for minikube - create a baby k8s cluster! awww

## Weird gcloud tips

Grab a project number (and see how you can use `flattened` as an output format and pipe to `awk`)
```
gcloud projects describe $PROJECT_ID --format=flattened | awk 'FNR == 7 {print $2}'
```

Grab latest cluster version in GKE (and see how you can use `value` as an output format)
```
gcloud beta container get-server-config --region us-central1 --format='value(validMasterVersions[0])'
```

## Save some space!

`Cloud Shell` only comes with 5 GB for your $HOME. Lol. So clean up every once in awhile!

You could clean up `docker` images using:
```
docker system prune
docker image prune
```

Hopefully you don't ever have to demo `bazel` but I had to once and borked my `Cloud Shell` once using it, clear your `bazel` cache:
```
bazel clean
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

$ delete a file
rm FILENAME

$ delete a directory (BE CAREFUL)
rm -rf DIR
```
## More random Cloud Shell things:

- Boost mode to get an `e2-medium` machine
- You can run `Cloud Shell` fresha and clean in `ephemeral mode`.
