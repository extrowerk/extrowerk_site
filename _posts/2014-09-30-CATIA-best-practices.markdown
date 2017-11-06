---
layout:     post
title:      "CATIA V5 Best Practices"
subtitle:   ""
date:       2014-09-30 12:01:00
author:     "z"
header-img: "img/dont.jpg"
---

<p>Our teacher told us many years ago:</p>
> "We bought the whole saw blade, so use not just the middle but the entire length!"

<p>Ok, You or your company bought the whole CATIA, then why don't use it 100%? Because there are some features, facilities, functions what makes your model harder to understand, uncomfortable to change, and they can make Your model unstable.</p>

* Only sketch on planes (not planes that are created with any reference to the solid or surface) not on a face or a surface. This will make your model more stable if you would like to make changes to it.
* Use positioned sketches with origin and orientation.
* Always fully constrain sketches (sketches should be green or yellow - never white)
* Always publish elements that will be linked to child parts, and only make external references to published elements
* Keep sketch as simple as possible to make modifications easily.
* Keep your tree well organized and take the time to rename the elements to something that makes sense and to categorize them into Geometrical Sets. It will make it easy on anyone else that uses your file.
* Give a lot of thought to applying relationships and constraints. They can cause you, or whoever has to update/modify your file, a lot of grief if not carefully created.
* For pocket use the pocket feature, don't use boolean for this.
* Don't use Remove Face, Replace Face for fun, CAD is serious business.
* Don't use CCP links in Assembly. Use Context Links.
* Work with cache if you have large assemblies.
* Avoid links between parts in an assembly. Use skeleton parts if you want to use linked parameters and geometry.

<p>...to be continued, stay tuned...</p>