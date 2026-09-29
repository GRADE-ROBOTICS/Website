# Website# grade-robotics.com — website v1

## Current

**v1 built 2026-09-21, not live.** A one-page static site, modelled on a
Figma → Claude Code → Higgsfield "construction website" build, rebuilt for
Grade's own story. Nothing here is deployed. grade-robotics.com still shows
GoDaddy Website Builder's "Launching Soon" page.

- **Files:** `index.html` holds everything: CSS, copy and the canvas
  scenes. Also `logo.svg` (the white v2 lockup) and `favicon.svg` (the
  orange app icon). No build step, no dependencies. Fonts come from
  Google Fonts.
- **Run it locally:** open `index.html` in a browser. Or use the
  `grade-website` entry in `.claude/launch.json`, which serves it at
  <http://localhost:8090>.
- **Private preview:** <https://claude.ai/artifact/HPekRjDFY6u7gzgWxDLLvW>. Only Yash can open it until it's shared from the page's Share menu.
- **Before it goes live:** Jiten reviews the copy, then someone makes the
  hosting decision (see [Going live](#going-live)).

## What's on the page

| Section | What it does |
| --- | --- |
| Hero | A huge **GRADE** word sits *behind* the machines. An excavator loads a tipper while a grader finishes and a second tipper cycles. This is the video's text-behind-the-excavator trick, done with live drawing instead of a photo cut-out. |
| Statement | The problem: one late tipper stalls the excavator, which stalls the grader. Then the cycle: dig → load → haul → dump → grade. |
| The fleet at work | A pinned, scroll-driven journey through six terrains: **house build → pipeline trench → desert highway → mountain rock slide → forest road → Moon & Mars**. In every scene the same fleet cooperates: EX-01 loads whichever tipper is parked, the other tipper hauls to Dump B, and a grader or dozer works alongside. Orange tags and a dashed link show the coordination. |
| Facts | 10 Hz, 3, 6, 0. All real (see below). |
| Products | The six-product line-up from customer one-pager v7, each with its honest status tag. |
| What's built | Four built items, each tagged with exactly what it is: hardware prototype, simulated machine, simulation. |
| CTA + footer | Pilot offer and contact. The footer is laid out like the title block of an engineering drawing. |

**Every claim comes from files already approved in this repo:**

- Headline, products and status tags: [customer one-pager v7](../../materials/one-pagers/company-one-pager/CHANGELOG.md).
- Built items and 10 Hz: [answer-bank.md](../../leadership/ceo/applications/answer-bank.md) Q4.
- The hero-video rules, all followed: no "autonomous" in the headline, no invented numbers, machines not painted yellow or orange. See [website-hero-video-fleet-os.md](../../marketing/content/website-hero-video-fleet-os.md) §1.
- The scenes are labelled as illustration on the page ("Our first pilots are pipeline and highway earthworks").
- Moon & Mars is tagged **Long-term vision**.

## Swapping in a filmed version (Higgsfield / Seedance)

The canvas scenes work as they are. To use AI footage the way the reference
video did:

1. Generate **six clips, one per terrain, ~5 s each.** The terrains are
   meant to look different, so the consistency problem the video hit (the
   same house changing shape between clips) doesn't apply here.
2. Paste the block below into every prompt, followed by that scene's line.
   Use the negative prompt from
   [website-hero-video-fleet-os.md §4.2](../../marketing/content/website-hero-video-fleet-os.md).

   ```
   Generic unbranded heavy equipment working together as one crew: a dusty
   grey tracked excavator loading a parked tipper truck with a plain steel
   bed, a second tipper driving away loaded, and a [motor grader | bulldozer]
   working alongside. No logos, no lettering, no painted stripes, no yellow
   or orange paint. Wide side-on shot at machine height, camera tracking
   slowly to the right. Documentary realism, filmic grade, not CGI. No
   people close to camera.
   ```

   | # | Terrain | Scene line (extra machine) |
   | --- | --- | --- |
   | 1 | House build | A residential plot at dusk: a concrete slab and timber wall frames of a house going up on the left, footings being dug, a spoil heap on the right. *(grader)* |
   | 2 | Pipeline trench | A long straight pipeline trench through open farmland, steel pipe sections resting on wooden skids beside it. *(bulldozer)* |
   | 3 | Desert highway | A new highway corridor through open desert, dunes behind, survey stakes along the edge, heat haze. *(grader)* |
   | 4 | Mountain rock slide | A mountain road half-blocked by a rock slide, boulders across the single lane, a steep grey slope behind, overcast light. *(bulldozer)* |
   | 5 | Forest road | A narrow access track being cut through dense conifer forest, fresh stumps along the edge, machines in single file. *(grader)* |
   | 6 | Moon → Mars | Concept shot, clearly futuristic: the same crew on grey lunar regolith with craters and Earth low in a black sky, then a match cut to red Martian ground beside a small domed habitat. *(bulldozer)* |

3. Join the six clips **in that order, at equal length.** The captions
   switch at even sixths of the scroll, so unequal scenes would drift out
   of sync with their text. Use a quick whip pan to the right between
   scenes. Delete the audio.
4. Export for scrubbing. Short keyframe gaps keep scroll-seeking smooth.
   Install ffmpeg first if needed (`winget install Gyan.FFmpeg`).

   ```bash
   ffmpeg -i fleet-story-master.mp4 -an -vf scale=1600:-2 -c:v libx264 -g 8 -crf 26 -pix_fmt yuv420p -movflags +faststart media/fleet-story.mp4
   ```

   Aim for under ~15 MB. Raise `-crf` if it's bigger.
5. In `index.html`, set `data-src="media/fleet-story.mp4"` on
   `<video id="storyVideo">`. The film then replaces the drawn scenes, and
   scrolling scrubs it. If the file doesn't load, the drawn scenes stay.

Keep the raw generations out of git (same rule as the hero-video plan).
**The film is an illustration.** Never caption it as a real or customer
site, and never use it as product evidence in an application.

## Going live

The live domain runs **GoDaddy Website Builder**, which can't host a
hand-built page like this one. Options:

- **Free static hosting** (Cloudflare Pages, Netlify or GitHub Pages):
  upload this folder, then point the domain's `www` and apex records at the
  host in GoDaddy DNS.
  - **Leave the MX, SPF and DMARC records alone.** <jiten@grade-robotics.com>
    depends on them.
- **GoDaddy web hosting (cPanel)** keeps everything at GoDaddy but costs
  extra.

It's Jiten's site, so the switch is his call.

## Open items

| # | Item | Owner |
| --- | --- | --- |
| 1 | Copy review. The page is built from approved documents, but it's new public text. | Jiten |
| 2 | Hosting decision and DNS change (above). | Jiten |
| 3 | Confirm <jiten@grade-robotics.com> as the public contact. | Jiten |
| 4 | Social-share (Open Graph) image for link previews: not made yet. | When going live |
| 5 | `--orange-ui` `#C74A17` is a slightly darker Grade Orange. It's used behind white text on buttons and the CTA block, because brand `#D9521A` with white text is 4.1:1, below the 4.5:1 accessibility minimum. Add it to the brand guide if kept. | Yash |
| 6 | Logo lettering is still the traced, unlicensed version (brand guide open item #1). It's the same file used everywhere else. | Yash |

## Related

- [marketing/content/website-hero-video-fleet-os.md](../../marketing/content/website-hero-video-fleet-os.md): Site Bible, negative prompt and footage rules
- [materials/press-kits/grade-robotics-logo/BRAND-GUIDE.md](../../materials/press-kits/grade-robotics-logo/BRAND-GUIDE.md): colours and logo files
- [materials/one-pagers/company-one-pager/CHANGELOG.md](../../materials/one-pagers/company-one-pager/CHANGELOG.md): source of the product copy
- [WIKI.md](../../WIKI.md): org-wide index

## Log

- 2026-09-21: v1 built. Static one-pager with a canvas hero (GRADE behind the machines) and a scroll-driven fleet journey through six terrains, per Yash. Optional slot for a Seedance film. Checked at phone width in the preview pane and at 1440 px with headless Chrome.
