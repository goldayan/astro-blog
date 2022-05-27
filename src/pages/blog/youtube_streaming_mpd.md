---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Play youtube video using mpd
description: mpd + youtube-dl combo
date: 2021-06-28
author: "Gold Ayan"
draft: true
category: "Coding"
tags:
  - mpd
---

What is MPD?
MPD is Music player Demon

<!-- more -->

mpc is the client for Mpd to update musics.

i recently come across this article

https://forums.freebsd.org/threads/listen-to-music-on-youtube-with-the-music-player-daemon.75289/

## Pinch Script

```sh
#!/bin/sh

# Youtube audio player

# script usage
usage()
{
echo "\
$(basename "$0") -i url"
exit 2
}
shopt -s expand_aliases

# error messages
INVALID_OPT_ERR='Invalid option:'
REQ_ARG_ERR='requires an argument'
WRONG_ARGS_ERR='wrong number of arguments passed to script'

# if script is run arguments pass and check the options with getopts,
# else display script usage and exit
[ $# -gt 0 ] || usage "${WRONG_ARGS_ERR}"

# getopts check and validate options
while getopts ':i:h' opt
do
  case ${opt} in
     i) input="${OPTARG}";;
     h) usage;;
     \?) usage "${INVALID_OPT_ERR} ${OPTARG}" 1>&2;;
     :) usage "${INVALID_OPT_ERR} ${OPTARG} ${REQ_ARG_ERR}" 1>&2;;
  esac
done
shift $((OPTIND-1))

# get the audio url and title
url=$(youtube-dl --no-check-certificate --no-playlist -e -f bestaudio -g "${input}")
link=$(echo "${url}" | awk -F' ' '/https:/ {print $1}') # audio url
name=$(echo "${url}" | awk -F'https:' '{print $1}') # name of song

# yt function
yt () {
    if [ -z "$(mpc current)" ]; then # nothing playing
        if [ -z "$(mpc playlist)" ]; then # playlist empty
         mpc add "${link}"
         mpc play
         notify-send "Now Playing ♫" "${name}"
      else # playlist not empty so clear add url and play
         mpc clear
         mpc add "${link}"
         mpc play
         notify-send "Now Playing ♫" "${name}"
      fi
    else # audio playing insert url and play next
      mpc insert "${link}"
      mpc next
      notify-send "Now Playing ♫" "${name}"
    fi
}

# run the yt function with the url
yt "${url}"

```

What the script does is ?

get the audio url of using youtube-dl
adding to the mpd

The beauty of the shell script we can expand to our needs

I have a bunch of youtube url which need to be played sequentially
the file contains the url each line

get each line and add it to the mpd which would be interesting
