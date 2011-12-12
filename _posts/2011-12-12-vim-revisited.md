---
title: "Vim: revisited"
description: "How I learned Vim the proper way."
layout: post
category: vim
styles: |
  p code.file { background: none; padding: 0; color: #666 }
  @media only screen and (min-width: 1000px) {
    body { max-width: 1300px }
    #post { width: 70% }
    h2 { clear: left }
    aside {
      width: 35%; float: right; margin-right: -43%;
      margin-top: -5em;
      font-size: .9em;
      color: #666;
    }
    #post aside { font-size: .8em }
    #post aside p { line-height: 1.4 }
    aside kbd { padding: 1px 3px }
    aside *:first-child { margin-top: 0 }
    aside h3 { border: none; padding-bottom: 0; font-variant: small-caps; line-height: 1 }
  }
  #post img { display:block; max-width: 100%; -webkit-box-shadow: rgba(0,0,0,.4) 0 2px 8px; }
  td, th { vertical-align:top; text-align:left }
  th { padding:.1em .3em .1em 0 }
  th kbd, td kbd { font-size:.8em; font-weight:normal }
  table { border-collapse: collapse; margin: 1.2em 0 1.2em 1.5em }
  @media only screen and (max-width: 480px) {
    table { margin: 1em 0 }
  }
  table thead { cursor:default }
  table.split tbody th { font-weight: normal }
  table.split th, table.split td { padding: .1em .4em }
  table.split thead tr { border-bottom: 1px solid silver }
  table.split th, table.split td { border-right: 1px solid silver }
  table.split th:last-child, table.split td:last-child { border:none }
  table.reverse th:first-child,
  table.reverse th:first-child + th { text-align: center }
  table.reverse td { border:none }
---

I've had an *off/on relationship* with Vim for the past many years.

Before, I never felt like we understood each other properly. I felt that
the kind of programming I'm doing is not easily done without plugins and
some essential settings in <code class="file">.vimrc</code>, but
fiddling with all the knobs and installing all the plugins that I
thought I needed was a process that in the end stretched out from few
hours to weeks, months even; and it the end it just caused frustration
instead of making me a happier coder.

Recently, I decided to give Vim another shot.  This time around it was
different – something in my brain switched and now for the first time in
my life I'm proud of my knowledge of Vim. My philosophy of it has changed
to "less is more", my approach was more disciplined and my motivation
stronger. And so you don't spend as much time learning as I did, I am
going to lay down some fundamentals.

## Start with a basic setup – but not zero

I learned this the hard way: you shouldn't start with a huge
<code class="file">.vimrc</code> file that you copied from someone else
nor should you install every single plugin that seems useful at that
moment.

This is a good starting point for your <code class="file">.vimrc</code>:

{% highlight vim %}
set nocompatible                " choose no compatibility with legacy vi
syntax enable
set encoding=utf-8
set showcmd                     " display incomplete commands
filetype plugin indent on       " load file type plugins + indentation

"" Whitespace
set nowrap                      " don't wrap lines
set tabstop=2 shiftwidth=2      " a tab is two spaces (or set this to 4)
set expandtab                   " use spaces, not tabs (optional)
set backspace=indent,eol,start  " backspace through everything in insert mode

"" Searching
set hlsearch                    " highlight matches
set incsearch                   " incremental searching
set ignorecase                  " searches are case insensitive...
set smartcase                   " ... unless they contain at least one capital letter
{% endhighlight %}

Everything mentioned in this article is valid in vim running in a
terminal as well as graphical (GUI) vim such as gvim or MacVim. A
graphical vim setting offers extra features, but I'm not covering any
of those here.

<div><aside>
<p><strong>Don’t use <a href="https://github.com/carlhuda/janus#readme" title="Janus: a Vim distribution">Janus</a></strong>. It’s a community maintained vim configuration project that in theory sounds nice, but once you start using it it’s not all rainbows. The current version of Janus installs harmful plugins such as Syntastic and adds tons of opinionated mappings and piles of hacks on top of one another. The “experimental” version of Janus is a rewrite that’s meant to be more configurable, but in fact it’s just more fragmented and harder to follow what’s going on. You should be in charge of your <code class="file">.vimrc</code> and always know what’s going on, but with Janus you’re not.</p>
</aside></div>

### Guidelines for expanding your vim settings later on:

1. Never copy something from other people unless you know *exactly* what
   that particular setting is doing and you recognize that you've needed
   it before.
2. Don't use too many plugins; start with 3-5 and add others as your
   skill progresses. Use [Pathogen][] for managing them.
3. Overly eager plugins like [Syntastic][] can actually *hurt* your
   development even when you're not actively using them. Zap such
   offenders quickly.
4. Discover your Vim heroes and periodically check how they have set
   their editor up. A lot of people publish their dotfiles on GitHub.

You can [view my personal vim configuration here][vimfiles].

### Make it pretty

Aesthetics of your text editor are very important given the time you're
spending in it. You should use a good programming font for your terminal
emulator: for example DejaVu Sans Mono or [Inconsolata][]. This will
affect vim running in the terminal. Next is the color scheme: pick one
by cycling through available colors with <kbd>:color &lt;Tab&gt;</kbd>.

The quality of the color theme will depend severely on your terminal
emulator. You should consider using a graphical Vim to reach the highest
quality of typeface and color; a graphical environment can also offers
extra options such as antialiasing and line spacing settings.

## Find your killer features

Right from the start, you'll need to discover some sweet power moves
that'll keep you returning to Vim rather than your old editor.

Vim has two main modes you need to care about first: the *normal mode*
(where you move, perform commands) and the *insert mode* (where you type
in text). The insert mode is on the surface nothing special when
compared to your old editor: you press <kbd>i</kbd>, you're in insert
mode, now you type text as your normally would. Pressing
<kbd>&lt;Esc&gt;</kbd> exits back to normal mode.

But it is *how* you enter insert mode that offers some advantage:

<table>
  <tr>
    <th><kbd>i</kbd>
    <td>insert before character under cursor
  <tr>
    <th><kbd>a</kbd>
    <td>insert after cursor
  <tr>
    <th><kbd>I</kbd>
    <td>insert at beginning of current line
  <tr>
    <th><kbd>A</kbd>
    <td>insert at end of the line
  <tr>
    <th><kbd>o</kbd>
    <td>starts insert mode in a new line below current one
  <tr>
    <th><kbd>O</kbd>
    <td>insert in a new line above current one
</table>

You can also enter insert mode by replacing existing text at the same
time:

<table>
  <tr>
    <th><kbd>ciw</kbd>
    <td>("change inner word") change word under cursor
  <tr>
    <th><kbd>ci"</kbd>
    <td>change double-quoted string (but keep the quotes)
  <tr>
    <th><kbd>ci(</kbd>
    <td>change text between matching parentheses, also works with brackets
  <tr>
    <th><kbd>cc</kbd>
    <td>change whole line
</table>

These shortcuts are a very compelling argument why Vim is more efficient
at *editing* code than most editors, but they are just the tip of the
iceberg of all the things possible with different motions.

For more information, see <kbd>:help change.txt</kbd>.

## Learn to move

In Vim, efficient movement is everything. After all, most of your time
is spent *editing*, not inserting text and code. Commands for selecting,
changing, moving and transforming text all follow the same motion
rules, thus making them fundamental in knowing Vim.  For example, in the
<kbd>ciw</kbd> command mentioned previously, the <kbd>iw</kbd> is a
motion that means "inner word".

Since there are too many different motions to remember at once, you'll
need some sort of a mnemonic to help you while learning.

<div><aside>
  <h3>The best Vim cheat sheet</h3>

  <p>Do an <a href="http://www.google.com/search?q=vim+cheat+sheet&amp;source=lnms&amp;tbm=isch">image search for &ldquo;vim cheat sheet&rdquo;</a> and you&rsquo;ll quickly hate life. But not all of them are ugly – there is a smart <a href="http://naleid.com/blog/2010/10/04/vim-movement-shortcuts-wallpaper/" title="Vim movement shortcuts wallpaper by Ted Naleid">wallpaper originally designed by Ted Naleid</a> and <a href="https://github.com/LevelbossMike/vim_shortcut_wallpaper#readme" title="A Vim cheat sheet wallpaper">re-worked with smoother colors</a>:</p>

  <p><a href="https://github.com/LevelbossMike/vim_shortcut_wallpaper/raw/master/vim-shortcuts_1280x800.png" title="1280x800 Vim cheat sheet wallpaper"><img src="https://a248.e.akamai.net/assets.github.com/img/57cc2998508eb2c6f0b46b265354e6dda3145757/687474703a2f2f696d673835302e696d616765736861636b2e75732f696d673835302f373533372f62696c6473636869726d666f746f323031313034303275672e706e67" alt="vim motion keys" /></a></p>

  <p>I don&rsquo;t actually use it as a wallpaper; while learning I&rsquo;ve kept it open in a separate window and switch to it whenever I needed to remind myself of what kind of motions are there.</p>
</aside></div>

### Knowing where you want to go

When you think about it, you always know where you want to position your
cursor before editing – if you can describe your intent to Vim, you can
perform it faster than hitting cursor keys repeatedly or even using the
mouse. In general:

* If you repeatedly (more than 2-3 times in a row) hit cursor keys to
  get somewhere, there is a better way.
* If you press backspace more than a couple of times to delete text,
  there is a better way.
* If you find yourself performing the same changes on several lines,
  there is a better way.

Here's an overview of several useful methods for navigating your
document, with 2nd column indicating the corresponding backwards
movement:

<table class="split reverse">
  <thead>
    <tr>
      <th title="forward">→
      <th title="backward">←
      <th>description
  <tbody>
  <tr>
    <th><kbd>/</kbd>
    <th><kbd>?</kbd>
    <td>search for a pattern of text, jump to it by hitting Enter (<kbd>&lt;CR&gt;</kbd>)
  <tr>
    <th><kbd>*</kbd>
    <th><kbd>#</kbd>
    <td>search for the word under cursor
  <tr>
    <th><kbd>n</kbd>
    <th><kbd>N</kbd>
    <td>jump to the next match for the previous search
  <tr>
    <th><kbd>$</kbd>
    <th><kbd>^</kbd>
    <td>position cursor at end of current line
  <tr>
    <th><kbd>f</kbd>
    <th><kbd>F</kbd>
    <td>position cursor on the character in the same line that matches the next keystroke
  <tr>
    <th><kbd>t</kbd>
    <th><kbd>T</kbd>
    <td>position cursor <em>before</em> the next character that matches the keystroke
  <tr>
    <th><kbd>;</kbd>
    <th><kbd>,</kbd>
    <td>repeat the last <kbd>f</kbd>, <kbd>F</kbd>, <kbd>t</kbd>, or <kbd>T</kbd>
  <tr>
    <th><kbd>w</kbd>
    <th><kbd>b</kbd>
    <td>move to start of next word
  <tr>
    <th><kbd>W</kbd>
    <th><kbd>B</kbd>
    <td>move to start of next "WORD" (sequence of non-blank characters)
  <tr>
    <th><kbd>}</kbd>
    <th><kbd>{</kbd>
    <td>move down one paragraph (block of text separated by blank lines)
  <!--<tr>
    <th><kbd>&lt;C-f&gt;</kbd>
    <th><kbd>&lt;C-b&gt;</kbd>
    <td>move down one page-->
  <tr>
    <th colspan=2><kbd>gg</kbd>
    <td>jump to first line of document
  <tr>
    <th colspan=2><kbd>G</kbd>
    <td>jump to end of document
</table>

To me, the killer ones on this list are "word", "WORD" and paragraph
motions.

<div><aside>
<p><strong>Pro tip</strong>: while in insert mode, <kbd>&lt;C-w&gt;</kbd> deletes the last word before cursor. This is more efficient than backspacing.</p>
</aside></div>

For in-depth documentation on motions, see <kbd>:help motion.txt</kbd>.

### After the jump

Some of the above motions jump, and it is very useful to know how to
backtrack those jumps. From the <kbd>:help jump-motions</kbd>
documentation:

> A "jump" is one of the following commands: `'`, `` ` ``, `G`,
> `/`, `?`, `n`, `N`, `%`, `(`, `)`, `[[`, `]]`, `{`, `}`, `:s`, `:tag`,
> `L`, `M`, `H` and the commands that start editing a new file.  If you
> make the cursor "jump" with one of these commands, the position of the
> cursor before the jump is remembered.

Mastering jumps is insanely powerful. Suppose you edited one line,
exited insert mode and now you are navigating the document to find out
something. Now you want to continue editing from the same spot. The
<kbd>`.</kbd> motion brings the cursor back on the exact place where
the last change was made.

Another example: you are in the middle of a script and need to add some
code, but that needs adding an extra `require` statement (or `import`,
`include`, or similar) near the top of the file. You jump to the top
with <kbd>gg</kbd>, add the `require` statement and jump back to before
the previous jump with <kbd>``</kbd>.

And if you did multiple jumps, you can backtrack with
<kbd>&lt;C-o&gt;</kbd>.

### Yanking: copy & paste

Once you know basic motions and navigating around jumps, you can be
efficient with copy ("yank" in Vim terms) and paste.

<table>
  <tr>
    <th><kbd>Y</kbd>
    <td>yank current line; prepend with number to yank that many lines
  <tr>
    <th><kbd>y}</kbd>
    <td>yank until end of paragraph
  <tr>
    <th><kbd>dd</kbd>
    <td>delete current line but copy it too (think "cut")
  <tr>
    <th><kbd>d3d</kbd>
    <td>delete 3 lines starting from current one
  <tr>
    <th><kbd>p</kbd>
    <td>paste yanked text at cursor; prepend number to paste that many times
  <tr>
    <th><kbd>P</kbd>
    <td>paste before cursor
</table>

What isn't obvious at first, but you're gonna notice soon, is that Vim
doesn't use your OS clipboard by default for these operations; i.e. you
can't paste text copied from Vim in other apps and <kbd>p</kbd> won't
paste into Vim what you just copied from another program on your
computer.

Vim uses its *registers* as its internal clipboard. You can even save
yanked text into a named register of your choosing to ensure it is never
overwritten and paste it later from that register if you need to paste
it multiple times in different spots. Commands for selecting registers
start with `"`:

<table>
  <tr>
    <th><kbd>"aY</kbd>
    <td>yank current line into register "a"
  <tr>
    <th><kbd>"ap</kbd>
    <td>paste from register "a"
  <tr>
    <th><kbd>"*Y</kbd>
    <td>yank line into special register "*" which is the system clipboard
  <tr>
    <th><kbd>"*p</kbd>
    <td>paste from register "*": the system clipboard
  <tr>
    <th><kbd>"_D</kbd>
    <td>delete from cursor until the end of line, but don't copy
</table>

The examples above show how you can explicitly opt-in to use the system
clipboard via `"*`, showing that Vim does have access to the
system clipboard, but doesn't use it by default. The last example uses
the `"_` (the "black hole") register, which is a way to discard text
without copying it and overriding the default register.

<div><aside>
<p><strong>Pro tip</strong>: after you paste code, it might not be indented correctly in the new context. You can select just pasted lines and autoindent them with <kbd>V`]=</kbd>. This should fix the indentation in most cases.  Breaking it down: uppercase <code>V</code> enters line-based visual mode, <code>`]</code> is a motion that jumps to the end of just changed text, and <code>=</code> performs auto-indentation.</p>
</aside></div>

