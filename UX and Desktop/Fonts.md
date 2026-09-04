# Fonts

## Font Rendering Configuration

### 1. Freetype2 Symlinks
Create symlinks for good-looking rendering defaults:
    ln -s /etc/fonts/conf.avail/11-lcdfilter-default.conf /etc/fonts/conf.d
    ln -s /etc/fonts/conf.avail/10-hinting-slight.conf /etc/fonts/conf.d

### 2. Fontconfig: /etc/fonts/local.conf

Default font families:
* serif → Heuristica
* sans-serif → Noto Sans, Noto Sans CJK SC
* monospace → Liberation Mono, Noto Sans Mono CJK SC
* fantasy → Signika
* cursive → TeX Gyre Chorus

Microsoft font substitutions:
* Arial → Liberation Sans
* Arial Narrow → Liberation Sans Narrow
* Book Antiqua → TeX Gyre Bonum
* Calibri → Carlito
* Cambria → Caladea
* Comic Sans MS → Signika
* Consolas → Droid Sans Mono Slashed
* Constantia → Merriweather
* Corbel → Merriweather Sans
* Courier New → Courier Prime
* Geneva → Noto Sans
* Georgia → Gelasio
* Helvetica → Liberation Sans
* Helvetica Narrow → Liberation Sans Narrow
* Helvetica Neue → Open Sans
* Impact → Oswald
* Lucida Console → Droid Sans Mono
* Lucida Grande → Droid Sans
* Lucida Sans → Droid Sans
* Palatino Linotype → TeX Gyre Pagella
* Segoe UI → WeblySleek UI
* Symbol → Symbola
* Tahoma → DejaVu Sans Condensed
* Times New Roman → Liberation Serif
* Trebuchet MS → Ubuntu
* Verdana → DejaVu Sans
* Wingdings → Symbola

(Full fontconfig XML in original Font Rendering.md)

### 3. Install Distro Fonts
    zypper in google-noto-fonts
    zypper in texlive-tex-gyre-fonts

### 4. Required Google/Open Font Packages
* Caladea (ttf-caladea)
* Carlito (ttf-carlito)
* DejaVu (ttf-dejavu)
* Impallari Cantora (aur/ttf-impallari-cantora)
* Liberation (ttf-liberation)
* Noto (noto-fonts)
* Open Sans (ttf-opensans)
* Overpass (otf-overpass)
* Roboto (ttf-roboto)
* TeX Gyre (tex-gyre-fonts)
* Ubuntu (ttf-ubuntu-font-family)
* Courier Prime (aur/ttf-courier-prime)
* Gelasio (aur/ttf-gelasio-ib)
* Merriweather (aur/ttf-merriweather)
* Source Sans Pro (aur/ttf-source-sans-pro-ibx)
* Signika (aur/ttf-signika)

