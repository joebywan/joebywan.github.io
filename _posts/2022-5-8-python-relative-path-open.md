---
layout: post
title: Python open relative path
---

I know my previous posts have been very technique related rather than technical, this is a warning, tech stuff inside :)

Working on a python script today and have been playing with different ways of storing data, but to do that I need the open() function to work.

Relative paths have been a thing for as long as I can remember, my quick snoop around online can't even find if they were introduced seperately to absolute paths.

So it confuses the hell outa me when the open() function for python can't use a relative path.

Looking around to work around it, I've had to import an extra library, and hack the script we're running's current directory onto the listfile.

```python
import os

currentDirectory = os.path.dirname(__file__)
with open (os.path.join(currentDirectory, './instancesList.json'), 'r') as f:
  data = json.load(f)
```

Rather frustrating and wasteful in my opinion.  That said, if there is an easier way, let me know.  I'd love to see it!