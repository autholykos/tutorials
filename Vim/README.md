# Vim as a Node/JS IDE

Vim is my choice of IDE. I like it because you can do things that are impossible in any other text editor. Since with great powers come great responsibilities, VIM is a tad bit more complicated than other edtitors and requires some setup by itself.

This won't be an extended tutorial about VIM as much as a tutorial about using VIM as an IDE to program in Node.

Vim comes with great **plugins** that help developers achieve an incredible number of tasks.

## Vundle

[Vundle](https://github.com/gmarik/Vundle.vim) stands for *Vim Bundle* and its aim is to makes it very simple to manage Vim plugins.

Quoting `Vundle` directly,

> Installation requires [Git] and triggers [`git clone`] for each configured repository to `~/.vim/bundle/` by default. Curl is required for search.

## NERDTree

A very cool functionality of NERDTree is the possibility to find text in a path.

```vim
:set path=/path/to/proj/**

:find keyword
```

