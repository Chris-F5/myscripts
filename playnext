#!/bin/bash

increment=1
peek=1
mpv=1
dir=.
positional_arguments=0
for opt in "$@"
do
  case "$opt" in
  -r|--replay)
    echo "r $opt"
    increment=0
    peek=0
    ;;
  -p|--peek)
    increment=0
    peek=1
    ;;
  -e|--echo)
    mpv=0
    ;;
  -*)
    echo "playnext: invalid flag '$opt'" >&2
    exit 1
    ;;
  *)
    positional_arguments=$(($positional_arguments+1))
    if ((positional_arguments = 1)); then
      dir=$opt
    else
      echo "playnext: only accepts one positional argument." >&2
    fi
    ;;
  esac
done
cd $dir || exit 1
if [ -f .playnext ]; then
  num=$(<.playnext)
else
  num=0
fi
number_re="^[0-9]+$"
if ! [[ $num =~ $number_re ]]; then
  echo "playnext: .playnext file invalid." >&2
  exit 1
fi
media_file=$(find . -type f | sort | grep -e ".mkv$" -e ".mp4$" -e ".mp3$" -e ".flv$" |
  sed "$(($num + $peek))q;d")
if [ -z "$media_file" ]; then
  echo "playnext: FINISHED" >&2
  exit 1
fi
num=$(($num + $increment))
echo "$num" > .playnext
echo "$media_file"
if (( mpv == 1 )); then
  mpv "$media_file"
fi
