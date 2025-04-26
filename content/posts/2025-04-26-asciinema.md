+++
title = "Recording terminal animations with ASCIInema"
date = "2025-04-26"

[taxonomies]
tags=["asciinema", "terminal", "fish"]
+++

_In the [previous article](@/posts/2025-04-20.md) I was using terminal animations to illustrate examples of using the 'jj' command line tool. The need to figure out how to do that delayed the article. But now I know how to do it and I can share this knowledge._

## Preamble

1. ASCIInema is not the only tool for recording terminal sessions, but it is the one which worked for me reliably.
2. I use Fish shell on macOS, but my solution should not be difficult to adapt to other shells.

## Recording

Official ["Getting Started" page](https://docs.asciinema.org/getting-started/) of ASCIInema has all the details about installing on different systems. In my case, I used Homebrew to install it:

```sh
brew install asciinema
```

Then, I added the following functions to my `~/.config/fish/config.fish` file:

```fish
# Asciinema recording function with custom dimensions
function arecc
    # Arguments: $argv[1] = columns, $argv[2] = rows

    # Get current date/time for filename
    set -l datetime (date +"%Y%m%d_%H%M%S")
    set -l filepath ~/Movies/asciinema_$datetime.cast

    # Launch fish with autosuggestions disabled and custom alias
    # Note the escaped quotes for the alias
    set -l init_commands "set -g fish_autosuggestion_enabled 0; alias jj=\"jj --no-pager\""

    # Launch asciinema with fish using the commands
    asciinema rec --cols $argv[1] --rows $argv[2] --command "fish -C '$init_commands'" $filepath

    echo "Recording saved to $filepath"
end

# Asciinema recording function with default dimensions (80×24)
function arec
    arecc 80 24
end
```

What's going on here:

- The `arecc` function takes two arguments: the number of columns and rows for the terminal window and `arec` is a wrapper for it with default values of 80 and 24 which I used for all of my videos
- All of the recordings are saved in `~/Movies` directory with a timestamp in the filename. They looked like `Movies/asciinema_20250420_021157.cast`
- I used a subshell with custom initialization commands:
  - `set -g fish_autosuggestion_enabled 0` disables the autosuggestions in the terminal. otherwise Fish would pollute the output with suggestions which are relevant to my environment, but not useful for the video
  - `alias jj="jj --no-pager"` forces jujutsu not to use pager for `jj status` and other commands. I wanted to show the linear flow and pager would redraw the screen and show only output of the single command.

That's pretty much it.

To start recording, I just run `arec` in the terminal and then execute several commands I want to record. When I am done, I press `Ctrl+D` to stop the recording.

## Using in article

The files are saved in a compact format which is trivially parsed and displayed by [asciinema-player](https://github.com/asciinema/asciinema-player) js library.

So, I just needed to:

Put asciinema-player.min.js, asciinema-player.css and and my recorder cast-sessions into the `static` directory of my zola-powered blog.

I added the following lines to the `header.html` partial of the template:

```html
<link rel="stylesheet" type="text/css" href="/asciinema-player.css" />
<script src="/asciinema-player.min.js"></script>
```

Finally, I added the blocks like this to the markdown-source of the article:

```html
<div id="ascii-jj-describe"></div>
<script>
  AsciinemaPlayer.create(
    "/jujutsu-different-approach/jj-describe.cast",
    document.getElementById("ascii-jj-describe"),
    {
      rows: 5,
      idleTimeLimit: 0.2,
    },
  );
</script>
```

I create a `<div>` container and connect js-player to it, passing some settings.

- `rows` limits the height of the player to 5 lines (which is enough for this animation)
- `idleTimeLimit` is set to 0.2 seconds to fast-forward thru the pauses in the recording. I am not the fastest person in the world and this setting makes sure that you do not suffer from this :)
