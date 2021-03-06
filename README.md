# github-issues-importer

Import Github issues from various sources (CSV, Excel, etc.)

## Why!?

Lots of things are great to do in public. Submitting CFPs isn't one of them. **WHY THE $^#% NOT?!?!** Knowing that your talk proposal is going to get put under a microscope in public is daunting for a lot of people (especially those who have never spoken before).

Having all your CFP submissions be Github issues, however, **is awesome.** It's just a huge pain to import them. Wait? This thing does that for me you say? Well WHY DIDN'T YOU TELL ME THAT!

It is designed to be used with a simple Google Form submission used by such awesome conferences as [JSConf](http://jsconf.us) and [JSConf.eu](http://2014.jsconf.eu/call-for-speakers/)

## Usage

```
npm install import-github-issues -g
import-github-issues --file ./cfp-responses.tsv --auth 'user:pass' --repo 'my-conf/cfp-review-2014'
Parsing | ./cfp-responses.tsv
Reading issues | my-conf/cfp-review-2014
Already exists | ZOMG JAVASCRIPT
Already exists | JIFASNIF
Already exists | WHY CANT WE DO ALL THE THINGS
Creating | SOME OTHER NEW TALK
Creating | THAT MEANS THIS IS IDEMPOTENT
```

_n.b. the script is designed to be idempotent so you can run it multiple times and everything will be fine._

## Handling Newlines in Large Text Fields

So hey, you know what's fun? Preserving formatting! It turns out that formats like `*.tsv` (tab-separated values) do not play nice with new lines within individual entries. So that means if you export to `*.tsv` from something like say Google Forms then it won't work.

**But wait! There's a fix!** All you have to do is export to an `*.xlsx` file and use that with the `cfp-github-importer`!

```
import-github-issues --file ./cfp-responses.xlsx --auth 'user:pass' --repo 'my-conf/cfp-review-2014'
Parsing | ./cfp-responses.xlsx
```

## Parsing and Format

This was originally designed (and defaults to) the [CFP format used by EmpireJS 2014][empirejs-cfp-2014]:

| Date | Name | Email | Twitter | Title | Description | Difficulty (1-5) | Anything Else? |
|:----:|:----:|:-----:|:-------:|:-----:|:-----------:|:----------------:|:--------------:|

If you would like to use a custom CFP format and template that's easy! Just use the following options:

```
  import-github-issues -t ./path/to/template.md -p ./path/to/parser.js
```

Where the `parser.js` would look something like:

``` js
module.exports = function (columns) {
  return {
    'author': [
      parts[1],
      parts[2],
      parts[3]
    ].filter(Boolean).join('\n'),
    'title': parts[4],
    'description': parts[5],
    'audience': parts[6],
    'anything-else': parts[7] || ''
  }
};
```

The above matches the default which has a corresponding `issue.md` template under `/templates`:

```
<author>

### What's your Ideal Audience? <audience>
_Beginner (1) to Advanced (5)_

## Tell Us More

<description>

## Questions/Comments/You Oughta Know/Anything else? Feel free to put the kitchen sink in here.

<anything-else>
```

#### LICENSE: MIT

[empirejs-cfp-2014]: https://docs.google.com/forms/d/1h0EEcAG67zB8RA7kJZAVYW9u95ZDch1hZBxIogNMRaw/
