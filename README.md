# Crowdstrike Bosh release

## Clone this repository and execute:

`bosh init-release`


## Creates the following directories
```
├ config
│   ├ blobs.yml
│   └ final.yml
├ jobs
├ packages
└ src
```

## Copy Falcon sensor agent (Crowdstrike agent) to src directory, example:
├ src
│   ├  falcon-sensor_5.12.0-7603_amd64.deb

## Register blobs or a new version using the following command:
`bosh add-blob src/falcon-sensor_5.12.0-7603_amd64.deb falcon-sensor_5.12.0-7603_amd64.deb`

# This poplulates the blobs.yml with filename, filename size, and SHA

# Update packages > crowdstrike-agent > spec, to include the blob
```
files:
  - falcon-sensor_5.12.0-7603_amd64.deb
```

# Update the file under config > final.yml to include the `blobstore_path`
```
blobstore:
  provider: local
  options:
    blobstore_path: /Users/bob/blobs

name: crowdstrike-boshrelease
```

# Final Release
`bosh create-release --final --force --tarball=crowdstrike-boshrelease.tgz`

# Upload the bosh Release
`bosh upload-release crowdstrike-boshrelease.tgz`

# Update the file runtime-config > crowstrike.yml file and update the version number

```
version: 1
```

# Update the runtime config
`bosh update-runtime-config --name=crowdstrike runtime-config/crowdstrike.yml`


# Apply changes from Ops Manager
