#! /bin/bash
set -e
set -u
set -o pipefail
exec > /tmp/nautilus_converter.log 2>&1
# `tail -f /tmp/nautilus_converter.log` to view logs in real time

# formats=$(file "$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS" -b --mime-type)
# notify-send frmts "$formats"

while IFS= read -r path; do
	# skip empty lines
	if [ -z "$path" ]; then
	        continue
	    fi
	
    # $path is file path
    # 1. Determine the format
    format=$(file "$path" -b --mime-type)

	# 2. Check if the processed file is one of supported type
	# CASE: IMAGE
    if [[ "$format" == image/* ]]; then
    	# Format is image. Handle with FFMPEG or Magick
		# Check if ffmpeg is installed
		if ! command -v ffmpeg >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert images, audios and videos you need FFMPEG installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
        to=$(GSK_RENDERER=gl zenity --list --title="Please, select the destination format for $(basename "$path")" --height=500 --column=Formats webp jxl png jpeg gif bmp)
        # Convert and save
        noext="${path%.*}"
        ffmpeg -i "$path" "$noext.$to"

    # CASE: VIDEO
    elif [[ "$format" == video/* ]]; then
    	# Format is video. Convert with FFMPEG.
    	# Check if ffmpeg is installed
		if ! command -v ffmpeg >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert images, audios and videos you need FFMPEG installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
        to=$(GSK_RENDERER=gl zenity --list --title="Select the destination format for $(basename "$path")" --height=500 --column=Formats webm mp4 mov mkv)
        # Convert and save
        noext="${path%.*}"
        ffmpeg -i "$path" "$noext.$to"

    # CASE: AUDIO
    elif [[ "$format" == audio/* ]]; then
    	# Format is audio. Convert with FFMPEG.
    	# Check if ffmpeg is installed
		if ! command -v ffmpeg >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert images, audios and videos you need FFMPEG installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
        to=$(GSK_RENDERER=gl zenity --list --title="Select the destination format for $(basename "$path")" --height=500 --column=Formats flac wav aac mp3 m4a)
        # Convert and save
        noext="${path%.*}"
        ffmpeg -i "$path" "$noext.$to"

    # CASE: PLAINTEXT
    elif [[ "$format" == text/plain ]]; then
    	# Format is plain text. Convert between encodings.
    	# Check if iconv is installed
		if ! command -v iconv >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert texts between encodings, you need iconv installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
		encodings=$(echo $(iconv -l) | awk -F' [0-9]' '{print substr($0, index($0,$2)-1)}' | tr ',\n/' ' ' | tr -s ' ')
        from=$(GSK_RENDERER=gl zenity --list --title="Select the source encoding for $(basename "$path") (guessed: $(file --mime-encoding -b $path | tr '[:lower:]' '[:upper:]'))" --height=500 --column=Formats $(file --mime-encoding -b $path | tr '[:lower:]' '[:upper:]') UTF8 $encodings)
        to=$(GSK_RENDERER=gl zenity --list --title="Select the destination format" --height=500 --column=Formats UTF8 $encodings)
        # Convert and save
        noext="${path%.*}"
        iconv -f "$from" -t "$to" "$noext".txt -o "$noext".txt
        
    # CASE: TEXT DOCUMENT
    elif [ "$format" == "application/vnd.oasis.opendocument.text" ] || [ "$format" == "application/vnd.openxmlformats-officedocument.wordprocessingml.document" ] || []; then
    	# Format is document. Convert with LibreOffice.
    	# Check if LibreOffice is installed
		if ! command -v libreoffice >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert documents, you need LibreOffice installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
        to=$(GSK_RENDERER=gl zenity --list --title="Select the destination format for $(basename "$path")" --height=500 --column=Formats odt docx doc pages rft html pdf)
        # Convert and save
        noext="${path%.*}"
        # libreoffice --convert-to "$to" "$path"
		output=$(libreoffice --convert-to "$to" "$path" 2>&1)
		if [ ! -f "${path%.*}.$to" ]; then
		    GSK_RENDERER=gl zenity --error \
		        --title="Conversion Failed" \
		        --text="$output"
		fi

        

   # CASE: SPREADSHEET
   elif [ "$format" == "application/vnd.oasis.opendocument.spreadsheet" ] || [ "$format" == "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" ]; then
    	# Format is spreadsheet. Convert with LibreOffice.
    	# Check if LibreOffice is installed
		if ! command -v libreoffice >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert documents, you need LibreOffice installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
        to=$(GSK_RENDERER=gl zenity --list --title="Select the destination format for $(basename "$path")" --height=500 --column=Formats ods xlsx xls numbers html pdf)
        # Convert and save
        noext="${path%.*}"
		output=$(libreoffice --convert-to "$to" "$path" 2>&1)
		if [ ! -f "${path%.*}.$to" ]; then
		    GSK_RENDERER=gl zenity --error \
		        --title="Conversion Failed" \
		        --text="$output"
		fi
		
   # CASE: PRESENTATION
   elif [ "$format" == "application/vnd.oasis.opendocument.presentation" ] || [ "$format" == "application/vnd.openxmlformats-officedocument.presentationml.presentation" ]; then
    	# Format is a presentation. Convert with LibreOffice.
    	# Check if LibreOffice is installed
		if ! command -v libreoffice >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert documents, you need LibreOffice installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
        to=$(GSK_RENDERER=gl zenity --list --title="Select the destination format for $(basename "$path")" --height=500 --column=Formats odp pptx ppt keynote html pdf)
        # Convert and save
        noext="${path%.*}"
		output=$(libreoffice --convert-to "$to" "$path" 2>&1)
		if [ ! -f "${path%.*}.$to" ]; then
		    GSK_RENDERER=gl zenity --error \
		        --title="Conversion Failed" \
		        --text="$output"
		fi
		
   # CASE: FONT
   elif [[ "$format" == font/* ]]; then
    	# Format is a font. Convert with FontForge.
    	# Check if FontForge is installed
		if ! command -v fontforge >/dev/null 2>&1; then
		    GSK_RENDERER=gl zenity --info --title="Missing dependency" --text="To convert fonts, you need FontForge installed and accessible from the command line"
		    exit 1
		fi
        # Ask user for the format they prefer
        to=$(GSK_RENDERER=gl zenity --list --title="Select the destination format for $(basename "$path")" --height=500 --column=Formats otf ttf woff woff2)
        # Convert and save
        noext="${path%.*}"
        fontforge -lang=ff -c 'Open($1); Generate($2); Close();' "$from" "$to"
       
    else
   		# Unknown format
   		RESPONSE=$(GSK_RENDERER=gl zenity --info --title="Unsupported format" --text="The file you want to convert is of unsupported format. Fill an issue to request its support" --extra-button="Report Issue")
		if [ "$RESPONSE" = "Report Issue" ]; then
		    xdg-open "https://example.com"
		fi
    fi
    
done <<< "$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS"
