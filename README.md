# The Slate: design copy

The Slate, the filmmaking team's storyboard and shot-list tool, as one HTML
file to design changes on. What's designed here is merged into the live
Slate, which is built from
[lc-camdengoff/the_slate](https://github.com/lc-camdengoff/the_slate).

## Opening it

Open `The Slate (design copy).html` in a browser or a design tool. It runs
on its own:

- There's no sign-in. Storyboards are kept in the browser that opened the
  file, not on the server.
- What needs the server isn't here: sign-in and the top bar, the Team
  Library and the Trash, Share, and comments.
- Opened from disk, the browser's console lists "Unsafe attempt to load URL
  {{ themeIcon }}" and a few like it. That's normal.

## Changing it

- Edit the file in place, keep its name, and commit with a line saying what
  changed.
- Leave `const LIB_ENABLED = false;` as it is: it's what lets the file run
  on its own.
- A commit whose message starts "Fresh copy from the Slate" replaces the file
  with the live Slate, including everything of yours merged so far. Always
  work from the latest file.

### With Claude

The app is the JSON string inside `<script type="__bundler/template">`, all
on one line. Change it by decoding that string, editing the HTML, CSS and
script it holds, and writing it back encoded exactly as before. Leave the
rest of the file alone: `__bundler/manifest` holds the fonts and libraries.

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
