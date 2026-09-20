---
title: 'Bili Reupload'
date: 2026-09-20
type: landing

sections:
  - block: markdown
    content:
      title: Bili Reupload
      text: |-
        A personal command-line tool used by the owner of this site to mirror
        videos from Bilibili to his own YouTube channel.

        ### What it does

        The tool downloads videos from a Bilibili playlist that the owner has
        been given permission to republish, then uploads them to a single
        YouTube channel that the owner controls. Titles, descriptions and
        thumbnails are carried over from the source, and every description
        credits the original creator and links back to the original video.

        ### Who can use it

        Nobody else. This is a single-user script that runs on the owner's own
        computer. There is no website, no sign-up, no hosted service, and no
        way for another person to connect an account to it.

        ### Google API usage

        The tool uses the YouTube Data API v3 with a single scope,
        `https://www.googleapis.com/auth/youtube.upload`, which it uses only to
        upload videos to the owner's own channel. It does not read, modify or
        delete anything else.

        ### Links

        - [Privacy policy](/bili-reupload-privacy/)
        - [Terms of service](/bili-reupload-terms/)
        - Contact: lilaobarenzhi888@gmail.com
    design:
      columns: '1'
---
