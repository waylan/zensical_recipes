---
icon: simple/cloudflareworkers
---

# Deploy to Cloudflare with Workers Builds

When properly configured, [Cloudflare Workers Builds] will detect changes made
to your Git repository, retrieve those changes, and automatically build and
deploy your Zensical site as static assets on a [Cloudflare Worker].
Cloudflare's [Git Integration] supports connecting to a Git repository hosted
on [GitHub], [GitLab], [Cloudflare Artifacts], or [Cursor Origin].

For other Git providers, you can use an [external CI/CD provider] and deploy
using [Wrangler CLI]. While that is out-of-scope for this article, the steps
listed below under [Preparing Your Project](#preparing-your-project) will
still apply.

[Cloudflare Workers Builds]: https://developers.cloudflare.com/workers/ci-cd/builds/
[Cloudflare Worker]: https://developers.cloudflare.com/workers/
[Git Integration]: https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/
[Cloudflare Artifacts]: https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/artifacts-integration/
[GitHub]: https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/github-integration/
[GitLab]: https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/gitlab-integration/
[Cursor Origin]: https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/cursor-origin-integration/
[external CI/CD provider]: https://developers.cloudflare.com/workers/ci-cd/external-cicd/
[Wrangler CLI]: https://developers.cloudflare.com/workers/wrangler/commands/general/#deploy

## Preparing Your Project

Before deploying, you need to properly prepare your project. First, create a
new file named `wrangler.toml` at the root of your project (next to your
Zensical configuration file).

Add the following to your `wrangler.toml` file:

``` toml
name = "projectname"
compatibility_date = "2026-10-01"
preview_urls = false
```

The `name` should be a unique name that will be used to identify the project
within Cloudflare's dashboard. The `compatibility_date` should be the date
that you created the file (see [Compatibility dates] for details.
`preview_urls` is set to `false` as that feature is outside of the scope of
this document.

[Compatibility dates]: https://developers.cloudflare.com/workers/configuration/compatibility-dates/

You now have a choice to make. You can either deploy to a [custom domain] or
a [`workers.dev` subdomain][workers.dev] provided by Cloudflare. You may
chose to start with a `workers.dev` subdomain for testing purposes and
later add a custom domain.

[custom domain]: https://developers.cloudflare.com/workers/configuration/routing/custom-domains/
[workers.dev]: https://developers.cloudflare.com/workers/configuration/routing/workers-dev/

/// tab | `workers.dev`

If you are using a `workers.dev` subdomain provided by Cloudflare, you can
add a line to `wrangler.toml` with `workers_dev = true`, but that is not
necessary so long as no `[[routes]]` section is configured.

Your `workers.dev` subdomain will take the format `<YOUR PROJECT
NAME>.<YOUR_ACCOUNT_SUBDOMAIN>.workers.dev`. `<YOUR PROJECT NAME>` is the
`name` you defined in `wrangler.toml` above and
`<YOUR_ACCOUNT_SUBDOMAIN>` is a subdomain shared across all projects
within the same account (usually defaults to your username).
///

/// tab | Custom Domain

If you are using a [custom domain], you should then add the following
lines to your `wrangler.toml` file.

``` toml
workers_dev = false

[[routes]]
pattern = "example.com"
custom_domain = true
```

Replace `example.com` with your custom domain. Be sure to have properly
configured your custom domain with Cloudflare. A domain must be managed by
Cloudflare to be available for use.
///

Finally, add the following to `wrangler.toml` to tell Cloudflare about the
files in your project.

``` toml
[assets]
directory = "./site"
not_found_handling = "404-page"
```

The `directory` asset should point to Zensical's build directory and
`not_found_handling` tells Cloudflare to use the [error page] provided by
Zensical in the build directory. Note that the value `"404-page"` has special
meaning to Cloudflare and is not the filename of the error page (see 
[Routing behavior] for more information).

[error page]: https://zensical.org/docs/customization/#custom-error-pages
[Routing behavior]: https://developers.cloudflare.com/workers/static-assets/#routing-behavior

!!! seealso "See Also"

    See [Wrangler Configuration] for all available configuration options.

[Wrangler Configuration]: https://developers.cloudflare.com/workers/wrangler/configuration/

<!-- 
    TODO: Explore defining the build command in a `[build]` section.
    See https://developers.cloudflare.com/workers/wrangler/custom-builds/
-->

Next, ensure that your `pyproject.toml` file properly lists all of the
dependencies needed to build your site. If you have been using `uv` (and
[set up your project with `uv`][uv]), then you should be good to go.

[uv]: https://zensical.org/docs/get-started/#install-with-uv

If your project contains a `.python-version` file you may want to ensure that
the version listed in the file matches the version Cloudflare uses by
default. You can find Cloudflare's default version listed under 
[Build Image Runtime] (currently `3.13.3`). If the versions do not match, then
Cloudflare will download and install the version specified in your
`.python-version` file, which significantly lengthens the build time.

[Build Image Runtime]: https://developers.cloudflare.com/workers/ci-cd/builds/build-image/#runtime

## Setup Cloudflare

The rest of the setup will be through the Cloudflare Dashboard. Be sure to
sign into your account and follow the steps below.

- From the Cloudflare Dashboard, go to [Workers & Pages]. 
- Select __Create application__. 
- Select your Git provider. Note that [GitHub] and [GitLab] will always be listed
  as options, whereas [Cloudflare Artifacts] and [Cursor Origin] will only be
  listed if you have already associated them with your account. See the 
  documentation for your provider for more details.
- Provide the information about the repository and continue.
- Cloudflare may redirect you to the repository host to authenticate.
- If asked, give permission for a hook to be set up at your repository host.
  Ensure that the hook is triggered only by the actions that you want to have
  trigger a new build.
- After completing the connection to your repository, set the __Build command__
  to `uv run zensical build --clean` (under Settings).
- Set the __Deploy command__ to `npx wrangler deploy` (under Settings).
- If you are using a custom domain, you will need to connect the domain to your
  worker. A domain must be managed by Cloudflare to be available for use.
- Select __Deploy__.

[Workers & Pages]: https://dash.cloudflare.com/?to=/:account/workers-and-pages

That's it. You are done. Once the build completes, you should find a link to
the live site from your Dashboard. So long as you properly configured the hook,
upon each hook action a new build will be initiated.

## Answers to Anticipated Questions

/// details | ### Why not use Cloudflare Pages?
    type: question
    attrs: { name: faq }

Cloudflare seems to be deprecating their Pages service (see the note at the
top of their [Pages Overview]). They recommend using workers instead.
However, if you still want to deploy to Pages, you are certainly welcome to
do so. I recommend loosely following their [MkDocs guide].

[Pages Overview]: https://developers.cloudflare.com/pages/
[MkDocs guide]: https://developers.cloudflare.com/pages/framework-guides/deploy-an-mkdocs-site/
///

/// details | ### Doesn't Cloudflare charge for workers?
    type: question
    attrs: { name: faq }

Cloudflare only charges for worker instances which run code in response to a
request. They do not charge for static assets served through a worker. Nor do
they charge for running builds. As Zensical only provides static files to be
served, there should never be any charges incurred. In fact, you can set up a
working site without ever providing Cloudflare with any payment information.
///

/// details | ### What if I don't want to transfer my domain to Cloudflare?
    type: question
    attrs: { name: faq }

Cloudflare only needs to manage your domain, it does not need to be the
register for your domain. See Cloudflare's documentation on how to
[Onboard a domain](https://developers.cloudflare.com/fundamentals/manage-domains/add-site/).
///
