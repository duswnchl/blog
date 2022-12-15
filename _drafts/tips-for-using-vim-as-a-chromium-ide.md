---
layout: post
title: Tips for using Vim as a chromium IDE
category:
- Chromium
- Tips
tags:
- chromium
- vim
- igalia-planet
---
There are lots of powerful IDEs which are broadly used by developers. Vim is a
text editor, but it can be turned out to be a good IDE with awewome plugins and
a little bit of configuration.

Many people prefer using Vim to develop software because of its lightness and
availability. I am one of them, and always use Vim to develop Chromium. However,
someone would think that Vim is hard to get to reach the same performance as
other IDE, since Chromium is a very huge project.

For those and potential users, this post introduces some tips for using Vim as a
Chromium IDE.

> The context of this document is for Linux users who are used to using vim.
{: .prompt-info }

## Code Formatting

I strongly recommand to use [vim-codefmt][3] for code formatting. It supports
most file types in Chromium including GN (See [GN docs][4]). I don't like to use
autoformat and just bind `:FormatLines` to `ctrl-I` for formating a visual
block.

![codefmt]({% asset_path codefmt.gif %})

## tools/vim

You can easily find a bunch of vim files in [tools/vim][1]. I create a *.vimrc*
file locally in a working directory and load only what I need.

```sh
let chtool_path=getcwd().'/tools'

filetype off
let &rtp.=','.chtool_path.'/vim/mojom'
exec 'source' chtool_path.'/vim/filetypes.vim'
exec 'source' chtool_path.'/vim/ninja-build.vim'
filetype plugin indent on
```

Chromium provides vimscript files for syntax highliting and file detection of
Mojom which is the IDL for Mojo interfaces(IPC) among Chromium services.

And `ninja-build.vim` allows you compile a file by `ctrl-O` or build the
specific target by `:CrBuild` command. See each vim files for details.

### YouCompleteMe

`tools/vim` has the example configuration for [YouCompleteMe][8] (a code
completion engine for Vim). I also add follow lines to in a local *.vimrc*.
(See `tools/vim/chromium.ycm_extra_conf.py`).

```sh
let g:ycm_extra_conf_globlist = ['../.ycm_extra_conf.py']
let g:ycm_goto_buffer_command = 'split-or-existing-window'
```

As you already know, YouCompleteMe requires clangd. Very fortunatley, Chromium
already supports clangd and remote index server to get daily index snapshots.

Do not skip documents about [using Clangd to build chromium][2] and [chromium
remote index server][9].

Here are short demos for completion suggestions,
![ycm_auto]({% asset_path ycm_auto.gif %})
and code jumping(`:YcmCompleter GoTo`, `:YcmCompleter GoToReferences`) during
Chromium development.
![ycm_jump]({% asset_path ycm_jump.gif %})

## Commenter

A good developer writes good comments, so a good commenter makes you a good
developer (joke). In my humble opinion, [NERD commenter][5] will make you a good
developer (joke again). You can get all details from docs of NERD commenter and
Chromium needs only an additional option for mojo files.

```sh
let g:NERDCustomDelimiters = { 'mojom': {'left': '//'} }
```

## File navigation

Chromium is really huge, so we benefit from efficient file navigtion tools.
There are lots of awesome fuzzy finder plugins for this purpose and I use
[command-t][6] for a long time. But command-t requires a vim executable with
ruby/lua support or Neovim. If you need simpler and still powerful one,
[fzf.vim][7] is an altenative great option.

Since Chromium is the git project, search commands based by `git ls-files` will
provide results very quickly. Use the option `let g:CommandTFileScanner = 'git'`
for `command-t`, and use the command `:GFiles` for `fzf.vim`.

![filenav]({% asset_path filenav.gif %})

## Conclusion

Thanks for reading and I hope this blog post can be any help to your development
enviroment for Chromium. Any comment for sharing some other cool tips always be
welcomed, so that we can make our vim more productive.

.... I will be back someday with debugging Chromium in Vim.
![sneak peek]({% asset_path back.png %})

[1]: https://source.chromium.org/chromium/chromium/src/+/main:tools/vim/?q=tools%2Fvim&ss=chromium
[2]: https://chromium.googlesource.com/chromium/src.git/+/HEAD/docs/clangd.md
[3]: https://github.com/google/vim-codefmt
[4]: https://source.chromium.org/gn/gn/+/main:misc/vim/README.md
[5]: https://github.com/preservim/nerdcommenter
[6]: https://github.com/wincent/command-t
[7]: https://github.com/junegunn/fzf
[8]: https://github.com/ycm-core/YouCompleteMe
[9]: https://linux.clangd-index.chromium.org/
