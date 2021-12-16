## Data Wrangling

- Overview

  - `less` gives us a “pager” that allows us to scroll up and down through the long output. 
  - `sed` is a “stream editor” that builds on top of the old `ed` editor. 

- Regular expressions

  - surrounded by `/`. 
  - Very common patterns are:
    - `.` means “any single character” except newline
    - `*` zero or more of the preceding match
    - `+` one or more of the preceding match
    - `[abc]` any one character of `a`, `b`, and `c`
    - `(RX1|RX2)` either something that matches `RX1` or `RX2`
    - `^` the start of the line
    - `$` the end of the line
  - https://regex101.com/
  - https://www.regular-expressions.info/

- Back to data wrangling

  - `uniq -c` will collapse consecutive lines that are the same into a single line
  - `sort` will, well, sort its input
  -  `head` / `tail`.

- awk – another editor

  - `awk` is a programming language that just happens to be really good at processing text streams.

  - ```shell
     | awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
    ```

  - ```shell
    BEGIN { rows = 0 }
    $1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
    END { print rows }
    ```

  - [Idiomatic awk](https://backreference.org/2010/02/10/idiomatic-awk/)

- Analyzing data

  - `bc` - a calculator 
  - R is another (weird) programming language that’s great at data analysis and [plotting](https://ggplot2.tidyverse.org/). 

- Data wrangling to make arguments

  - `xargs` 

  - ```shell
    rustup toolchain list | grep nightly | grep -vE "nightly-x86" | sed 's/-x86.*//' | xargs rustup toolchain uninstall
    ```

- Wrangling binary data

  - use ffmpeg to capture an image from our camera, convert it to grayscale, compress it, send it to a remote machine over SSH, decompress it there, make a copy, and then display it.

  - ```shell
    ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 -
     | convert - -colorspace gray -
     | gzip
     | ssh mymachine 'gzip -d | tee copy.jpg | env DISPLAY=:0 feh -'
    ```

- [ ] Take this [short interactive regex tutorial](https://regexone.com/).
- [ ] For JSON data, try [`jq`](https://stedolan.github.io/jq/).
