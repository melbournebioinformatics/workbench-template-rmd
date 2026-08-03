# Melbourne Bioinformatics Workbench Lesson Template (R Markdown)

The University of Melbourne fork of the [Carpentries Workbench][workbench] R Markdown lesson
template. Use it to start a new MB training workshop whose episodes **execute code at build
time**, so outputs, tables and plots are generated from the source rather than pasted in.

It differs from the upstream Carpentries template in two ways. It pins our theme fork,
[`uom-varnish`][uom-varnish], in `config.yaml`:

```yaml
varnish: 'melbournebioinformatics/uom-varnish@main'
url: 'melbournebioinformatics.github.io/uom-varnish'
```

and it adds UoM institute codes to `carpentry:`, which selects the branding on the built site:
`uom` (University of Melbourne), `mb` (Melbourne Bioinformatics), `mig` (Melbourne Integrative
Genomics), `wehi` (Walter and Eliza Hall Institute), `abacbs` (Australian Bioinformatics and
Computational Biology Society). Keep both settings when you adapt this template.

Full contributor documentation lives in the [MB tutorials wiki][wiki].

## Which template do I want?

| | Use when |
|---|---|
| [**`workbench-template-md`**][md] | Episodes are prose, screenshots and fenced code blocks that are *shown*, not run. Command-line tutorials, GUI walkthroughs, conceptual material. |
| **`workbench-template-rmd`** (this one) | Episodes must *execute* code at build time. R, or Python via `reticulate`. |

Only pick this one if you actually need code to run. R Markdown lessons carry a pinned dependency
set that has to be maintained, and every build has to install and run it.

## Note about lesson life cycle stage

Although `config.yaml` states the life cycle stage as pre-alpha, **the template is stable and
ready to use**. The life cycle stage is preset to `"pre-alpha"` because that is the right setting
for a brand new lesson.

## Create a new lesson from this template

Click **Use this template** at the top right of
[the repository page](https://github.com/melbournebioinformatics/workbench-template-rmd), then
**Create a new repository**.

Name the repository in lowercase with dashes separating words, and make the name say what the
lesson is about plus either its focus or its technical level. `intro-to-r` is topic plus level;
`rna-seq-counts-to-genes` is topic plus scope. The name becomes the published URL, so it is worth
a minute of thought. A new lesson can start private and be made public later.

## Configure the new lesson

1. **Enable GitHub Pages.** _Settings_ → _Pages_, build from the `gh-pages` branch. That branch
   appears once the first build workflow has run, so check _Actions_ if it is not there yet.
2. **Fill in `config.yaml`.** Every field marked `# FIXME`: `title`, `carpentry_description`,
   `created`, `keywords`, `contact`, `source`, and `life_cycle` (`pre-alpha` → `beta` once the
   lesson is usable). Set `carpentry:` to your institute code, and list your episode files in
   teaching order under `episodes:`. **Leave the `varnish:` and `url:` lines alone.**
3. **Rename `FIXME.Rproj`** to match the repository name.
4. **Annotate the repository.** On the landing page, click the cog next to _About_, tick "Use your
   GitHub Pages website", and add topic tags: `lesson`, the life cycle stage, and the language.
5. **Adjust `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md` and `LICENSE.md`** for your project.
6. **Replace this README** with a description of your lesson, and delete these instructions.

## Build and preview locally

Requires R and pandoc. One-time setup on your machine:

```r
install.packages("pak")

options(repos = c(
  carpentries = "https://carpentries.r-universe.dev",
  CRAN        = "https://cloud.r-project.org"
))

pak::pak(c("sandpaper", "pegboard", "tinkr"))
pak::pak("melbournebioinformatics/uom-varnish")   # installs under the package name `varnish`

sandpaper::use_package_cache(prompt = FALSE)
```

We use `pak` rather than `devtools`, which has been split up and superseded. Then, from inside
the lesson repository:

```r
sandpaper::serve()          # live-reload preview on http://127.0.0.1:4321
sandpaper::build_lesson()   # one-off build into site/
sandpaper::check_lesson()   # structure and link validation
```

## Dependencies

Two separate sets of packages, and mixing them up is the single most common problem people hit:

|  | Lives in | Contains | Installed by |
|---|---|---|---|
| **Build toolchain** | your normal user library | `sandpaper`, `pegboard`, `tinkr`, `varnish` | you, with the `pak` block above |
| **Lesson dependencies** | `renv/profiles/lesson-requirements/` in this repo | whatever the `.Rmd` episodes `library()` | `sandpaper::manage_deps()`, automatically |

sandpaper does not make itself the project's renv environment. It loads the `lesson-requirements`
renv profile only while rendering episodes, which is why your interactive session can still find
sandpaper.

Adding a package to an episode is therefore just: `library(thepackage)`, rebuild, and **commit the
changed `renv/profiles/lesson-requirements/renv.lock`**. CI decides whether the lesson has pinned
dependencies purely by whether that file is in the repository, so if it is not committed the
lesson builds against whatever is current on the day.

> ⚠️ **Never run `renv::init()` or `renv::activate()` in a lesson repository.** Both write a root
> `.Rprofile` that hijacks every R session in the repo into the lesson library, at which point
> `sandpaper` appears to vanish. If that has already happened, delete `.Rprofile` and restart R.

The full explanation, including troubleshooting, is in [Renv and dependencies][renv-wiki].

## Python episodes

`episodes/python-with-reticulate.Rmd` is a working example. Declare Python dependencies in the
episode's setup chunk and `reticulate` provisions an ephemeral environment with `uv`, fetching
both the interpreter and the packages:

````
```{r setup, include=FALSE}
Sys.setenv(RETICULATE_PYTHON = "managed")
library(reticulate)
py_require(c("pandas", "matplotlib"), python_version = "3.12", exclude_newer = "2026-08-01")
```
````

Nothing needs adding to the CI workflows for this to work. Delete the example episode, and its
entry in `config.yaml`, if your lesson is R only.

## Writing episodes

Episodes live in `episodes/*.Rmd`, one per major section, 15 to 45 minutes each and never longer
than an hour. Every episode needs `title`, `teaching` and `exercises` in its frontmatter, plus
`questions` and `objectives` blocks at the top and a `keypoints` block at the end.
`episodes/introduction.Rmd` demonstrates every available block.

Callouts, challenges and solutions use Carpentries fenced divs. **Fence depth must match exactly
between the opening and closing markers**, or pegboard fails and the block renders as plain text.

- [Sandpaper documentation](https://carpentries.github.io/sandpaper-docs/episodes.html)
- [Component style guide](https://carpentries.github.io/sandpaper-docs/component-guide.html)
- [How the Workbench works](https://carpentries.github.io/workbench/workflow-guide.html)

[workbench]: https://carpentries.github.io/sandpaper-docs/
[uom-varnish]: https://github.com/melbournebioinformatics/uom-varnish
[md]: https://github.com/melbournebioinformatics/workbench-template-md
[wiki]: https://github.com/melbournebioinformatics/melbournebioinformatics.github.io/wiki
[renv-wiki]: https://github.com/melbournebioinformatics/melbournebioinformatics.github.io/wiki/Renv-and-dependencies
