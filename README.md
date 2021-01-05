# Crowdstrike Bosh release

* Clone this repository and execute:

  `bosh init-release`


* The config directory is created with the following contents:
  ```
  ├ config
  │   ├ blobs.yml
  │   └ final.yml
  ├ jobs
  ├ packages
  └ src
  ```

* Copy Falcon sensor agent (Crowdstrike agent) to `src` directory, example:
  ```
  ├ src
  │   ├  falcon-sensor_5.12.0-7603_amd64.deb
  ```

* Register blobs or a new version using the following command:
  ```
  bosh add-blob src/falcon-sensor_5.12.0-7603_amd64.deb falcon-sensor_5.12.0-7603_amd64.deb
  ```
This populates the `blobs.yml` with blob filename, filename size, and SHA

* Update `packages > crowdstrike-agent > spec`, to include the blob
  ```
  files:
    - falcon-sensor_5.12.0-7603_amd64.deb
  ```

* Update the file under `config > final.yml` to include the `blobstore_path` variable:

  ```
  blobstore:
    provider: local
    options:
      blobstore_path: /Users/bob/blobs

  name: crowdstrike-boshrelease
  ```

* Create the Release
  ```
  bosh create-release --final --force --tarball=crowdstrike-boshrelease.tgz
  ```

* Update the file under `runtime-config > crowstrike.yml` file and update the version number, from the output of the previous command
  ```
  version: 1
  ```

* Upload the bosh Release
  ```
  bosh upload-release crowdstrike-boshrelease.tgz
  ```

* Generate runtime config
  ```
  bosh int runtime-config/crowdstrike.yml --var=release_version=1 --var=crowdstrike_customer_id=xxxxxxxxxx > generated_runtime_config.yml
  ```

* Update the runtime config
  ```
  bosh update-runtime-config --name=crowdstrike generated_runtime_config.yml
  ```

* Trigger `Apply Changes` from Ops Manager
