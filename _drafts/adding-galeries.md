# How to add gallery

Terrible script for reading image names into gallery format

``` python
from os import listdir

for filename in list(filter(lambda x : "." in x, listdir())):
    print("  - relative_path: " + filename)
    file_name_parts = filename.split(".")
    thumbnail_filename = file_name_parts[0] + "_tn.jpg"
    print("    relative_path_thumbnail: thumbnails/" + thumbnail_filename)
    print("    text: lorem impsum")
```
