
<!-- README.md is generated from README.Rmd. Please edit that file and render with rmarkdown::render("README.Rmd")-->

# joelnitta-home

Source code for [personal website of Joel
Nitta](https://www.joelnitta.com).

Created with [Quarto](https://quarto.org/) in
[R](https://www.r-project.org/).

## Production deployment

Production deployment is set up using GitHub Actions with
[./.github/workflows/publish-cloudflare-pages.yml](./.github/workflows/publish-cloudflare-pages.yml).

On pushes to the `main` branch, this workflow will deploy the site to
Cloudflare Pages.

The workflow file is very simple and basically runs two commands:

1.  `Rscript -e 'babelquarto::render_website()'`, which compiles the
    Quarto blog to a static website within the `_site` folder. This
    folder contains an `index.html` which is the root of the website, as
    well as various JavaScript and CSS files and images; you can read
    more on [`babelquarto`](https://docs.ropensci.org/babelquarto/).
    Note that `_site` is ignored in `.gitignore` because it is generated
    from the source code; the source code is what we track in version
    control, not the built files.

2.  `wrangler pages deploy _site --project-name=joelnitta-home`, which
    deploys the `_site` folder to Cloudflare Pages. You can read more
    details on
    [`wrangler pages deploy`](https://developers.cloudflare.com/workers/wrangler/commands/#deploy-1).

### Secrets

This workflow requires two secrets to be set up in GitHub:

- `CLOUDFLARE_API_TOKEN`: see Cloudflare docs on [how to create an API
  token](https://developers.cloudflare.com/pages/how-to/use-direct-upload-with-continuous-integration/#generate-an-api-token)
- `CLOUDFLARE_ACCOUNT_ID`: see Cloudflare docs on [how to get the
  account
  ID](https://developers.cloudflare.com/pages/how-to/use-direct-upload-with-continuous-integration/#get-project-account-id)

Both secrets must be stored in the settings for the GitHub repository -
see GitHub docs on [Creating secrets for a
repository](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository).

## Local deployment

- Generate the translated website with `babelquarto::render_website()`

- Preview the website with `servr::httw("_site")`

## Drafting a new blogpost

Use the [custom `draft_post()` function](R/functions.R):

``` r
post_qmd <- draft_post(
  slug = "example_post",
  title = "How to use the draft_post() function",
  desc = "Using templates to increase productivity",
  categories = c("R", "data")
)
#> ℹ Creating new directory at 'posts/2024-12-15_example_post'
#> ℹ Writing blog post template at 'posts/2024-12-15_example_post/index.qmd'
readr::read_lines(post_qmd)
```

    #>  [1] "---"                                                                                                               
    #>  [2] "title: \"How to use the draft_post() function\""                                                                   
    #>  [3] "description:"                                                                                                      
    #>  [4] "  Using templates to increase productivity"                                                                        
    #>  [5] "date: 2024-12-15"                                                                                                  
    #>  [6] "date-modified: today"                                                                                              
    #>  [7] "image: featured.png"                                                                                               
    #>  [8] "citation:"                                                                                                         
    #>  [9] "  url: 2024-12-15_example_post"                                                                                    
    #> [10] "lang: en"                                                                                                          
    #> [11] "categories:"                                                                                                       
    #> [12] "  - R"                                                                                                             
    #> [13] "  - data"                                                                                                          
    #> [14] "---"                                                                                                               
    #> [15] ""                                                                                                                  
    #> [16] "```{r}"                                                                                                            
    #> [17] "#| label: setup"                                                                                                   
    #> [18] "#| include: false"                                                                                                 
    #> [19] ""                                                                                                                  
    #> [20] "renv::use(lockfile = \"renv.lock\")"                                                                               
    #> [21] "```"                                                                                                               
    #> [22] ""                                                                                                                  
    #> [23] ""                                                                                                                  
    #> [24] "## Reproducibility {.appendix}"                                                                                    
    #> [25] ""                                                                                                                  
    #> [26] "- [Source code](https://github.com/joelnitta/joelnitta-home/tree/main/posts/2024-12-15_example_post/index.qmd)"    
    #> [27] "- [`renv` lockfile](https://github.com/joelnitta/joelnitta-home/tree/main/posts/2024-12-15_example_post/renv.lock)"

``` r
fs::dir_delete(fs::path_dir(post_qmd))
```

## Licenses

Code: [MIT](LICENSE)

Text and images, unless otherwise indicated: Creative Commons
Attribution [CC BY
4.0](https://creativecommons.org/licenses/by/4.0/legalcode)

Publications (PDF files): Indicated in each publication.
