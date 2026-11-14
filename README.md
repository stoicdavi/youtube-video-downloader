# yt-dlp Docker Image

A small Ubuntu-based Docker image for running [yt-dlp](https://github.com/yt-dlp/yt-dlp). The image includes Python 3 and FFmpeg, so yt-dlp can download media and use FFmpeg for post-processing.

## Requirements

- Docker

## Use the Docker Hub Image

Pull the published image:

```sh
docker pull daviddean181/youtube-dl
```

To run yt-dlp without saving the download to your host, pass a YouTube URL:

```sh
docker run --rm daviddean181/youtube-dl "<YOUTUBE_URL>"
```

The download is stored inside the container. `--rm` removes the container when yt-dlp exits, so the download is not retained. To save it in your current host directory, mount that directory at `/mydir`:

```sh
docker run --rm -v "$(pwd):/mydir" daviddean181/youtube-dl "<YOUTUBE_URL>"
```

## Build Locally

From the project directory, build the image:

```sh
docker build -f Dockerfile.yt-dlp -t yt-dlp .
```

The Dockerfile downloads the latest yt-dlp release when the image is built. Rebuild the image to pick up a newer release.

## Download with the Local Image

Mount a host directory at `/mydir` so downloaded files remain available after the container exits. Run this from the directory where you want the downloads saved:

```sh
docker run --rm -v "$PWD:/mydir" yt-dlp "https://www.youtube.com/watch?v=VIDEO_ID"
```

Arguments after the image name are passed directly to yt-dlp. For example, to extract audio as MP3:

```sh
docker run --rm -v "$PWD:/mydir" yt-dlp -x --audio-format mp3 "https://www.youtube.com/watch?v=VIDEO_ID"
```

FFmpeg is included in the image for supported extraction and conversion options.

## Default Behavior

If you run the image without arguments, it uses the sample URL configured in the Dockerfile:

```sh
docker run --rm -v "$PWD:/mydir" yt-dlp
```

Providing a URL or other yt-dlp arguments replaces that default command. Downloads are written in `/mydir`; without a mounted directory, they remain inside the container and are removed when it exits with `--rm`.
