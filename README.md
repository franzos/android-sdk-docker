# Android SDK on Docker (for React Native)

This is a Docker image for building Android applications on CI services specifically for React Native. The Dockerfile has been adapted from https://github.com/MobileDevOps/android-sdk-image to be more flexible and provide the right JDK and SDK.

The initial version has been tested with:

## Releases

- `franzos/android-sdk:latest` - Latest version of the image
- `franzos/android-sdk:node16-jdk11-sdk28-v0.1` - First version of the image (`v0.1`)
  - Node `v16.20.0`
  - JDK `11.0.18 2023-01-17`
  - Android SDK `v28.0.3`
  - tested with React Native `v0.65`

_Do not use `latest` unless you know what you are doing._

## Usage

(1) Install your project dependencies (`npm install`) before running the container

(2) For ease, add a `docker-compose.yml` to your RN project:

```yaml
version: '3.6'

services:
  builder:
    image: franzos/android-sdk:node16-jdk11-sdk28-v0.1
    command: tail -f /dev/null
    volumes:
      - .:/usr/src/app
```

(3) Start the container and connect to it.

```bash
docker compose -up
docker exec -it rna-builder bash
```

If you prefer to run the container in the background, do `docker compose -up -d`.

### Shortcuts

Build debug APK

```bash
docker exec -it rna-builder build-debug-apk
```

This will generate a debug APK at:

```
android/app/build/outputs/apk/debug/app-debug.apk
```

You can install it to your device with:

```bash
adb install app-debug.apk
```

## TODO

- [ ] Support newer Android SDK's
- [ ] Support newer RN versions
- [ ] Automate build process for signed APK's

## Release

```bash
docker build -t rna-builder .
docker tag rna-builder franzos/android-sdk:node16-jdk11-sdk28-v0.1
docker push franzos/android-sdk:node16-jdk11-sdk28-v0.1
```

On Linux hosts without iptables:

```bash
docker build --network=host -t rna-builder .
```