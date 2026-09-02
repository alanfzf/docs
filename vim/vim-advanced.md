# Vim Advanced Cheat Sheet

## Do something with external commands
1. We can use `:.!<command>` to replace the current line with the output
   of an external command, example:

`:.!date` will replace the current line with the output of the date
command, date command can be anything.


1. We can use `:%!<command>` to format the whole file with an external
   command, example:

`:%!column -t` to format tables in whole file.

** Insert contents of register in : or / prompt
1. `<C-r><C-w>` to insert the word under the cursor
2. `<C-r>"` to insert the last yanked text
3. `<C-r>/` to insert the last search pattern
4. `<C-r>=` to insert the result of an expression

** G Commands
1. `gi` to go to last insert position and enter insert mode
2. `g;` to jump to previous change
3. `g,` to jump to next change
4. `gv` to reselect last visual selection
