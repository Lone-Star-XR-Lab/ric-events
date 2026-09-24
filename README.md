# RIC Upcoming Events

A small static page listing upcoming events for the Lone Star College–Tomball
Research & Innovation Center (RIC). It's published on GitHub Pages and
embedded as a permanent iframe on the RIC's page in the college CMS, so
events can be added or updated immediately instead of waiting on CMS
approval.

**Live site:** https://lone-star-xr-lab.github.io/ric-events/

## How it's embedded

The CMS page contains this iframe, which doesn't need to change again:

```html
<iframe
    src="https://lone-star-xr-lab.github.io/ric-events/"
    title="Research and Innovation Center Upcoming Events"
    width="100%"
    height="1000"
    style="border:0;">
</iframe>
```

## Files

```
ric-events/
├── index.html   Page structure, styling, and rendering logic. Rarely needs editing.
├── events.js    The event list. Edit this to add/change/remove events.
└── assets/      Flyer images referenced by events.js.
```

You should almost never need to touch `index.html` — day-to-day updates only
involve `events.js` and `assets/`.

## Adding a new event

1. Add the flyer image to `assets/` (PNG or JPG). Keep file names lowercase
   with hyphens, no spaces (e.g. `xr-open-lab.png`).
2. Open `events.js` and copy the **FIELD REFERENCE** block from the comment
   near the top of the file into the `events` list, then fill it in. See
   [Event fields](#event-fields) below for what each one does.
3. Save, commit, and push to GitHub:
   ```
   git add events.js assets/your-flyer.png
   git commit -m "Add [event name]"
   git push
   ```
4. GitHub Pages redeploys automatically, usually within a minute or two.
   Refresh the live site (or the CMS page) to confirm.

## Editing or removing an event

- **Edit:** find the event's block in `events.js` and change the field(s).
- **Remove early:** delete the event's whole `{ ... }` block.
- **Let it expire naturally:** do nothing — see [Automatic expiration](#automatic-expiration).

## Event fields

| Field | Required? | Notes |
|---|---|---|
| `title` | Required | The event's headline. |
| `date` | Required | Format `YYYY-MM-DD`. Used for sorting and the "Date:" line. |
| `time` | Optional | Free text, e.g. `"1:00 PM – 3:00 PM"`. |
| `location` | Optional | Free text, e.g. `"XR Lab, E251"`. |
| `course` | Optional | A MyWorkshops course code, e.g. `"LSC-5247"`. Shown as "Course:" **and** used to auto-build a "Register in MyWorkshops" link (a course-code search) if `link` isn't set. |
| `description` | Optional | A sentence or two shown under the details. |
| `image` | Optional | Path to the flyer, e.g. `"assets/xr-open-lab.png"`. |
| `imageAlt` | Recommended if `image` is set | Describes the flyer for screen readers — describe the image, don't just repeat the title. |
| `link` | Optional | A direct URL for the button. Overrides the auto-generated `course` link if both are set. |
| `linkText` | Optional | Button text. Defaults to `"Learn More"`. |
| `expires` | Optional | Format `YYYY-MM-DD`. Defaults to `date` if omitted. See below. |

## Automatic expiration

An event stays visible **through the end of its `expires` date** and
disappears starting the next day. If `expires` isn't set, `date` is used
instead. This means most one-day events never need to be manually deleted —
just leave them in `events.js` and they'll drop off on their own. (You can
still delete old blocks occasionally to keep the file tidy.)

If there are no current events, the page automatically shows a "No featured
events scheduled" message instead of an empty grid.

## MyWorkshops registration links

For events tied to a MyWorkshops course code, set `course` (e.g.
`"LSC-5248"`) and leave `link` out — the page builds the registration link
for you:

```
https://myworkshops.lonestar.edu/psc/elmprd/EMPLOYEE/ELM/c/LM_OD_EMPLOYEE_FL.LM_FND_LRN_FL.GBL?Page=LM_FND_LRN_RSLT_FL&Action=U&KWRD=LSC-5248
```

For anything else (an external registration form, a general info page,
etc.), just set `link` directly and skip `course`.

## Previewing changes before you push

Open this folder in VS Code and use the **Live Server** extension (right-click
`index.html` → "Open with Live Server") to preview locally. Live Server
auto-reloads on save, so you can edit `events.js` and see the result
immediately without waiting on a GitHub Pages deploy.

## Known limitations

- The CMS iframe has a fixed height (`1000px`). If there are only one or two
  events, there will be some empty space below them — this is a CMS-side
  constraint, not a bug in this page.
- Flyer images are shown as-is at up to 320px tall. Large image files (over
  ~1MB) will load noticeably slower on mobile; compressing flyers before
  upload (e.g. with Squoosh or TinyPNG) is recommended.
