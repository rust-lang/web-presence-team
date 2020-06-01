# Zolaification aka One Website To Rule Them All
The current website is mainly divided between three separate components, the main website, the blog, and "Thanks". This has caused a few issues since their release as we have continued to work on it, as the styling has to be duplicated on each, and any change has to be replicated on each or the styling will be out of sync.

Not only is styling not shared between websites but each component is written in a unique Rust app, the main website is dynamic web server hosted on Heroku while the blog and thanks are separate bespoke static site generators that are deployed on GitHub Pages.

This makes the barrier to contributing quite high. It would significantly reduce the maintenance burden if we can consolidate these websites into a single website, and we could move away from requiring nightly rust for building and running the website. Ideally we would use a static site generator, as we haven't had a need for bespoke backend for any features on the website.

## Why Zola?

There are a lot of different static site generators, so there's a good reason ask why pick one over another. Zola has quite a few unique features that make it particularly appealing to us.


- **Independently maintained. —** We're not the sole or the biggest user of Zola, so there is plenty of community interest in continuing to use and add to Zola.
- **Tera templating language —** Tera is Zola's backing templating language that is a much more fully featured templating language that allows us to write a lot of the logic that we have in Rust directly in the Tera templates themselves.
- **Written In Rust —** Since Zola is written in Rust, it's much easier for Rust Web Presence members to contribute features to Zola that they need for the website. In fact some of that is currently in progress to add features currently missing from Zola that is present in our bespoke apps such as fluent support and multiple RSS feeds.
- **Asset collocation —** Zola by default allows you to host static content from the same directory as the source markdown, this removes some of the absolute URLs that required on the blog because the static assets directory is seperate.


## URL Scheme Changes

While Zola does provide the functionality we need it does not map directly to our current URL scheme, and merging the three websites will require moving the blog and thanks to be available under the `www.rust-lang.org` host. The blog currently has the following URL scheme.

**Current Scheme**

    https://blog.rust-lang.org/{YYYY}/{MM}/{DD}/{title}.html
    https://blog.rust-lang.org/inside-rust/{YYYY}/{MM}/{DD}/{title}.html

This date based URL scheme is not supported in Zola and the Zola maintainers have said they are not interested in adding this date scheme. Additionally we have had issues with the date based URL scheme, as sometimes a blog post is merged with the wrong date, and then updating the link will break any links to the previous posts, such as on social media or in RSS readers. For these reasons there is motivation to also take the time to use merging onto `www.rust-lang.org` to also revisit this URL scheme and then make the switch once.

Instead of following a date based scheme, we should try to replicate forge's layout of grouping by team for blog. For now this layout would **not** be reflected on the front end of blog itself, only in the internal layout and in the URL scheme. The blog's current flat list of articles grouped by year will remain unchanged.

**Proposed Scheme**

    https://www.rust-lang.org/blog/{topic}/{title}.html
    https://www.rust-lang.org/blog/inside-rust/{team}/{title}.html
    # Examples
    https://www.rust-lang.org/blog/releases/1.44.0.html
    https://www.rust-lang.org/blog/docs-rs/choose-your-build-target.html
    https://www.rust-lang.org/blog/inside-rust/governance-wg/meetings/2020-04-05.html
## Migrating to Zola

Luckily the website being divided into three separate apps actually makes migrating to a single component easier, as we can by switching only a single component and slowly integrate the other two without requiring that maintenance work be stopped or blocked on the transition. The best candidate to start with is the Rust blog, the website currently has fluent localisation which is not currently supported by Zola (though it is being actively worked on), and "Thanks" is both pretty bespoke in terms of functionality and is the least popular of the three. Switching the blog allows us to test and iterate on Zola with a sizeable portion of our audience, while maintaining the current level of functionality.

