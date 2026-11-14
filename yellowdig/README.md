# Yellowdig Triage

An AI agent skill that catches you up on your Yellowdig course community. It reads the discussion through Yellowdig's data API, picks out the posts that actually need you, and gives you a single HTML dashboard with a draft reply for each one.

It is read-only. It never posts, comments, or reacts in Yellowdig. You decide what to send.

## What you get

- **Six summary tiles:** students, active students, posts, comments, reactions, and how many items are flagged for you.
- **Three short lists of posts with no staff reply yet:**
  - posts that tag you or reply to your own posts;
  - posts worth a reply (strong posts nobody noticed, or a content slip worth a light correction);
  - blockers and time-sensitive items (broken links, access problems, group logistics, deadlines).
- **A draft reply for every flagged post**, with a Copy button and a link to the post.
- **A searchable table of every post in the window**, with an answered/open badge. Optionally, it also drafts one-to-two-sentence replies for the newest 50 unanswered posts.

The dashboard is one self-contained HTML file. It works on a phone and in light or dark mode.

## Requirements

- An **owner or facilitator** role in the Yellowdig community.
- A **Yellowdig API key**. To get one:
  1. In Yellowdig, click **User Options** in the bottom-left corner.
  2. Choose **Account Settings**, then **API Keys**.
  3. Click **Generate Key** and copy the key.

  Keys expire, so you'll generate a fresh one now and then.
- An AI agent that supports skills (`SKILL.md` folders) and can make web requests. Claude Code, Claude.ai, and Codex all work. In practice the agent usually needs a browser, because Yellowdig's data host often blocks direct server-side requests.

## Install

The skill lives in the `yellowdig` folder of this repository. Clone the repository first:

```bash
git clone https://github.com/bcelen/skills.git
```

Then copy the `yellowdig` folder to where your agent looks for skills.

**Claude Code**

```bash
cp -r skills/yellowdig ~/.claude/skills/
```

**Codex**

```bash
cp -r skills/yellowdig ~/.agents/skills/
```

**Claude.ai (web, desktop, mobile)**

Zip the folder and upload the zip in the Skills section of your Claude settings.

```bash
cd skills && zip -r yellowdig.zip yellowdig -x '*.DS_Store'
```

**Other agents**

Point the agent at `SKILL.md`, or paste its contents into the conversation. The skill names no host-specific tools, so any agent with web access and file output can follow it.

## Use

Ask your agent something like:

> Catch me up on my Yellowdig course.

> Run a Yellowdig triage.

**First run.** The agent asks for:

1. **The community URL.** Copy it from your browser while the community is open. It looks like `https://yellowdig-au.app/n/<network>/community/<id>`.
2. **Your API key.**
3. **Your timezone**, for post times.
4. **A voice profile** (optional). If you have a writing-voice skill, name it and the drafts will be written in it. Otherwise they use a clear, warm lecturer tone.
5. **Where to start.** The default is the last 7 days. It also scans the whole term once for older posts that tag you or report a problem and still have no reply.

**Later runs.** The agent remembers your settings and asks only for the API key. It proposes starting where the last report ended, so there is no gap and no overlap.

If nothing new has been posted since the last run, it tells you so and skips the dashboard.

## Privacy and safety

- **Read-only.** It never writes to Yellowdig.
- **Your API key is never saved.** The agent asks for it each run and does not store or repeat it.
- **No student emails or contact details** appear in the report.
- **Nothing is sent automatically.** Every draft is a suggestion for you to edit and post yourself.
- Between runs, the agent keeps a small settings file per community (network, timezone, instructor and facilitator names, last cutoff date, and a short run log). It contains no API key and no student data.

## Notes

- **AU and US regions both work.** The agent picks the right data host from your community URL. A key only works on its own region's host.
- **If the key is rejected**, it has most likely expired. Generate a new one and run again.
- This is an independent project. It is not affiliated with or endorsed by Yellowdig.

## License

[MIT](LICENSE) © 2026 Boğaçhan Çelen
