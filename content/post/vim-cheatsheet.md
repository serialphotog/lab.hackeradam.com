---
title: "My VIM Cheatsheet"
description: "This is my VIM quick reference"
date: '2024-04-19'
---

This is my ongoing Vim quick reference. Note that this is in no way meant to be an exhaustive cheatsheet for Vim. This is more a list of things I'm personally constantly forgetting how to do. 

# Searching, Pattern Matching, and Replacing

### Searching for a Pattern

```vim
:/pattern
```

## Find and Replace

```vim
:%s/searchtext/replacetext/g
```
**Note:** Leave off the `/g` to only replace the first match on each line.

### Limit the Range of Affected Lines

```vim
:N,Ms/searchtext/replacetext/g
```

Where N is the first line and M is the last line.

### Show All Lines That Match a Pattern

```vim
:g/pattern
```

### Delete All Lines that Contains a Pattern

```vim
:g/searchpattern/d
```

### Delete All Lines that Do Not Contain a Pattern

```vim
:v/searchpattern/d
```

or

```vim
:g!/searchpattern/d
```

## Registers

In Vim, the concept of a clipboard is called a register. Like any other text-editor, Vim has a default register that you can use for copy and paste, but you can also use each letter (a through z) to have multiple named registers.

### In Normal Mode

- To copy the current word under the cursor into the default register: `yw`
- To copy the current word under the cursor into the "a" register: `ayw`
- To paste from the default register at the cursor: `p`
- To paste from the "a" register after the cursor: `ap`

### In Edit Mode

- To paste the contents of the default register: `<ctrl-r>`
- To paste the contents from the "a" register: `<ctrl-r>a`

## Bookmarks

Vim allows you to set bookmarks using the letters a through z so you can more easily refer to a section of a given file.

- Set local bookmark called "a" - `ma`
- Go to local bookmark called "a" - `'a`
- For global bookmarks (spanning multiple files/buffers), use uppercase letters: `mA`, `'A`

## Inverting Case

- For a single line: `g ~ ~`
- For a single character under the cursor: `~`
- For a word under the cursor: `~iw` 