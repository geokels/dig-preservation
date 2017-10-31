---
layout: page
title: Setup
permalink: /setup/
---

Please see [this section of the workshop template][workshop-setup]
for instructions on installing Git.

First we'll download our sample data that we will be working with in the class:

1. Open your browser
2. Paste in the following link and hit Enter/Return: <https://github.com/ucsdlib/cats-as-data/archive/master.zip>
3. Navigate to the downloaded zip file. Most likely, it is in your Downloads folder.
4. Move the zip file to your Desktop (drag and drop should work).
5. Extract the contents of the zip file. This can usually be done by right-clicking on the file and clicking the "Extract
   here" or "Extract all" option.
6. Right-click on the `cats-as-data-master` folder and rename it to `cats-as-data`.

We'll do our work in the `cats-as-data` folder. So please open your terminal and change your working directory to it with:

~~~
$ cd
$ cd Desktop
$ cd cats-as-data
~~~
{: .bash}

The `cats-as-data` folder should contain `README.md`,
`cats-human-situations.csv`, and an `images` directory.  Depending on how you
unzipped the files, these might all be inside another folder called
`cats-as-data-master`.  If this happens to you, just `cd` once more into that
inner folder: `cd cats-as-data-master`.

[workshop-setup]: https://swcarpentry.github.io/workshop-template/#git
