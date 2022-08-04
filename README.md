# A statically generated blog example using Next.js and Sanity

This example showcases Next.js's [Static Generation](https://nextjs.org/docs/basic-features/pages) feature using [Sanity](https://www.sanity.io/) as the data source.

You'll get:

- Next.js deployed with the [Sanity Vercel Integration](https://www.sanity.io/docs/vercel-integration).
- Sanity Studio running on localhost and deployed in the [cloud](https://www.sanity.io/docs/deployment).
- Sub-second as-you-type previews in Next.js
- [On-demand revalidation of pages](https://nextjs.org/blog/next-12-1#on-demand-incremental-static-regeneration-beta) with [GROQ powered webhooks](https://www.sanity.io/docs/webhooks)

## Demo

### [https://next-blog-sanity.vercel.app/](https://next-blog-sanity.vercel.app/)

## Deploy your own

Using the Deploy Button below, you'll deploy the example using [Vercel](https://vercel.com?utm_source=github&utm_medium=readme&utm_campaign=next-example) as well as connect it to your Sanity dataset using [the Sanity Vercel Integration](https://www.sanity.io/docs/vercel-integration).

[![Deploy with Vercel](https://vercel.com/button)][vercel-deploy]

### Related examples

- [WordPress](/examples/cms-wordpress)
- [DatoCMS](/examples/cms-datocms)
- [TakeShape](/examples/cms-takeshape)
- [Prismic](/examples/cms-prismic)
- [Contentful](/examples/cms-contentful)
- [Strapi](/examples/cms-strapi)
- [Agility CMS](/examples/cms-agilitycms)
- [Cosmic](/examples/cms-cosmic)
- [ButterCMS](/examples/cms-buttercms)
- [Storyblok](/examples/cms-storyblok)
- [GraphCMS](/examples/cms-graphcms)
- [Kontent](/examples/cms-kontent)
- [Ghost](/examples/cms-ghost)
- [Umbraco Heartcore](/examples/cms-umbraco-heartcore)
- [Blog Starter](/examples/blog-starter)
- [Builder.io](/examples/cms-builder-io)

## Configuration

### Step 1. Set up environment variables

Pull down environment variables from your Vercel project:

```bash
npx vercel env pull
```

#### You can also set them manually

Run `cd studio && npx @sanity/cli init`. You'll then be able to select from existing projects. The CLI will update sanity.json with the project ID and dataset name.

Copy the `.env.local.example` file in this directory to `.env.local` (which will be ignored by Git):

```bash
cp .env.local.example .env.local
```

Then set these variables in `.env.local`:

- `NEXT_PUBLIC_SANITY_PROJECT_ID` should be the `projectId` value from the `studio/sanity.json` file.
- `NEXT_PUBLIC_SANITY_DATASET` should be the `dataset` value from the `studio/sanity.json` file.
- `SANITY_API_READ_TOKEN` create an API token with `read-only` permissions:
  - Run `cd studio && npx @sanity/cli manage` or go to https://manage.sanity.io/ and open your project.
  - Go to **API** and the **Tokens** section at the bottom, launch its **Add API token** button.
  - Name it **SANITY_API_READ_TOKEN**, set **Permissions** to **Viewer**.
  - Hit **Save** and you can copy/paste the token.
- `SANITY_PREVIEW_SECRET` can be any URL friendly value you want (for example by running `Math.random().toString(36).substr(2, 10)`), it's sent as a query parameter to enable [Preview Mode](https://nextjs.org/docs/advanced-features/preview-mode).

Your `.env.local` file should look like this:

```bash
# Connect to Content Lake
NEXT_PUBLIC_SANITY_PROJECT_ID=...
NEXT_PUBLIC_SANITY_DATASET=...
# Preview mode
SANITY_API_READ_TOKEN=...
SANITY_STUDIO_PREVIEW_SECRET=...
# On-demand revalidation
SANITY_REVALIDATE_SECRET=
```

### Step 2. Add CORS origins for Preview Mode

When starting Preview Mode an "is logged in" check is performed, and it requires CORS headers.

```bash
# Ensure the Sanity CLI are using the right project id and dataset
npm run studio:env

cd studio
# Next.js runs on port 3000 by default
npx @sanity/cli cors add http://localhost:3000 --credentials
# If deploying to vercel were your first step let's add the production URL while at it
npx @sanity/cli cors add https://next-blog-sanity.vercel.app/ --credentials
```

### Step 3. Run Next.js in development mode

```bash
npm install
npm run dev
```

```bash
yarn install
yarn dev
```

Your blog should be up and running on [http://localhost:3000](http://localhost:3000)! If it doesn't work, post on [GitHub discussions](https://github.com/vercel/next.js/discussions).

### Step 4. Populate content & try preview mode

Run `npm run studio:dev` in another terminal and after the project has started and you have navigated to the URL given and you're ready to create some content.

- Click on the "Create new document" button top left and select **Post**.
- Type some dummy data for the **Title**.
- **Generate** a **Slug**..
- There's enough data to start [Preview Mode](https://nextjs.org/docs/advanced-features/preview-mode):
  [![image](https://user-images.githubusercontent.com/81981/182708582-1fedc87a-d3e7-48f6-9927-ba3840a7fa2a.png)](https://www.sanity.io/docs/preview-content-on-site)

- Scroll down to **Author** and click **Create new**.
- Give the **Author** a **Name**.
- For the image, select [Unsplash](https://unsplash.com/) in [the dropdown](https://user-images.githubusercontent.com/81981/182700991-1f5bc497-605d-4416-a1b9-3c98671fd326.png) and filter by "face".
- Upload a **Cover** **Image**, e.g. search for "Architecture".
- See how the blog post layout respond in real-time as you define hotspot & crop on the image.
- You can navigate around in the preview, including the frontpage.
- Create a couple more **Posts** and watch how the layout adapt to more content.

**Important:** For each post record, you need to click **Publish** after saving. If not, the post will be in the draft state and only visible when Preview Mode is active.

To exit Preview Mode, you can click on _"Click here to exit preview mode"_ at the top.

### Step 5. Deploy on Vercel

You can deploy this app to the cloud with [Vercel](https://vercel.com?utm_source=github&utm_medium=readme&utm_campaign=next-example) ([Documentation](https://nextjs.org/docs/deployment)).

#### Deploy Your Local Project

To deploy your local project to Vercel, push it to GitHub/GitLab/Bitbucket and [import to Vercel](https://vercel.com/new?utm_source=github&utm_medium=readme&utm_campaign=next-example).

**Important**: When you import your project on Vercel, make sure to click on **Environment Variables** and set them to match your `.env.local` file.

#### Deploy from Our Template

Alternatively, you can deploy using our template by clicking on the Deploy button below.

[![Deploy with Vercel](https://vercel.com/button)][vercel-deploy]

### Step 6. Add deployment URL to CORS origins

Enable Preview Mode by adding the production URL to the deployment you made in [Step 5](#step-5-deploy-on-vercel):

```bash
npx @sanity/cli cors add https://next-blog-sanity.vercel.app/ --credentials
```

### Step 7. Setup Revalidation Webhook

Using GROQ Webhooks Next.js can rebuild pages that have changed content. It rebuilds so fast it can almost compete with Preview Mode.

Ensure any environment variables that were added in Step 5 exists in the local checkout

```bash
npx vercel env pull
```

Create a secret (for example by running `Math.random().toString(36).substr(2, 10)`) that will be used to verify that webhook payloads came from Sanity infra, and set it as the value for `SANITY_REVALIDATE_SECRET`:

```bash
npx vercel env add SANITY_REVALIDATE_SECRET
# Update local .env
npx vercel env pull
# Apply the new env var in production
npx vercel --prod
```

Wormhole into the [manager](https://manage.sanity.io/) by running `cd studio && npx @sanity/cli hook create`:

- **Name** it "On-demand Revalidation".
- Set the **URL** to use the Vercel app url from [Step 5](#step-5-deploy-on-vercel) and append `/api/revalidate`, for example: `https://cms-sanity.vercel.app/api/revalidate`
- Set the **Trigger on** field to <label><input type=checkbox checked> Create</label> <label><input type=checkbox checked> Update</label> <label><input type=checkbox checked> Delete</label>
- Set the **Filter** to `_type == "post" || _type == "author"`
- Set the **Secret** to the same value you gave `SANITY_REVALIDATE_SECRET` earlier.
- Hit **Save**!

#### Testing the Webhook

- Open the Deployment function log. (**Vercel Dashboard > Deployment > Functions** and filter by `api/revalidate`)
- Edit a Post in your Sanity Studio and publish.
- The log should start showing calls.
- And the published changes show up on the site after you reload.

### Step 8. Deploy Sanity Studio

Set the production URL in an env var so the Studio can create its "Open Preview" link correctly, for activating Preview Mode.

```bash
npx vercel env add SANITY_STUDIO_PRODUCTION_URL
```

Deploy the Studio to Sanity's CDN:

```bash
npm run studio:deploy
```

### Next steps

- Use Preview Mode to see the effect of image hotspot cropping, in [real](https://user-images.githubusercontent.com/81981/182700131-6f2deeb5-548e-4dd3-b041-8f89e96a0bac.png)-[time](https://user-images.githubusercontent.com/81981/182700249-95d6f045-dcb2-4acf-bd1e-3f1ecbe2980b.png).
- Mount your preview inside the Sanity Studio for comfortable side-by-side editing
- [Join the Sanity community](https://slack.sanity.io/)

[vercel-deploy]: https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fvercel%2Fnext.js%2Ftree%2Fcanary%2Fexamples%2Fcms-sanity&env=SANITY_STUDIO_PREVIEW_SECRET&envDescription=Any%20URL%20friendly%20value%20to%20secure%20Preview%20Mode&project-name=cms-sanity&repo-name=cms-sanity&demo-title=Blog%20using%20Next.js%20%26%20Sanity&demo-description=On-demand%20ISR%2C%20sub-second%20as-you-type%20previews&demo-url=https%3A%2F%2Fnext-blog-sanity.vercel.app%2F&demo-image=https%3A%2F%2Fuser-images.githubusercontent.com%2F110497645%2F182727236-75c02b1b-faed-4ae2-99ce-baa089f7f363.png&integration-ids=oac_hb2LITYajhRQ0i4QznmKH7gx
