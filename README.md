# Usage

```
markd path/to/file.md
```

Now `http://localhost:9999` will show `file.md` rendered to HTML with LaTeX support via [KaTeX](https://katex.org/).
When you change the input file, not only will the page automatically rebuild,
but your browser will also automatically refresh.

This isn't supposed to be a Swiss army-knife or a Porsche; there are apps for that.
I just want to edit my Markdown files in Vim and see the output in another window.
... Also if I'm being honest, I wanted to see what it would look like to do this in a shell script

The answer: not especially pretty.
But I needed fewer external dependencies than I expected.
It's not even clear to me that it would have much been better as a Python script!
And if POSIX were invented today, I believe it might have had built-in tools to replace entr (inotify is post-POSIX, after all), ucspi-tcp, and even cmark (c.f. troff)!

# Install

Install dependencies:

```
brew install cmark entr ucspi-tcp
```

- `cmark` is used for rendering Markdown.
- `entr` is used to watch files for changes.
- `ucspi-tcp` is used to serve the HTML and the [server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)
  endpoint that is used for live-reloading. I wanted to just use `netcat`, but it
  turns out (1) netcat is not actually very portable and (2) handling multiple
  requests quickly gets you into even more esoteric shell scripting than the
  script currently involves.

# To do

- Allow specifying the port!
- Make this work on platforms other than macOS.
- Get rid of the annoying `zsh terminated` message.
- Prettier default stylesheet.

# Prior art

- [livemd](https://github.com/chrboe/livemd) (2018). No live-reloading.
- [indoor-wiki](https://github.com/jyc/indoor-wiki) (2016). By yours truly.
  Much more complicated: for browsing files of Markdown folders. No
  live-reloading. Also depends on OCaml (but avoids shell...).
