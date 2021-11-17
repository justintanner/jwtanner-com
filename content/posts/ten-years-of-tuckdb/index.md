---
title: Ten years of TuckDB
description: Thoughts on the last 10 years of TuckDB and our future plans.
author: Justin Tanner
date: "2020-05-05"
---

In 2010 we launched a website called [TuckDB][1], a database of antique postcards published by Raphael Tuck & Sons. Since then we've uploaded hundreds of thousands of images, expanded our database of postcards and launched a new website [TuckDB Ephemera][2]. We thought that now was a good time to reflect on the last 10 years and discuss our future plans.

TuckDB started as a Microsoft Access database born from the collaboration between two collectors, Richard Moulton and Allan Braun. As the MS Access database was reaching maturity, we considered publishing it as a book (as many collectors do), but instead decided to create a website.

{{< bootstrap_figure src="ms_access.jpg" alt="Screenshot of MS Access" >}}The original MS Access database{{< /bootstrap_figure >}}

Initially the website listed about 100,000 postcards without any images. After launching we experimented by scanning and uploading 100 images to see if they improved the database.
The images looked great, they made the website. The next problem was how would we scan over 100,000 postcards and match them to the existing database records?

{{< bootstrap_figure src="play.jpg" alt="Side-by-side of different overprints" >}}
Two postcards with different overprints taken from <a class="text-muted" href="https://tuckdbpostcards.org/items/69848">PLAY</a>
{{< /bootstrap_figure >}}

We started by scanning with a regular flatbed scanner. Scanning one side at a time, cropping the image and uploading it to the website. It was slow and error prone. After trying several different scanners we found the Fujitsu ScanSnap. The ScanSnap had several advantages over a flatbed: it scanned both the front and back of a postcard at the same time and it automatically cropped the images.

{{< bootstrap_youtube id="EdNP9e2et5w" >}}
Scanning an antique postcard with a Fujitsu Snap Scan
{{< /bootstrap_youtube >}}

The ScanSnap sped up the process of scanning images, but the task integrating those images into the existing collection was larger. To help, we hired several diligent editors and slowly worked our way through all 100,000 postcards.

After about 5 years of adding, re-organizing and uploading images of postcards we had over 300,000 beautiful images. The database was now the most complete re-creation of the lost records of Raphael Tuck & Sons postcards to date.

{{< bootstrap_figure src="richard.jpg" alt="A photo of Richard Moulton" >}}
Richard Moulton working on the collection.
{{< /bootstrap_figure >}}

{{< bootstrap_figure src="entering.jpg" alt="Desks organizing photos" >}}
Three workstations for scanning and re-organizing postcards.
{{< /bootstrap_figure >}}

Meanwhile, we created a sister website to the postcard website, TuckDB Ephemera. Tuck seems to have been involved in almost anything made from paper ranging from jigsaw puzzles, a vast library of books, greeting cards and more. Richard Moulton's daughter Alison Milling was working on her own collection of Tuck ephemera. Again we started by converting the MS Access database into a website. We are currently busy editing and uploading the second collection.

{{< bootstrap_figure src="bear.jpg" alt="Mechanical Bear" >}}
A "mechanical" from TuckDB Ephemera <a class="text-muted" href="https://tuckdbephemera.org/items/28473">OFF TO THE PARTY</a>
{{< /bootstrap_figure >}}

10 years ago when started this project, we looked for other people publishing their collectibles online, but sadly there wasn't an easy solution. Most collectors were using: MS Access, MS Excel, WordPress or other collector specific apps. So we built TuckDB as a custom software project from scratch and learned a lot along the way.

10 years later many people around the world have used our images (which are royalty free) on their blogs, books and YouTube videos. We are thrilled by all the positive feedback and we'd like to thank those that have contacted us providing missing images and information.

{{< bootstrap_figure src="tuck_house.jpg" alt="Picture of a red barn" >}}
The "Tuck House" - home of physical Tuck collections.
{{< /bootstrap_figure >}}

Looking towards the future we ask the question, how do you make a database of collectibles last forever? Not everyone has the resources to build and host a custom website. Many collectors have contacted us asking how they could publish their collection in a similar way.

We don't yet have a good answer to that question, but if you're interested in such a project please tell us what you think on our [forum][3].
And finally a big thank you to all the editors of TuckDB and people across the world who have contacted us over the years.

Happy collecting.

{{< bootstrap_figure src="sheep_station.jpg" alt="Sheep shearers posing for a photo" >}}
An Australian Sheep Station "<a class="text-muted" href="https://tuckdbpostcards.org/items/69687">Sheep Shearers</a>"
{{< /bootstrap_figure >}}

Furthur reading:

* [A list of people who contributed to TuckDB over the years][4]
* [How TuckDB came together][5]
* [History of Raphael Tuck & Sons][6]

[1]: https://tuckdbpostcards.org
[2]: https://tuckdbephemera.org
[3]: https://forum.tuckdb.org
[4]: https://tuckdbpostcards.org/credits
[5]: https://tuckdbpostcards.org/start
[6]: https://tuckdbpostcards.org/history
