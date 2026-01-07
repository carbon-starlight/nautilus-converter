# Convert files directly from Files

This script is a particial implementation of a [feature request](https://discourse.gnome.org/t/feature-request-add-an-option-to-quickly-convert-formats-right-from-files/) to make file conversion built-in and avalible in right-click context menu of GNOME Files, a.k.a. Nautilus. 

VIDEO HERE

> [!IMPORTANT]
> You need the following dependences: `ffmpeg` to convert <ins>images, audio and video</ins>, `iconv` to convert <ins>text</ins> between encodings, `libreoffice` to convert <ins>documents</ins>, `ctmconv` to convert 3D models and `fontforge` to convert <ins>fonts</ins>.

While not a full substitude for this function in terms of convenience *(I would like conversion to be availble with formats listed right in menu, not Zenity dialog, and for there to be no need to install additional dependences via terminal)* it allows for conversion in a not much less inconvenient manner.

There were several attempts to make such a converter, [one](https://github.com/hexplor/nautilus-imageconverter) exclusively for images, another [one](https://github.com/tongphe/batch-convert-documents) — for one-way conversion of documents to PDF or HTML, and [one more](https://codeberg.org/Lich-Corals/linux-file-converter-addon), that I became aware of after finishing my project, for images, audio and video. But never — for the amount of formats I attempted to cover.

This project was mostly written in [micro](https://micro-editor.github.io/), with finishing touches (when working with long horizontal lines) done in VSCodium. LLM usage was pretty limited, with them mostly supplying short code snippets on how to do x with variable y with no overall control over production. When I opened VSC, one model was set to suggest completions of code, but I deliberately ignored accepting them (often choosing to retype) until commit 376ba32.

## Installation

You'll need to paste the file `Convert…` to `~/.local/share/nautilus/scripts/` directory and make it executable. A command to do that from the termimal:

```bash
curl -L -o ~/.local/share/nautilus/scripts/Convert… https://raw.githubusercontent.com/carbon-starlight/nautilus-converter/HEAD/Convert…
chmod +x "~/.local/share/nautilus/scripts/Convert…"
```

## Copying

I decided to use the GPLv3 license utilized by Nautilus, except the more universal -or-later version, but no code from Nautilus was borowed. If you think a permissive license would be a better fit, file and issue with your reasoning and I will consider it.
