# Build a Google Compute Image

## Prerequisites

* Google Compute service account credentials
* Google Cloud SDK ([gcloud](https://cloud.google.com/sdk/docs/install))
* [Packer](https://www.packer.io/downloads)


## Authenticate

A number of options exist, but this simplest may be to

```
gcloud auth application-default login
```

Then if you're on MacOs or Linux

```
export GOOGLE_APPLICATION_CREDENTIALS="$HOME/.config/gcloud/application_default_credentials.json"
```

or Windows

```
export GOOGLE_APPLICATION_CREDENTIALS="%APPDATA%/gcloud/application_default_credentials.json"
```


## Use Packer to build and upload an image

Copy common scripts into place

```
gh repo clone clicktruck/scripts
cp scripts/init.sh .
cp scripts/kind-load-cafile.sh .
cp scripts/inventory.sh .
cp scripts/install-krew-and-plugins.sh .
```

Type the following to build the image

```
packer init .
packer fmt .
packer validate .
packer inspect .
packer build -only='{BUILD_NAME}.*' .
```
> Replace `{BUILD_NAME}` with `standard`.  In ~10 minutes you should notice a `manifest.json` file where within the `artifact_id` contains a reference to the image ID.


### Available overrides

You may wish to size the instance and/or choose a different region to host the image.

```
packer build --var project_id=fe-cphillipson --var zone=europe-central2-a -only='standard.*' .
```
> Consult the `variable` blocks inside [googlecompute.pkr.hcl](googlecompute.pkr.hcl)



## For your consideration

* [Google Compute](https://www.packer.io/docs/builders/googlecompute) Builder
