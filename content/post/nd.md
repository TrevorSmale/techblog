+++
author = "Hugo Authors"
title = "That Unix file is not deleted 👺"
date = "2024-09-01"
description = "Removing a file is not as it seems"
tags = [
  "Tech", "Linux", "Unix", "Administration", "Engineering", "TidBits",
]
categories = [
    "TidBits",
]
series = ["TidBits"]

+++

Removing a file in Unux does not actually remove it from memory. You are only reducing the number of hardlinks pointing to the file to '0'.
In order to actually delete the information is to overwrite it. This will happen over time as the disk fills, or if one would like to delete a file in a timely fashion should over write empty drive sectors.