# yt-dlp Docker Image

A Docker image for running [yt-dlp](https://github.com/yt-dlp/yt-dlp), available in two variants. Both include Python 3 and FFmpeg.

- **Alpine** (`Dockerfile.alpine`): smaller base image; runs as a non-root user and has no default URL.
- **Ubuntu** (`Dockerfile.yt-dlp`): Ubuntu 24.04 base image; runs as root and uses a sample URL when no arguments are provided.

## Requirements

- Docker

## Build an Image

Build either variant from the project directory:

```sh
docker build -f Dockerfile.alpine -t yt-dlp:alpine .
docker build -f Dockerfile.yt-dlp -t yt-dlp:ubuntu .
```

Each build downloads the latest yt-dlp release. Rebuild an image to pick up a newer release.

## Run a Variant

Mount your current directory at `/mydir` to save downloads on your host. Choose the image tag for the variant you built:

```sh
docker run --rm -v "$(pwd):/mydir" yt-dlp:alpine "<YOUTUBE_URL>"
docker run --rm -v "$(pwd):/mydir" yt-dlp:ubuntu "<YOUTUBE_URL>"
```

Arguments after the image name are passed directly to yt-dlp. For example, extract audio as MP3 with either variant:

```sh
docker run --rm -v "$(pwd):/mydir" yt-dlp:alpine -x --audio-format mp3 "<YOUTUBE_URL>"
```

Without a mounted directory, downloads remain inside the container. With `--rm`, the container and its downloads are removed when yt-dlp exits. Provide a URL when running the Alpine image. The Ubuntu image uses its configured sample URL if you omit arguments.

## Docker Hub

To publish both variants, tag and push them separately:

```sh
docker tag yt-dlp:alpine daviddean181/youtube-dl:alpine
docker push daviddean181/youtube-dl:alpine

docker tag yt-dlp:ubuntu daviddean181/youtube-dl:ubuntu
docker push daviddean181/youtube-dl:ubuntu
```

After the tags have been pushed, pull and run the variant you want. Mount the current directory to keep downloads on the host:

```sh
docker pull daviddean181/youtube-dl:alpine
docker run --rm -v "$(pwd):/mydir" daviddean181/youtube-dl:alpine "<YOUTUBE_URL>"
```

Use the `:ubuntu` tag instead for the Ubuntu variant. The untagged `daviddean181/youtube-dl` image refers to the repository's `latest` tag, which is independent of the `:alpine` and `:ubuntu` tags.

