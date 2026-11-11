# Transcript layouts

The shared library of **transcript layouts** for the Transcript App: a program that turns academic transcript PDFs
into digital credentials (1EdTech CLR 2.0) on the user's own computer.

## What's here

- **`layouts/`**: one file per layout. A layout teaches the app to read one school's transcript format (where the
  terms, courses, grades and totals are, and what the school's grades mean) without using AI. Once a layout is
  here, transcripts in that format are converted for free, on every computer.
- **`index.json`**: the list of published layouts. For each one it holds a fingerprint (SHA-256) of the file, so
  the app can check that a download arrived intact, and a fingerprint of its content.
- **`index.json.sig`**: the maintainers' digital signature of `index.json`. The app has the maintainers' public keys
  built in and ignores any list that isn't signed with one of their private keys, or that was changed after
  signing. Because the list holds every layout's fingerprint, the signature covers the layouts too. So even if
  someone changed this repository, the app wouldn't accept their layouts.

## What's **not** here

- **No student information.** Layouts describe a format, never a person: no names, IDs, grades or transcripts. The
  app checks for student data before it saves any layout.
- **No program code.** The app itself is kept elsewhere.

## How the app uses it

Every time the app opens, it reads `index.json` and downloads any layout that's new or has changed. It checks
each download against its fingerprint, and checks that it's a valid layout, before using it. A layout removed from
the library is removed from the app the next time it opens. Without an internet connection, the app keeps the
layouts it already has. The app sends nothing about students: only the requests for these files.

## How layouts get here

Layouts are added with the app's `library/publish.py` script, which checks each one automatically first: it must be
a valid layout, and contain nothing that looks like personal data. On the computer that learned it, the app has
already checked it against that school's transcripts (it must read them exactly as the AI did, and contain none of
their student information). Later, layouts learned on users' computers will be sent here automatically, through a
small submission service. There's no need to send layouts here yourself.

## License

To be decided.
