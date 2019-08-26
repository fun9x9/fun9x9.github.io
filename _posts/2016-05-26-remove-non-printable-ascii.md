---
title: "특수문자 삭제 (how remove non printable ascii characters file unix)"
date: 2016-05-26T16:11:00+09:00
categories:
  - IT
tags:
  - sh
  - shell
  - script
  - Unix
  - Linux
  - character
  - tr
---

```sh
tr -cd '\11\12\15\40-\176' < file-with-binary-chars > clean-file
```

- octal 11: tab 
- octal 12: linefeed 
- octal 15: carriage return 
- octal 40 through 
- octal 176: all the "good" keyboard characters 

출처 : [http://alvinalexander.com/blog/post/linux-unix/how-remove-non-printable-ascii-characters-file-unix](http://alvinalexander.com/blog/post/linux-unix/how-remove-non-printable-ascii-characters-file-unix)

(http://alvinalexander.com/blog/post/linux-unix/how-remove-non-printable-ascii-characters-file-unix)
