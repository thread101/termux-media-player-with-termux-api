#! /data/data/com.termux/files/usr/bin/env bash

RED="\e[31;1m"
GREEN="\e[32;1m"
BLUE="\e[34;1m"
YELLOW="\e[33;1m"
RESET="\e[0m"

MPATH="/storage/B057-0E1D"

function get-stream() {
  termux-media-scan -r -v $MPATH \
  | grep -E ".mp3$"
}

function set-stream() {
  stream=""
  local search=`echo "$MPATH/" | sed 's/\//\\\\\//g'`
  while read track; do
    stream+="`echo $track | tr , \& | sed -e \"s/$search//\"`,"
  done < <(get-stream)
}

function notification() {
  termux-notification -t "media player" \
    --content $text \
    --id 200 \
    --alert-once \
    --on-delete "termux-media-player stop" \
    --button1 "pause" \
    --button1-action "termux-media-player pause && termux-media-player info | termux-toast -b black -g bottom" \
    --button2 "play" \
    --button2-action "termux-media-player play && termux-media-player info | termux-toast -b black -g bottom" \
    --button3 "select" \
    --button3-action "bash `realpath $0` && termux-media-player info | termux-toast -b black -g top" 
}

function main() {
  printf "$YELLOW [*] Scanning stream ...$RESET\n"
  termux-toast -b black -g bottom "Scanning stream hang on ..."
  set-stream
  choice=`termux-dialog spinner \
    -t "Choose your track" \
    -v "$stream"`

  code=`echo $choice | jq '.code'`
  text=`echo $choice | jq '.text' | tr -d \"`
  index=`echo $choice | jq '.index'`
  ((code==-2)) && printf "$RED [!] No track selected $RESET\n" >&2 && return

  local i=0
  while read track; do
    if ((i!=index)); then
      ((i++))
      continue
    fi
    termux-media-player play "$track" >/dev/null
    printf "$GREEN [*] Playing: $track $RESET\n"
    break
  done < <(get-stream)
  notification
}

main