You can inspect the current state of all registers with
<kbd>:registers</kbd>. See <kbd>:help registers</kbd>.

## Quickly navigate files

Real-world projects have more than one file to edit. Efficient switching
between files is just as important as motion commands.

Folks coming from TextMate, IDEs, or gedit will quickly find themselves
craving for a directory tree side pane, and the community is going to
unanimously recommend NERD tree. **Don't use the NERD tree plugin.** It
is clumsy, will *hurt* your split windows workflow because of edge-case
bugs and plugin incompatibilities, and *you never needed* a file browser
pane in the first place, anyway. The only thing it can be useful for is
familiarizing yourself with the directory/file structure of a new
project, and you can easily do that with the `tree` command-line tool
(easily installed via package manager of choice, such as Homebrew on OS
X).

What you need is a plugin for fuzzy searching of file names in a
project. I find the [Command-T][cmdt] plugin very capable and fast, but
it requires ruby on the system and vim compiled with ruby support. If
that's a deal breaker for you, some people swear by [ctrlp.vim][ctrlp].

Here are my mappings for starting a file search with Command-T. I
start a project-wide search with <kbd>,f</kbd> and search the directory
of the current file with <kbd>,F</kbd>:

{% highlight vim %}
" use comma as <Leader> key instead of backslash
let mapleader=","

