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
brew install cmark-gfm entr ucspi-tcp
```

- `cmark-gfm` is used for rendering Markdown. `cmark-gfm` is GitHub's fork of
  `cmark`, which I use for extensions like tables. It would be easy to swap it
  out for `cmark` or even Pandoc.
- `entr` is used to watch files for changes.
- `ucspi-tcp` is used to serve the HTML and the [server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)
  endpoint that is used for live-reloading. I wanted to just use `netcat`, but it
  turns out (1) netcat is not actually very portable and (2) handling multiple
  requests quickly gets you into even more esoteric shell scripting than the
  script currently involves.

# To do

- Allow specifying the port!
- Make this work on platforms other than macOS.
- See if this can be extended in a clean way to support serving entire
  directories, like with indoor-wiki.
- Get rid of the annoying `zsh terminated` message.
- Save KaTeX locally to support offline use. 
- Handle path names with spaces.
- Support GitHub-style [alert blocks](https://github.com/orgs/community/discussions/16925)?

# Prior art

- [livemd](https://github.com/chrboe/livemd) (2018). Engineered in an arguably
  saner way (Go program as opposed to shell script). No live-reloading, though.
- [indoor-wiki](https://github.com/jyc/indoor-wiki) (2016). By yours truly.
  Much more complicated: for browsing files of Markdown folders. No
  live-reloading. Also depends on OCaml (but avoids shell...).

# FAQ

## Why don't Fira and Iosevka render in Safari?

Safari decided to [totally disable display of user-installed fonts](https://stackoverflow.com/a/63208227).
