# PeanutBase Jekyll site

### Links

[PeanutBase](https://www.peanutbase.org) - on pb-jekyll-vm

[PeanutBase (development)](https://dev.peanutbase.org) - on dev-pb-jekyll

[GitHub repository](https://github.com/PeanutBase/jekyll-peanutbase)

### Notes

On dev-pb-jekyll, the file `germplasm/js/lightbox.js` is mine,
it was an attempt to use the arrow keys to scroll between Germplasm images.

### Contact page

You set the submit form id and spam protection information in `contact/index.html`.

We use [Formspark](https://formspark.io) as the submit action service. It has a limit of 250 submissions, we have 131 remaining (as of 26 Sep 2024).

The recaptcha does not seem very effective at preventing spam (though maybe we only see the 119 it allowed through, and not the thousands it blocked?).
One alternative we considered was [BotPoison](https://botpoison.com).

### To do

Synchronize with [jekyll-starter-legumeinfo](https://github.com/legumeinfo/jekyll-starter-legumeinfo) ?
