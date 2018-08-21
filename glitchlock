#!/bin/bash
# ┏━┓┳  o┏┓┓┏━┓┳ ┳┳  ┏━┓┏━┓┳┏ 
# ┃ ┳┃  ┃ ┃ ┃  ┃━┫┃  ┃ ┃┃  ┣┻┓
# ┇━┛┇━┛┇ ┇ ┗━┛┇ ┻┇━┛┛━┛┗━┛┇ ┛
#
# author: xero <x@xero.nu> http://xero.nu
# requires: i3lock-color, imagemagick, scrot

scrot /tmp/lock.png
convert /tmp/lock.png /tmp/lock.jpg
file=/tmp/lock.jpg

function datamosh() {
	fileSize=$(wc -c < "$file")
	headerSize=1000
	skip=$(shuf -i "$headerSize"-"$fileSize" -n 1)
	count=$(shuf -i 1-10 -n 1)
	for i in $(seq 1 $count);do byteStr=$byteStr'\x'$(shuf -i 0-255 -n 1); done;   
	printf $byteStr | dd of="$file" bs=1 seek=$skip count=$count conv=notrunc >/dev/null 2>&1
}

steps=$(shuf -i 40-70 -n 1)
for i in $(seq 1 $steps);do datamosh "$file"; done

GLITCHICON=${GLITCHICON:=""}
PARAM=(--bar-indicator --bar-position h --bar-direction 1 --redraw-thread -t "" \
	--bar-step 50 --bar-width 250 --bar-base-width 50 --bar-max-height 100 --bar-periodic-step 50 \
	--bar-color 00000077 --keyhlcolor 00666666 --ringvercolor cc87875f --wrongcolor ffff0000 \
	--veriftext="" --wrongtext="" --noinputtext="" )

LOCK=()
while read LINE
do
	if [[ "$LINE" =~ ([0-9]+)x([0-9]+)\+([0-9]+)\+([0-9]+) ]]; then
		W=${BASH_REMATCH[1]}
		H=${BASH_REMATCH[2]}
		Xoff=${BASH_REMATCH[3]}
		Yoff=${BASH_REMATCH[4]}
		if [ ! -z "$GLITCHICON" ]; then
			IW=`identify -ping -format '%w' $GLITCHICON`
			IH=`identify -ping -format '%h' $GLITCHICON`
			MIDXi=$(($W / 2 + $Xoff - $IW / 2))
			MIDYi=$(($H / 2 + $Yoff - $IH / 2))
			LOCK+=($GLITCHICON -geometry +$MIDXi+$MIDYi -composite)
		fi
	fi
done <<<"$(xrandr)"

convert /tmp/lock.jpg /tmp/lock.png >/dev/null 2>&1
rm /tmp/lock.jpg
file=/tmp/lock.png

convert "$file" "${LOCK[@]}" "$file"

i3lock -n "${PARAM[@]}" -i "$file" > /dev/null 2>&1