" double percentage sign in command mode is expanded
" to directory of current file - http://vimcasts.org/e/14
cnoremap %% <C-R>=expand('%:h').'/'<cr>

map <leader>f :CommandTFlush<cr>\|:CommandT<cr>
map <leader>F :CommandTFlush<cr>\|:CommandT %%<cr>
{% endhighlight %}

For this to be fast, ensure that you don't have any other mappings that
start with `<leader>f`, otherwise Vim will always force a slight delay
after keystrokes before resolving the mapping. (See all of the current
mappings in effect with <kbd>:map</kbd>.)

In the same session, Vim remembers previously open files. You can list
them with <kbd>:buffers</kbd> (shortcut: <kbd>:ls</kbd>). Without even
using any plugins, you can jump to a buffer on this list by typing
<kbd>:b</kbd> and a part of a buffer's name. For example, if you've
previously opened `lib/api_wrapper.rb`, you can return to it with
<kbd>:b api</kbd>. Hit <kbd>&lt;Tab&gt;</kbd> to cycle between multiple
matches.

To switch between the currently open buffer and the previous one, use
<kbd>&lt;C-^&gt;</kbd>. This key combination is a bit hard to reach, so
you can remap to, for instance, twice hitting `<leader>`:

{% highlight vim %}
nnoremap <leader><leader> <c-^>
{% endhighlight %}

