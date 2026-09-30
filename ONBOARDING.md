# The Slate: how it's set up and how to work on it

## The big picture

The Slate is the filmmaking team's storyboard tool. It lives in two places, and each one has a single job.

| | **The design file** | **The live Slate** |
|---|---|---|
| What it is | `The Slate design copy.html` in [the-slate-design-code](https://github.com/lc-camdengoff/the-slate-design-code) | The real tool at [slate.creativemedia.church](https://slate.creativemedia.church/), built from [the_slate](https://github.com/lc-camdengoff/the_slate) |
| Its job | **Where the app is designed and changed.** It's the original. | **Shows the latest design to the team**, with real accounts and real storyboards. |
| Server | A built-in **test server** with made-up people and boards | The **real server** (the PHP files in `the_slate`) |
| Who sees it | Anyone with the link to the test page | The team, after signing in |

Changes flow one way only:

```
design file  ──(you click Publish)──►  live Slate
```

The live Slate never sends anything back to the design file.

## 1. Look at the design

Open the test page: **https://lc-camdengoff.github.io/the-slate-design-code/**

It's the full Slate: storyboards, the Team Library, Share, comments and the Trash all work. None of it is real:

- **You're signed in as Craig Groeschel**, an ordinary team member (not an admin).
- **The teammates, boards and comments are sample data** built into the file.
- **Anything you do is saved only in your own browser.** Nobody else sees it, and it never touches real storyboards. Experiment freely.
- **GIF search and the top bar's links don't work** here. They need the real server.

You can also download the file and open it straight from your computer, or open it in a design tool. It works the same way.

## 2. Make a change

Every change to how the Slate looks or works goes into **`The Slate design copy.html`**. Three ways to do it:

- **With Claude:** start a Claude Code session on `the-slate-design-code` and describe what you want. The [README](README.md) tells Claude how to edit the file safely.
- **In a design tool or editor:** edit the file, keep its name, and upload it to the repo.
- **By upload on GitHub:** always start from the **latest** file on `main`. If you upload a copy you've had for a while, you'll quietly undo everyone else's changes made since.

Merging to `main` updates the **test page only**, usually within a minute or two. The live Slate doesn't change yet, so you can merge, look, and tweak as often as you like.

**If the test page still looks old,** do a hard refresh: Cmd+Shift+R on a Mac, Ctrl+Shift+R on Windows.

**About the sample data:** your browser keeps what you did on the test page between visits. When someone changes the sample data, your browser throws away its old copy by itself and starts fresh. To start over any other time, open the browser console and run `slateDesignReset()`.

## 3. Publish it to the live Slate

When the design on `main` is ready for the team:

1. Go to [the_slate → Actions → Deploy The Slate](https://github.com/lc-camdengoff/the_slate/actions/workflows/deploy.yml).
2. Click **Run workflow**, leave the branch as `main`, then click **Run workflow** again.
3. Wait about 15 seconds for a **green tick**.

Behind the scenes, it:

1. takes the app from the design file,
2. removes the test server and switches the real one on,
3. saves that version in `the_slate`, noting which design version it came from,
4. uploads it to the live site.

Team members see the new version the next time they load the Slate.

**Heads-up:** any change merged into `the_slate`'s `main` (a server fix, for example) also publishes whatever is on the design file's `main` at that moment. **Treat the design file's `main` as "ready to go live".** Keep half-finished work on a branch or in a draft pull request.

## 4. Undo a change on the live site

1. In `the-slate-design-code`, undo the change: revert its commit or pull request on GitHub.
2. Click **Run workflow** again. The live Slate goes back to match.

## The rules

1. **Change the app only in the design file.** Never edit `app.html` or `src/template.html` in `the_slate`. If someone does, the next publish **stops with a red X** instead of overwriting their work, and the message says to make the change in the design file.
2. **Leave the test server's two markers alone.** Keep the `data-design-copy` on the test server's `<script>` and the line `const LIB_ENABLED = true; …`. If either is missing, publishing stops, so the test server can't end up on the live site.
3. **Keep the file's name:** `The Slate design copy.html`.
4. **Server changes go in `the_slate`**: the PHP files, sign-in rules and storage. If the real server starts sending something new, update the test server in the design file to match, so the test page keeps working.

## If publishing shows a red X

Nothing went live; the team still has the previous version. Click the failed run and read the message:

| Message says | What happened | Fix |
|---|---|---|
| "The app here … was changed after it last came from the design copy" | Someone edited the app directly in `the_slate` | Move their change into the design file, put `the_slate`'s copy back, then Run workflow again |
| "should have one `<script data-design-copy>`" or "one `const LIB_ENABLED = true;` line" | The design file lost one of its markers | Restore the marker in the design file, merge, then run again |
| Anything else | Something unexpected | Ask Claude or a developer, and share the link to the failed run |

## Where to read more

- This repo's [README](README.md) covers working on the file.
- `the_slate`'s README, and DEPLOY.md's **"Where the app comes from"** section, cover how publishing and the server work.
