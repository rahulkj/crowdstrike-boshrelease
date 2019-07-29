# Crowdstrike
## General Bootstrapping a Release Steps

## From the release directory you have created execute the following command (this is one time only, just for new BOSH add-on):
`bosh init-release`
## Creates the following directories
├ config
│   ├ blobs.yml
│   └ final.yml
├ jobs
├ packages
└ src

## Copy Falcon sensor agent (Crowdstrike agent) to src directory, example:
├ src
│   ├  falcon-sensor_5.12.0-7603_amd64.deb

## Register blobs or a new version using the following command:
`bosh add-blob src/falcon-sensor_5.12.0-7603_amd64.deb falcon-sensor_5.12.0-7603_amd64.deb`

# This poplulates the blobs.yml with filename, filename size, and SHA

# Creates the skeleton for packages (First time only):
`bosh generate-package crowdstrike-agent`

# Creates the following directories under packages:
├ packages
│   └ crowdstrike-agent
│   ├ packaging
│   └ spec


# Creates the skeleton for jobs (First time only):
`bosh generate-job crowdstrike-agent`

# Creates the following directories under jobs:
├ jobs
│   └ crowdstrike-agent
│   ├ monit
│   ├ spec
│   └ templates

# Add all the logic in the skeletons:
# Update the file /packages/crowdstrike-agent/spec with the appropriate file name of the deb package
# Only Update packages/crowdstrike-agent/packaging, if installation steps have changed
# Update these files /jobs/crowdstrike-agent/spec & jobs/crowdstrike-agent/monit, if packaged name has changed and/or monitoring method has changed
# Note: /jobs/crowdstrike-agent/templates/in/ctl.erb is where the commands to install and configure the agent reside


# Dev Release
`bosh create-release --force`

Target BOSH director
BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=<Insert Here> BOSH_CA_CERT=<Insert Here> BOSH_ENVIRONMENT=<Insert Here>

`bosh upload-release`

Update release version within crowdstrike-boshrelease/runtime-config
Example:
version: 0+dev.6

`bosh update-runtime-config --name=crowdstrike-agent runtime-config/crowdstrike.yml`

Apply changes from Ops Manager

# Final Release
`bosh create-release --final`

Target BOSH director
BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=<Insert Here> BOSH_CA_CERT=<Insert Here> BOSH_ENVIRONMENT=<Insert Here>

`bosh upload-release`

Update release version within crowdstrike-boshrelease/runtime-config
Example:
version: 1

`bosh update-runtime-config --name=crowdstrike-agent runtime-config/crowdstrike.yml`

Apply changes from Ops Manager
