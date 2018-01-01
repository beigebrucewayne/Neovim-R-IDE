# Writing Rust With A REPL In Vim

![header](https://i.imgur.com/VYDVxmc.png)

Rust is a fascinating language filled with novel ideas. However, the tooling is still maturing. The *seemingly* best option for writing Rust appears to be [Visual Studio Code](https://code.visualstudio.com) with the [RLS plugin](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust). It's a nice setup, but I think there is a slightly more optimal solution utilizing Vim with a Rust REPL. This post will detail this layout.

## Rust Support

First up is getting basic support for Rust. Luckily the Rust team makes this easy with [rust-lang/rust.vim](https://github.com/rust-lang/rust.vim). The `rust.vim` plugin adds the usual: file detection, syntax highlighting, formatting, etc...

One novel feature of the plugin is automatic `rustfmt` on save.

```vim
let g:autofmt_autosave = 1
```

## LSP

After establishing basic support, the next step is getting access to the RLS. In order to do this you'll need a plugin to communicate with the language server. For Vim there is [Vim-Lsp](https://github.com/prabirshrestha/vim-lsp) and for Neovim [LanguageClient-Neovim](https://github.com/autozimu/LanguageClient-neovim) - *also works with Vim*.

```vim
" a basic set up for LanguageClient-Neovim

" << LSP >> {{{

let g:LanguageClient_autoStart = 0
nnoremap <leader>lcs :LanguageClientStart<CR>

" if you want it to turn on automatically
" let g:LanguageClient_autoStart = 1

let g:LanguageClient_serverCommands = {
    \ 'python': ['pyls'],
    \ 'rust': ['rustup', 'run', 'nightly', 'rls'],
    \ 'javascript': ['javascript-typescript-stdio'],
    \ 'go': ['go-langserver'] }

noremap <silent> H :call LanguageClient_textDocument_hover()<CR>
noremap <silent> Z :call LanguageClient_textDocument_definition()<CR>
noremap <silent> R :call LanguageClient_textDocument_rename()<CR>
noremap <silent> S :call LanugageClient_textDocument_documentSymbol()<CR>
" }}}
```

## Completions

Another core component to this setup is a completion manager. Neovim has [nvim-completion-manager](https://github.com/roxma/nvim-completion-manager) - a great choice, while Vim has [YouCompleteMe](https://github.com/Valloric/YouCompleteMe) or [Completer](https://github.com/maralla/completor.vim).

## RLS

Now we are starting to get to the meaty part - the RLS, or [Rust Language Server](https://github.com/rust-lang-nursery/rls). A brief description from the repo is below.

> The RLS provides a server that runs in the background, providing IDEs, editors, and other tools with information about Rust programs. It supports functionality such as 'goto definition', symbol search, reformatting, and code completion, and enables renaming and refactorings.

> The RLS gets its source data from the compiler and from Racer. Where possible it uses data from the compiler which is precise and complete. Where its not possible, (for example for code completion and where building is too slow), it uses Racer.

The installation process is easy enough. 

```bash
# get rustup
# don't use hombrew
curl https://sh.rustup.rs -sSf | sh

rustup self update

# get nightly compiler
rustup update nightly

# after nightly installed
rustup component add rls-preview --toolchain nightly
rustup component add rust-analysis --toolchain nightly
rustup component add rust-src --toolchain nightly
```

After that, you'll pass in the commands to your LSP plugin of choice. An example config for `LanguageClient-Neovim` is provided below.

```vim
let g:LanguageClient_serverCommands = {
    \ 'rust': ['rustup', 'run', 'nightly', 'rls']
}
```

If you open a Rust file in Vim you should see something analogous to below.

![completion](https://i.imgur.com/tUbEkZU.png)

## RUSTI

Last but certainly not least, [Rusti](https://github.com/murarth/rusti). This solution isn't a silver bullet. Yet, it provides something quite unique to Rust development - a REPL. This small quirk is great for quickly verifying an idea. And for myself, has made learning Rust significantly easier.

To get Rusti follow these steps:

```bash
# Rusti builds with Rust nightly, using the Cargo build system.
# Currently, it must be built using a nightly release of the Rust compiler released no later than 2016-08-01.
rustup install nightly-2016-08-01

# install rusti using cargo
rustup run nightly-2016-08-01 cargo install --git https://github.com/murarth/rusti

# next up we'll alias the run command
alias rrepl="rustup run nightly-2016-08-01 ~/.cargo/bin/rusti"
```

Rusti Workflow:

- Split screen `<C-W>v`
- Enter terminal mode `:term`
- Get REPL `rrepl`

* Example
```bash
rusti=> println!("Hello, world!");
Hello, world!
rusti=> 2 + 2
4
rusti=> (0..5).collect::<Vec<_>>()
[0, 1, 2, 3, 4]
```

* The `.block` command will run multiple lines of Rust code as one program. To end the command and run all code, input . on its own line.
```bash
rusti=> .block
rusti+> let a = 1;
rusti+> let b = a * 2;
rusti+> let c = b * 3;
rusti+> c
rusti+> .
6
```

### BONUS

One last thing to note, that may or may not be obvious depending on your Vim knowledge, is executing cargo commands in Vim. Specifically `:! cargo run` and the like.