Since my `<leader>` is set to comma, I just have to hit `,,` to
alternate between files, for instance tests and implementation.

### Split windows

You can split the current buffer horizontally or vertically.  This is
useful, for instance, to view simultaneously the top and the bottom part
of the same file. But it's even more useful to view different files in
splits; for instance tests and implementation.

<table class="split">
  <thead><tr>
    <th>where
    <th>horizonal
    <th>vertical
  <tbody>
  <tr>
    <th>normal mode
    <td><kbd>:split</kbd>
    <td><kbd>:vsplit</kbd>
  <tr>
    <th>:CommandT
    <td><kbd>&lt;C-s&gt;</kbd>
    <td><kbd>&lt;C-v&gt;</kbd>
  <tr>
    <th>:Ack
    <td><i>n/a</i>
    <td><kbd>v</kbd>
</table>

To switch cursor between these windows, use <kbd>&lt;C-w&gt;</kbd> (as
in "window") and then *direction*, where direction is one of:
↑<kbd>k</kbd>&nbsp;↓<kbd>j</kbd>&nbsp;←<kbd>h</kbd>&nbsp;→<kbd>l</kbd>.
To ease this navigation, you can use the following mappings so you can
omit the <kbd>&lt;C-w&gt;</kbd> prefix:

{% highlight vim %}
" easier navigation between split windows
nnoremap <c-j> <c-w>j
nnoremap <c-k> <c-w>k
nnoremap <c-h> <c-w>h
nnoremap <c-l> <c-w>l
{% endhighlight %}

