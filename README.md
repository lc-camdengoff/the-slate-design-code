# The Slate: design copy

The Slate, the filmmaking team's storyboard and shot-list tool, as one HTML
file to design changes on. What's designed here is merged into the live
Slate, which is built from
[lc-camdengoff/the_slate](https://github.com/lc-camdengoff/the_slate).

## Opening it

Open `The Slate (design copy).html` in a browser or a design tool. It has a
stand-in for the server built in, so all of it works on its own, the Team
Library, Share, comments and the Trash included, with sample people,
storyboards and comments:

- You're signed in as Riley Carter, an ordinary member of the team. One of
  your storyboards is shared with two people and has someone asking for
  access. Others are shared with you to comment on, or only to view. The
  Team Library has boards that everyone can edit, comment on or only view.
- To see what someone gets from a link to a board they can't open, add
  `#board=christmas-sneak-peek-5e1f2a` to the end of the file's address.
- Whatever you do (new boards, comments, sharing) is kept in your browser,
  not on any server. To start again from the sample data, open the browser's
  console and run `slateDesignReset()`.
- GIF search finds nothing, and the top bar's links go nowhere: those need
  the real server.
- Opened from disk, the browser's console lists "Unsafe attempt to load URL
  {{ themeIcon }}" and a few like it. That's normal.

## Changing it

- Edit the file in place, keep its name, and commit with a line saying what
  changed.
- The `<script data-design-copy …>` at the top of the app is the stand-in
  server and its sample data. Change the sample data if it helps you try out
  a design, but only your changes to the app itself are merged into the
  Slate. Leave `const LIB_ENABLED = true; …` as it is.
- A commit whose message starts "Fresh copy from the Slate" replaces the file
  with the live Slate, including everything of yours merged so far. Always
  work from the latest file.

### With Claude

The app is the JSON string inside `<script type="__bundler/template">`, all
on one line. Change it by decoding that string, editing the HTML, CSS and
script it holds, and writing it back encoded exactly as before. Leave the
rest of the file alone: `__bundler/manifest` holds the fonts and libraries.
Inside the app, the first `<script>`, marked `data-design-copy`, is the
stand-in server; change it only to change the sample data.

```python
import json, re
path = 'The Slate (design copy).html'
page = open(path, encoding='utf-8').read()
m = re.search(r'<script type="__bundler/template">\n(.*?)\n  </script>', page, re.S)
app = json.loads(m.group(1))
# ... edit app, the page's HTML, CSS and script ...
app = json.dumps(app, ensure_ascii=False).replace('</', '<\\u002F')
open(path, 'w', encoding='utf-8').write(page[:m.start(1)] + app + page[m.end(1):])
```
