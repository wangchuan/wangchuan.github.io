---
layout: post
title: "Lua and Torch Useful Materials"
category: programming
tags: [lua, torch, code]
use_math: true
---

<!-- add TOC here -->
<div id="genTocHere"></div>

### Lua and Torch Useful Materials

#### File IO
```lua
require 'paths'
require 'torch'

local file = io.open("ans.txt", "w+")
file:write("hello")
file:write("world")
file:write("hello\n")
file:write("world\n")
file:close()
```

#### Return files with specified extension in a directory
```lua
for file in paths.files(dir) do
    if file:find(ext .. '$') then
        print(file)
    end
end

file_table = {}
for f in paths.files(fdname) do
    ext = paths.extname(f)
    if ext == 'jpg' then
        table.insert(file_table, f)
    end
end

local function compare(a, b)
    return tonumber(a:match("%d+")) < tonumber(b:match("%d+"))
end
table.sort(file_table, compare)
```

