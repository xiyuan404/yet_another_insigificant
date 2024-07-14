gg - goes to the begin of the document
G - navigates at its very end
d - delete mode
`d^`: delete to the start of line
`cw`: change from cursor to end of word and enter insert mode
`:.,$d`: delete from current line to end of the file
`diw`: delete (cut) word under the cursor
`daw`: delete (cut) word under the cursor and the space after or before it
`dd`: delte the whole lines
`V, expnad or shrink the line selection, and press d`: select and delete
`opt + shift + cursor under line`: multi line selection

```
copy/delete word under cursor in Vim
yw / byw

Assuming that the cursor is at the first character of the word simply do this in command mode:
     yw
y is for yank and w is for word.

Other ways of doing the same thing which are not as efficient:
     vey
the v starts visual select mode. e tells vim to move to end of word. y yanks or copies the word. to delete replace y with x.

if the cursor is somewhere in the middle of the word, add a b before the command as in:
     byw
or
     bvey

the v starts visual select mode. e tells vim to move to end of word(`b` tells vim to move to start of word). y yanks or copies the word. to delete replace y with x.
``

`viw` does a visual select inside the word.
`bve` to select the word under cursor
Be aware though that if the cursor points to a first character you just need `ve` combination


yaw to yank the entire word regardless of where the cursor is located.

type `cs('` to [c]hange [s]urroundings from ( to '
`ds(` to [d]elete  [s]urroundings `{` altogether

c deletes the selection and enters insert mode
() types the opening and closing parentheses
<Esc> exits insert mode
P pastes the previously deleted text inside the parentheses



1.Press v to begin character-based visual selection, or V to select whole lines,
2. Press d (delete) to cut, or y (yank) to copy.
3. Press p to paste after the cursor, or P to paste before.



[^1]:[wrap.dev Vim ](https://www.warp.dev/topic/vim)