For more information, see <kbd>:help windows</kbd>.

## Further resources for learning

* <kbd>:help</kbd> – it's more useful than it appears at first!
* [Coming home to Vim][cominghome]
* [VimCasts][]
* [Destroy All Software][destroy] – there are some episodes on Vim
* [VimGolf][] – time-consuming but rewarding game
* [Walking Without Crutches][presentation] – presentation by Drew Neil
* [My vim configuration and plugins][vimfiles]

Thanks Gary Bernhardt, Drew Neil for tips and inspiration, and Tim Pope
for all your work on plugins.

  [janus]: https://github.com/carlhuda/janus#readme
    "Janus: a Vim distribution"
  [cominghome]: http://stevelosh.com/blog/2010/09/coming-home-to-vim/
    "Coming home to Vim by Steve Losh"
  [vimcasts]: http://vimcasts.org/ "VimCasts by Drew Neil"
  [destroy]: http://destroyallsoftware.com
    "Destroy All Software – screencasts for serious developers by Gary Bernhardt"
  [syntastic]: https://github.com/scrooloose/syntastic#readme
  [vimgolf]: http://vimgolf.com/ "VimGolf"
  [cmdt]: https://wincent.com/products/command-t "Command-T plugin for Vim"
  [presentation]: http://walking-without-crutches.heroku.com/
    "Vim: Walking Without Crutches presentation by Drew Neil"
  [pathogen]: https://github.com/tpope/vim-pathogen#readme
    "Pathogen makes it easy to install vim plugins in separate directories"
  [alloy]: https://github.com/alloy/macvim/wiki/Screenshots
    "Screenshots of MacVim fork with native file browser"
  [vimfiles]: https://github.com/mislav/vimfiles
    "Mislav's Vim configuration files"
  [inconsolata]: http://www.levien.com/type/myfonts/inconsolata.html
    "Inconsolata programming typeface"
  [ctrlp]: http://kien.github.com/ctrlp.vim/
    "Full path fuzzy file, buffer and MRU file finder for Vim"