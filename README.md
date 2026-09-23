# NASP journal finder

**<https://jesperalvarsson.github.io/nasp-journal-finder/>**

Paste an abstract, a title, or a handful of keywords, and the page ranks the
journals of the **Karolinska Institutet Journal List (KI-JL) 2026** by how well
they match - with each journal's KI level, the kinds of article it actually
publishes, and a link to the journal itself.

It is meant for research on suicide, self-harm and mental ill-health, the field
of **NASP**, the National Centre for Suicide Research and Prevention of Mental
Ill-Health at Karolinska Institutet. Of the 6,859 journals on the KI-JL, 1,605
are classified relevant to that field - 1,413 of them at a KI level that scores
points, which is the figure shown at the top of the page. The rest stay
searchable, so an out-of-scope idea still gets an answer.

This is a personal project, published so colleagues can try it. It is not a KI
product, it is not maintained by KI, and nothing in it is an official position
of Karolinska Institutet.

## Why it exists

The problem it addresses is a habit, not a lookup: good work goes to journals
below what it warrants, because the shortlist of candidates is the shortlist of
places we have published before. So relevance here is computed from **what the
research is about**, never from where it has appeared. The topic model is built
from the titles, MeSH terms, keywords and abstracts of 149 NASP papers from
2021-2026; publication venues are read exactly once, at the very end, to check
how much of the known-good set the result still reaches. A model that learned
from venues would only ever recommend more of the same.

Each journal also carries its five-year **article-type mix** - trials, reviews,
observational studies, case reports, guidelines, editorials - so a study can be
shaped to fit a target journal's tradition before it is written rather than
after a desk rejection.

## Using it

Nothing to install and nothing to sign in to. Open the link, paste text into the
query box, and read down the ranking.

- **Filter by KI level** to see only the level 2 and 3 options. That is the
  point of the exercise.
- **Click a journal title** to open the journal. 6,145 of the links were
  verified by fetching them; where none was found the title opens the ISSN
  register record instead, and you can set your own address by expanding the
  row. Your links stay in your own browser and are not shared.
- **Save the page** (Ctrl+S) to use it offline. It keeps working with no
  network; only the wider model below needs one.

The page carries a 1,728-journal text model inside it and fetches the full
6,403-journal model (`profiles.json`, about 8 MB over the wire) in the
background a second or two after it opens. Until that lands the built-in model
answers, so there is nothing to wait for; when it arrives the ranking widens and
the page says so in a notice. Opened from a downloaded copy the fetch is blocked
by the browser's origin rules, so drag `profiles.json` onto the page instead if
you want the wide model offline.

The site carries a `robots.txt` that asks search engines to stay away, so it is
shareable by link but not findable. Remove that file when it should be.

`harvester.html` is the maintenance tool that rebuilds journal profiles from
PubMed. It is here for completeness; testers do not need it.

## Trying it, and telling me what is wrong

What would help most:

1. Paste the abstract of a paper you have already published, and see whether its
   actual journal appears - and what appears above it.
2. Paste something you are working on now, and see whether the level 2 and 3
   suggestions are plausible places to send it or obvious nonsense.
3. Tell me about journals you expected and did not get. A missing journal is
   more informative than a wrongly ordered one.

Open an issue, or send me the query and what you expected.

## How it was built, and what is still wrong

The build scripts, the test harnesses and an honest account of the limits live
in the working repository, which is private because it contains the KI-JL
spreadsheets. Ask if you would like access.

In short: 6,403 of the 6,859 journals have a text profile built from up to 300
of their PubMed articles from the last five years. A journal enters the relevant
set by a title rule (551), by topical coverage alone (508), or by both (546) -
that middle number is the point of the exercise, 508 journals no keyword list
would have named. 232 of the relevant journals sit at KI level 2 or 3.

Two limits worth knowing before trusting a ranking. Journal links were checked
by fetching them once, which proves an address answers, not that it answers for
that journal. And the topic model is built from 149 papers, so topics resting on
a handful of them are visibly noisier than the large ones - the topic list on
the page shows how many journals each one reaches, which is the tell. A topic
that mostly reaches journals the subject rules cannot place at all is treating a
method as a subject, and is held back from widening the list; it says so in the
topic panel, and can still be ticked deliberately.

## Rebuilding this site

    python3 build_site.py --repo nasp-journal-finder
