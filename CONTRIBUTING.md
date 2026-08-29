# Contributing to AWS Community Day CEE 2026

Thanks for helping out! This repository holds the session and speaker data that powers [AWS Community Day CEE 2026](https://awscommunity.eu) — Thursday, September 17, 2026, in Budapest, Hungary. Contributions here are almost always **data changes** (adding or updating a speaker profile, photo, or session), not application code, so this guide is written with that in mind.

## Repository structure

```
sessions/
  main-stage/        # COM### — Main Stage talks and keynotes
  breakout-stage/     # BRK### — Breakout Stage talks
  workshops/           # WKS### — Hands-on workshops
speakers/
  profiles/           # <speaker-id>.json — one file per speaker
  images/             # <speaker-id>.jpg  — one headshot per speaker
LICENSE
CONTRIBUTING.md
README.md
```

## Ways to contribute

- **Add or update a speaker profile** (`speakers/profiles/`)
- **Add a speaker photo** (`speakers/images/`)
- **Add or update a session** (`sessions/main-stage/`, `sessions/breakout-stage/`, or `sessions/workshops/`)
- **Fix errors** in existing data (typos, wrong times, broken links, outdated bios, speaker changes/visa issues, etc.)
- **Improve the README** or this guide
- **Report an issue** if you spot something wrong but aren't sure how to fix it

If you're not sure whether a change belongs in this repo (e.g. website copy, ticketing, sponsorship), please open an issue.

## Getting started

1. **Fork** this repository.
2. **Clone** your fork locally:
   ```bash
   git clone https://github.com/<your-username>/aws-community-day-cee-2026.git
   cd aws-community-day-cee-2026
   ```
3. Create a new branch for your change:
   ```bash
   git checkout -b add-speaker-jane-doe
   ```
4. Make your change (see the guidelines below).
5. Validate that your JSON is well-formed (see [Validating your changes](#validating-your-changes)).
6. Commit, push, and open a pull request against `main`.

## Adding or updating a speaker

Speaker profiles live in `speakers/profiles/` as one JSON file per speaker, named with the speaker's slug (e.g. `jane-doe.json`). Base your file on an existing profile to match the schema:

```json
{
  "id": "jane-doe",
  "name": "Jane Doe",
  "title": "Senior Solutions Architect",
  "company": "Example Corp",
  "country": "Hungary",
  "bio": "A few sentences about the speaker's role, expertise, and background.",
  "linkedin": "https://www.linkedin.com/in/jane-doe/",
  "sessions": [
    "BRK006"
  ],
  "image": "speakers/images/jane-doe.jpg",
  "aws_programs": {
    "aws_employee": false,
    "aws_hero": false,
    "aws_community_builder": false,
    "aws_user_group_leader": false,
    "aws_new_voices": false
  }
}
```

Guidelines:

- **`id`** must match the filename (without `.json`) and be unique, lowercase, hyphen-separated.
- **`bio`** should be third-person, factual, and free of marketing/sales language — this is a community event, not a company pitch.
- **`sessions`** must list the session ID(s) (e.g. `COM004`, `BRK002`, `WKS001`) the speaker is presenting, and each of those session files must list this speaker's `id` back in its `speaker_ids` array. Keep both sides in sync.
- **`aws_programs`** only include the flags that apply — the recognized keys currently in use are `aws_employee`, `aws_hero`, `aws_community_builder`, `aws_user_group_leader`, and `aws_new_voices`. Don't invent new keys without discussing them in an issue first, and only include the ones that are actually `true` for a given speaker (check a few existing profiles for the current convention).
- Only include a `linkedin` field if the speaker has approved sharing that link publicly.

### Adding a speaker photo

- Add the image to `speakers/images/`, named to match the `id` (e.g. `jane-doe.jpg`).
- Use a professional, reasonably high-resolution headshot in `.jpg` format (the existing convention in this repo).
- Make sure you have the speaker's permission to use and publish the photo.
- Reference the exact path (`speakers/images/<id>.jpg`) in the `image` field of the profile JSON.
- If a speaker withdraws or is replaced, remove their photo and profile rather than leaving stale files behind (see "Replacing a speaker" below).

## Adding or updating a session

Sessions are organized by stage/format into three folders, each with its own ID prefix:

| Folder | ID prefix | Format |
|---|---|---|
| `sessions/main-stage/` | `COM###` | Main Stage talks & keynotes |
| `sessions/breakout-stage/` | `BRK###` | Breakout Stage talks |
| `sessions/workshops/` | `WKS###` | Hands-on workshops |

Each session is one JSON file, e.g. `sessions/main-stage/COM004.json`:

```json
{
  "id": "COM004",
  "title": "Session title",
  "stage": "main-stage",
  "start_time": "11:50",
  "end_time": "12:20",
  "duration_minutes": 30,
  "speaker_ids": [
    "jane-doe"
  ],
  "abstract": "Full session description/abstract.",
  "language": "English",
  "audience_capacity": 200,
  "room": "Üvegterem",
  "event_info": {
    "event_name": "AWS Community Day CEE",
    "event_date": "2026-09-17",
    "location": "Kristály Színtér, Budapest, Hungary"
  }
}
```

Guidelines:

- **`id`** must match the filename (without `.json`) and follow the prefix convention for its folder (`COM###`, `BRK###`, or `WKS###`).
- **`stage`** should match the folder (`main-stage`, `breakout-stage`, or `workshops`).
- **`speaker_ids`** must reference existing speaker `id`s in `speakers/profiles/`, and each referenced speaker's profile must list this session's `id` back in its `sessions` array.
- **`start_time`/`end_time`** use 24-hour `HH:MM` format; make sure `duration_minutes` matches the gap between them.
- **`abstract`** should be vendor-neutral and free of sales pitches, consistent with the [AWS Community Day Code of Conduct](https://aws.amazon.com/events/community-day/).
- **`event_info`** should match the values already used across other session files — don't alter the shared event name/date/location unless the event details themselves have officially changed.
- Double-check title, time slot, and stage against the official agenda at [awscommunity.eu](https://awscommunity.eu) before submitting.

### Replacing a speaker (e.g. visa issues, withdrawal)

This happens occasionally and touches multiple files. When replacing a speaker on a session:

1. Update the session's `speaker_ids` to the new speaker's `id`, and update `title`/`abstract` if the talk itself changed too.
2. Add the new speaker's profile (`speakers/profiles/<id>.json`) and photo (`speakers/images/<id>.jpg`) if they're not already in the repo, with the session's ID added to their `sessions` array.
3. Remove the outgoing speaker's session reference. If they have no other sessions in the event, remove their profile and photo entirely rather than leaving orphaned files.
4. Update the schedule table in `README.md` to reflect the change.
5. Mention the reason briefly in your commit message and PR description (e.g. "Replace Main Stage speaker due to visa issue").

## General data-quality guidelines

- **Valid JSON only.** No trailing commas, no comments, use double quotes.
- **UTF-8 encoding** — preserve accented characters (names, cities) as-is rather than transliterating them.
- **No placeholder data.** Don't commit `TODO`, `Lorem ipsum`, or draft content — open a draft PR marked `[WIP]` instead if you want early feedback.
- **Keep speakers and sessions in sync.** Every session's `speaker_ids` and every speaker's `sessions` array should reference each other correctly — this is the most common source of errors.
- **One logical change per PR** where possible (e.g. one new speaker, or one session fix) — this makes review much faster, except for linked changes like a speaker replacement, which naturally spans a few files.
- **Don't remove or rewrite someone else's bio/session copy** without their consent — reach out to the original submitter or an organizer first.

## Validating your changes

Before opening a PR, check that every JSON file you touched is valid. From the repo root:

```bash
# Validate a single file
python3 -m json.tool speakers/profiles/jane-doe.json > /dev/null && echo "Valid JSON"

# Validate every JSON file in the repo
find . -name "*.json" -exec python3 -m json.tool {} \; > /dev/null
```

(Any JSON validator works — `jq`, an editor's built-in linter, or an online validator are all fine too.)

## Commit messages

Keep them short and descriptive, e.g.:

- `Create speaker profile: jane-doe.json`
- `Fix typo in Paul Schwarzenberger bio`
- `Replace speaker for COM004 due to visa issue`

## Pull requests

- Give your PR a clear title describing the change (e.g. "Add speaker: Jane Doe").
- In the description, mention what changed and why (e.g. "New confirmed speaker for the Breakout Stage").
- Link to any relevant issue if one exists.
- A maintainer will review your PR and may ask for small changes before merging. Please be patient — this is a volunteer-run community project.
- By submitting a PR, you confirm you have the right to share any bios, photos, and links you're contributing, and that all sessions/speakers are for AWS Community Day CEE 2026.

## Code of Conduct

This project follows the spirit of the [AWS Community Day Code of Conduct](https://aws.amazon.com/events/community-day/): be respectful, inclusive, and free of harassing, offensive, or discriminatory language in all contributions — including bios, session descriptions, commit messages, and PR discussions.

## License

This project is licensed under the [MIT License](LICENSE). By contributing, you agree that your contributions will be licensed under the same terms.

## Questions?

- **Bugs or data issues**: open a [GitHub Issue](https://github.com/awscommunitycee/aws-community-day-cee-2026/issues)
- **Discussion**: [GitHub Discussions](https://github.com/awscommunitycee/aws-community-day-cee-2026/discussions)

Thanks again for contributing — see you in Budapest! 🇭🇺
