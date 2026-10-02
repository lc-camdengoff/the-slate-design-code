# The Slate: design copy

The Slate, the filmmaking team's storyboard and shot-list tool, as one HTML
file to design changes on. This file is the original: the live Slate takes
its app from here, and its server from
[lc-camdengoff/the_slate](https://github.com/lc-camdengoff/the_slate).

New here? [ONBOARDING.md](ONBOARDING.md) walks through the whole process:
looking at a design, changing it, and publishing it to the live Slate.

## Opening it

Open `The Slate design copy.html` in a browser or a design tool. It has a
stand-in for the server built in, so all of it works on its own, the Team
Library, Share, comments and the Trash included, with sample people,
storyboards and comments:

- You're signed in as Craig Groeschel, an ordinary member of the team. One of
  your storyboards is shared with two people and has someone asking for
  access. Others are shared with you to comment on, or only to view. The
  Team Library has boards that everyone can edit, comment on or only view.
- Some of the sample people have a photo (a drawn one) and the rest show
  their initials, as on the live Slate.
- To see what someone gets from a link to a board they can't open, add
  `#board=christmas-sneak-peek-5e1f2a` to the end of the file's address.
- Whatever you do (new boards, comments, sharing) is kept in your browser,
  not on any server. When a new version of the file changes the sample data,
  your browser starts again from it by itself the first time you open it. To
  start again from the sample data any other time, open the browser's
  console and run `slateDesignReset()`.
- GIF search finds nothing, a still dragged in from ShotDeck asks for a
  paste instead, and the top bar's links go nowhere: those need the real
  server.
- Opened from disk, the browser's console lists "Unsafe attempt to load URL
  {{ themeIcon }}" and a few like it. That's normal.

## Changing it

- Edit the file in place, keep its name, and commit with a line saying what
  changed.
- The `<script data-design-copy …>` at the top of the app is the stand-in
  server and its sample data. They stay here: the live Slate takes out the
  stand-in and uses the real server. Change the sample data if it helps you
  try out a design. Leave `const LIB_ENABLED = true; …` as it is, and keep
  the `data-design-copy` on the stand-in's `<script>`: going live stops if
  either is missing, rather than put the stand-in on the live site.
- Merging to `main` updates this repo's GitHub Pages copy only. To make it
  live, open [the_slate's Actions](https://github.com/lc-camdengoff/the_slate/actions/workflows/deploy.yml),
  choose **Deploy The Slate**, then **Run workflow**. It takes the app from
  this file on `main`, without the stand-in, and deploys it with the real
  server. Any push to the_slate's `main` does the same.
- Change the app here, never in the_slate: its deploy stops rather than
  overwrite a change made there. When the real server starts sending
  something new, change the stand-in to match.

### With Claude

The app is the JSON string inside `<script type="__bundler/template">`, all
on one line. Change it by decoding that string, editing the HTML, CSS and
script it holds, and writing it back encoded exactly as before. Leave the
rest of the file alone: `__bundler/manifest` holds the fonts and libraries.
Inside the app, the first `<script>`, marked `data-design-copy`, is the
stand-in server; change it only to change the sample data.

```python
import json, re
path = 'The Slate design copy.html'
page = open(path, encoding='utf-8').read()
m = re.search(r'<script type="__bundler/template">\n(.*?)\n  </script>', page, re.S)
app = json.loads(m.group(1))
# ... edit app, the page's HTML, CSS and script ...
app = json.dumps(app, ensure_ascii=False).replace('</', '<\\u002F')
open(path, 'w', encoding='utf-8').write(page[:m.start(1)] + app + page[m.end(1):])
```