### 5. Envision Settings
[Reddit](https://www.reddit.com/r/linux/comments/1bh1x80/tweaks_for_the_freetype_font_rendering/) / [GitHub](https://github.com/maximilionus/freetype-envision)

## Open Source Font Stack (Curated)
* Atkinson Hyperlegible (accessibility)
* Fira
* GNU Unifont
* Google Noto (universal coverage)
* Hack Pro
* IBM Plex
* Intel One Mono
* Inter
* JetBrains Mono
* Microsoft Cascadia Code
* Ubuntu

## Infinality Font Rendering
Note: The original Infinality patches are long-dead and incompatible with modern freetype2. Infinality Remix (by pdeljanov) attempted to revive them but appears to have stalled as of 2023. The modern approach is to use freetype2's built-in subpixel rendering with the Envision settings (see above) rather than Infinality patches. Consider Infinality Remix abandoned unless development resumes.
* https://github.com/pdeljanov/infinality-remix/issues/13
* https://github.com/pdeljanov/infinality-remix

## Reference Links
* https://news.ycombinator.com/item?id=30705078
* https://gist.github.com/cryzed/e002e7057435f02cc7894b9e748c5671
* https://wiki.archlinux.org/title/Font_configuration


---

## From legacy notes: Font Rendering.md
1. Create the following symlinks using root to instruct freetype2 to use good-looking rendering defaults:
2. Modify (or create) `/etc/fonts/local.conf`
		<?xml version="1.0"?>
		<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
		<!-- /etc/fonts/local.conf file for local customizations -->
		<fontconfig>
		  <!-- Replacements from http://bohoomil.com/doc/05-fonts/ (until ibfonts-meta-extended) -->
		    <family>serif</family>
		    <prefer><family>Heuristica</family></prefer>
		  </alias>
		    <family>sans-serif</family>
		      <prefer>
		        <family>Noto Sans</family>
		        <family>Noto Sans CJK SC</family>
		      </prefer>
		  </alias>
		    <family>monospace</family>
		    <prefer>
		      <family>Liberation Mono</family>
		      <family>Noto Sans Mono CJK SC</family>
		    </prefer>
		  </alias>
		    <family>fantasy</family>
		    <prefer><family>Signika</family></prefer>
		  </alias>
		    <family>cursive</family>
		    <prefer><family>TeX Gyre Chorus</family></prefer>
		  </alias>
		    <test name="family"><string>Arial</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Liberation Sans</string>
		  </match>
		    <test name="family"><string>Arial Narrow</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Liberation Sans Narrow</string>
		  </match>
		    <test name="family"><string>Book Antiqua</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>TeX Gyre Bonum</string>
		  </match>
		    <test name="family"><string>Calibri</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Carlito</string>
		  </match>
		    <test name="family"><string>Cambria</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Caladea</string>
		  </match>
		    <test name="family"><string>New Century Schoolbook</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>TeX Gyre Schola</string>
		  </match>
		    <test name="family"><string>Comic Sans MS</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Signika</string>
		  </match>
		    <test name="family"><string>Consolas</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Droid Sans Mono Slashed</string>
		  </match>
		    <test name="family"><string>Constantia</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Merriweather</string>
		  </match>
		    <test name="family"><string>Corberl</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Merriweather Sans</string>
		  </match>
		    <test name="family"><string>Courier New</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Courier Prime</string>
		  </match>
		    <test name="family"><string>Geneva</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Noto Sans</string>
		  </match>
		    <test name="family"><string>Georgia</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Gelasio</string>
		  </match>
		    <test name="family"><string>Helvetica</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Liberation Sans</string>
		  </match>
		    <test name="family"><string>Helvetica Narrow</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Liberation Sans Narrow</string>
		  </match>
		    <test name="family"><string>Helvetica Neue</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Open Sans</string>
		  </match>
		    <test name="family"><string>Impact</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Oswald</string>
		  </match>
		    <test name="family"><string>ITC Zapf Chancery</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>TeX Gyre Chorus</string>
		  </match>
		    <test name="family"><string>Lucida Calligraphy</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Quintessential</string>
		  </match>
		    <test name="family"><string>Lucida Handwriting</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Quintessential</string>
		  </match>
		    <test name="family"><string>Lucida Casual</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>CantoraOne</string>
		  </match>
		    <test name="family"><string>Lucida Console</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Droid Sans Mono</string>
		  </match>
		    <test name="family"><string>Lucida Sans Typewriter</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Liberation Sans Mono</string>
		  </match>
		    <test name="family"><string>Lucida Fax</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Luxi Mono</string>
		  </match>
		    <test name="family"><string>Lucida Sans</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Droid Sans</string>
		  </match>
		    <test name="family"><string>Lucida Grande</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Droid Sans</string>
		  </match>
		    <test name="family"><string>Palatino Linotype</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>TeX Gyre Pagella</string>
		  </match>
		    <test name="family"><string>SegoeUI</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>WeblySleek UI</string>
		  </match>
		    <test name="family"><string>Symbol</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Symbola</string>
		  </match>
		    <test name="family"><string>Tahoma</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>DejaVu Sans Condensed</string>
		  </match>
		    <test name="family"><string>Times New Roman</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Liberation Serif</string>
		  </match>
		    <test name="family"><string>Trebuchet MS</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Ubuntu</string>
		  </match>
		    <test name="family"><string>Verdana</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>DejaVu Sans</string>
		  </match>
		    <test name="family"><string>Wingdings</string></test>
		    <edit name="family" mode="assign" binding="strong">
		      <string>Symbola</string>
		  </match>
		</fontconfig>
3. Install Distro Fonts
4. Google Fonts:
- Caladea (`ttf-caladea`)
- Carlito (`ttf-carlito`)
- DejaVu (`ttf-dejavu`)
- Impallari Cantora (`aur/ttf-impallari-cantora`)
- Liberation (`ttf-liberation`)
- Noto (`noto-fonts`)
- Open Sans (`ttf-opensans`)
- Overpass (`otf-overpass`)
- Roboto (`ttf-roboto`)
- TeX Gyre (`tex-gyre-fonts`)
- Ubuntu (`ttf-ubuntu-font-family`)
- Courier Prime (`aur/ttf-courier-prime`)
- Gelasio (`aur/ttf-gelasio-ib`)
- Merriweather (`aur/ttf-merriweather`)
- Source Sans Pro (`aur/ttf-source-sans-pro-ibx`)
- Signika (`aur/ttf-signika`)
2. [Install Envision Settings](https://www.reddit.com/r/linux/comments/1bh1x80/tweaks_for_the_freetype_font_rendering/) [Github](https://github.com/maximilionus/freetype-envision)


---

## From legacy notes: Open Fonts.md
Atkinson Hyperlegible
GNU Unifont
Google Noto
Intel’s One Mono
JetBrains Mono
Microsoft Cascadia Code
NebulaSans