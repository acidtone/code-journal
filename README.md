# Code Journal
Tony's notes and such for his coding explorations.

---

## March 31, 2022
**Wishlist**: A collection of tutorials tagged by the technologies and versions used.
    
```yaml
- title: Let's learn SvelteKit by building a static Markdown blog from scratch
  link: https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog
  tech: 
    - svelte@3
    - sveltekit@next
    - mdsvex@0.10 
```
- User stories: 
    - As a learner, I want to save a great tutorial I just found, so that I can easily find it later.
    - As a tutor, I want to collect a library of learning materials, so that I can easily assign homework to my clients.
    - As a teacher, I want to filter out old tutorials, so that my students don't get confused by references to older syntax.
    - As a school, I want to filter for tutorials by technology, so that I can quickly generate materials for a particular course/program.
- Could also do the same thing with CSS properties, HTML elements and JS features?
- How do you host it? 
    - Could it be a file database? markdown with front matter?
    - Add the complexity of Strapi?
    - Sanity.io? Possible to convert to Strapi later?
- Collections
    - Resources
        - title
        - author
        - link
        - slug
        - breakdown
    - Authors
        - name
        - slug
        - socials: gh, codepen, etc
    - Technologies
        - name
        - slug
        - homepage

---

## March 27, 2022
**Goal**: Create a slides directory in SvelteKit (SK) that wraps RevealJS around all files inside it.
- I just did this today with Nuxt (both 2 & 3) with mixed results. I've got high hopes that this will be easier with SK.
- First decision: do I carry on with the md-blog project or create a new one? 
    - I don't know what's ahead of me so I'm going to keep it simple: implement reveal site-wide and figure out how to do it on the route level later. 
    - If that works: implement markdown slides.
- Got it working with Ash and Jess's help but the .__nuxt container needs an explicit height.
- Conclusion: now sure what's breaking but it feels like the nuxt container is hiding the slide content by becoming zero height. It could also be a tailwind issue?

---

## March 23, 2022
**Goal**: Deploy md SK skeleton site to Netlify
- Going off the official [Netlify docs](https://docs.netlify.com/configure-builds/common-configurations/sveltekit/).
- [And it worked](https://admirable-pithivier-572d1d.netlify.app/tests/markdown) after I removed `config.kit.target`. The documentation needs to be updated.

---

## March 22, 2022
**Goal**: Set up a boilerplate Svelte site with Markdown support
- Following this tutorial: [Let's learn SvelteKit by building a static Markdown blog from scratch](https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog)
    - Skipped the SASS portion
    - So far so good; MDSveX installed like a charm
    - After md support added, VS Code is showing syntax squiggles for the Svelte code
    - PrismJS is working by default with MDSveX but there are no highlight styles. 
        - Problem: How to add `prism.min.css` from a CDN
        - `svelte.head` seems to be the Svelte way of adding a `link` element to the `head` of a page. [The official tutorial](https://svelte.dev/tutorial/svelte-head) shows adding `<svelte:head>` to `App.svelte` that file doesn't exist in the skeleton site that's been generated.
        - Solution: Added the `link` to `__layout.svelte` using `<svelte:head>` and it worked! `svelte.head` seems to be built into SvelteKit.
        - TODO: Add some remark plugins
            - definition lists
            - element classes
            - wrapper elements
    - Using the Git page from WBDV as a sample blog post. 
        - TODO: Figure out where/how images are handled in SK (for error screencaps)
            - Where are static assets kept?
            - How are relative links handled?
            - Does SK have any fancy features for image handling?
    - TODO: Add `/categories/index.svelte` page to list all categories
    - [Adding autoprefixer](https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog#add-autoprefixer-with-postcss) requires `svelte-preprocess`, which I skipped because I'm not using SASS so I'll have to go back and install it.
        - Pretty straight forward...
    - TODO: Add Excerpts to the blog index page
    - TODO: Add [Custom error page](https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog#customize-the-error-page)
    - TODO: [Preload routes](https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog#preload-routes)
- Done!

**Goal**: Install remark plugins for extended markdown syntax parsing.
- Mostly using the [MDSveX Docs](https://mdsvex.pngwn.io/docs#remarkplugins--rehypeplugins)
- Definition lists:
    - `remark-definition-list` doesn't seem to work anymore but `remark-deflist` does.
- Wrappers: 
    - `remark-container` works as advertized
    - Tried `remark-directive` for deeper syntax support but the `{}` syntax seems to interfere with Svelte. I'd really like to get this one working. Might require an Issue submission to the `MDsveX` repo.
- Table of Contents
    - Leaving this for later. Can be done with `remark` and/or `rehype`. Requires more research.
- Codepen Embeds
    - The HTML method from Codepen's copy/paste utility breaks during build but the iFrame option seems to work.


---

## March 8, 2022
Research: Design in Figma
- Mood boards are cool
    - [How to Mood Board for Web Design](https://www.youtube.com/watch?v=1A-tepzfhJw) by Jesse Showalter
        - Moodboards should contain:
            - Colour palette
            - Typography
            - Textures
            - Patterns
            - Photo selections
            - Misc
        - Types of mood boards
            1. Strict mood board
            2. Messy mood board
            3. Pin board
            4. Style Tile
    - [How to Prepare for a Brand Identity Mood Board](https://www.youtube.com/watch?v=5uHZNZc38II)
- Compilation of Mood board contents from the above and some other videos:
    1. Colour palettes
    2. Typography
    3. Photos and illustrations
    4. Textures
    5. Patterns
    6. Logos
    7. Misc: anything that directly relates to the project

---

## Feb 22, 2022
Goal: Find a router for OPNSense
- Apparently, Memory Express isn't what it used to be. Every potential router brand that will run pfSense or OPNSense is not found. The only appliance that comes close are Ubiquiti brand, which have a bad reputation. Shame.
- Found this Reddit thread: [Best little hardware device for OpnSense/PFSense?](https://www.reddit.com/r/homelab/comments/f4ge95/best_little_hardware_device_for_opnsensepfsense/) 
    - Mentioned brands:
        - Something with a [PCEngines](https://www.pcengines.ch/) board
        - [HP T620+](https://support.hp.com/ca-en/document/c04017240) with a quad-port PCIe NIC
        - [Protectli](https://protectli.com/product-comparison/)
        - [Qotom](https://www.qotom.net/) mini-PC
        - [fitlet](https://fit-iot.com/web/products/fitlet2/)
        - Intel NUC
    - None of these are available locally from what I can tell. Might try AliExpress for the first time...

---

## Feb 19, 2022
Goals: 
- Secure my local network
- Potentially open network so I have access to my files outside my home
- Learn network shit
- Create a base setup where I can:
    - sniff packets coming from the Nest
    - sniff packets in general
    - Unicorn: create a Youtube history of videos I've watched in the Shield/Roku/etc
        - Goal: reference for educational vids I can use for lesson plans/activities/etc.

Results:
- pfsense vs opnsense
    - [pfSense® vs OPNSense - which firewall OS is better?](https://teklager.se/en/pfsense-vs-opnsense/)
        - I had no idea there was nerd-drama between pgSense and OPNsense. Juicy.
        - It seems like I can go either way but the (apparent) behaviour of the pfSense community pisses me off enough to offset the bigger ecosystem. Going to try OPNSense!
- load balancing
    - [How can you survive without a load balancer in every home?](https://www.loadbalancer.org/blog/how-can-you-survive-without-a-load-balancer-in-every-home/)
        - Still not convinced I need a load balancer but if I do...
        - I don't think I'll go with the free version of Kemp? I think HAProxy is the way to go if I can find a NetworkChuck-level tutorial to walk me through the process. Worst case, I guess I can take notes on NC's video and figure out how to do it on HAProxy.
- hardware:
    - [What hardware to buy for OPNsense router in 2021](https://teklager.se/en/opnsense-hardware/)
        - None of the routers on this list are available at MemEx. In fact, I can't find any non-wifi routers other than Ubiquiti (which seem to not have a good following)
        - TODO: Find a good router that can run either OPNSense (preferred) or pfSense


---

## Jan 30, 2022
Goal: Looking for js-based libraries that can be used as CDN examples.
- The requirements of the library are still vague but here's what I can think of so far:
    - A minimal amount of JS knowledge is required to give us a starting point for JS lessons:
        - variables and assignment
        - function invocation
        - object literals
    - The library should provide immediate value to the coder installing it;
    - The difficulty of the library should be appropriate to the coder's skill level

- Revealjs
    - This is the first library I thought of and most of the installation was review for me.
    - The [installation options](https://revealjs.com/installation/) do not include CDNs
    - The simplest option, [Basic Setup](https://revealjs.com/installation/#basic-setup) comes with _a lot_ of extra files and directories:
        - `/dist` plus `theme`
        - `/plugin`
        - `gulp.js`
        - `LICENSE`
        - etc, etc
    - The other options are [Full Setup](https://revealjs.com/installation/#full-setup) ("Recommended") and [Install from npm](https://revealjs.com/installation/#installing-from-npm)
    - What seems to be missing is a "blank boilerplate" install using a CDN?
    - Question: Is there a simple config/setup option that would make a good introduction to JS?
        - The [Markup Guide](https://revealjs.com/markup/) has a bare bones set up that could make a good intro to invoking a function:

            ```html
            <html>
            <head>
                <link rel="stylesheet" href="dist/reveal.css">
                <link rel="stylesheet" href="dist/theme/white.css">
            </head>
            <body>
                <div class="reveal">
                <div class="slides">
                    <section>Slide 1</section>
                    <section>Slide 2</section>
                </div>
                </div>
                <script src="dist/reveal.js"></script>
                <script>
                Reveal.initialize();
                </script>
            </body>
            </html>
            ```
        - The [Config Options](https://revealjs.com/config/) could be a good intro to object literals:

            ```js
            Reveal.initialize({
                controls: true,
                rtl: false,
                navigationMode: 'default',
                preloadIframes: null,
                autoAnimateEasing: 'ease',
                autoAnimateDuration: 1.0,
                autoAnimateUnmatched: true,
                autoAnimateStyles: [
                    'opacity',
                    'color',
                    'background-color',
                    'padding',
                    'font-size',
                    'line-height',
                    'letter-spacing',
                    'border-width',
                    'border-color',
                    'border-radius',
                    'outline',
                    'outline-offset'
                ],
                autoSlide: 0,
                // Transition style
                transition: 'slide', // none/fade/slide/convex/concave/zoom
                pdfMaxPagesPerSlide: Number.POSITIVE_INFINITY,
                hideCursorTime: 5000
            });
            ```

            - complex value types: object literal as the container, array as a config option
            - boolean, string, number and null as primitives
        - The [`Reveal.configure()`](https://revealjs.com/config/#reconfiguring) could be used to introduce methods (and serve as a `button` assignment for triggering events).
    - The bare bones example they give is all that's needed to get things started, but I'll use a CDN so it will fit in a Gist.

## Jan 22, 2022
**Goal**: Import a subset of terms into Sanity 

**Plan**:
1. Convert a terms file to ndjson.
2. Import to sanity using the cli.
3. Add some categories to the terms using studio.
4. Export the result.
5. Figure out how to structure a full import.

**Brute force**
- Materials from last session
    - [bulk imports](https://www.sanity.io/docs/importing-data)
    - [content modelling](https://www.sanity.io/docs/content-modelling)
- Imports seem to complete without error but they don't show up in studio. 
- I've had to try importing a few times after changes but I get `Document by ID "global" already exists` errors the second time around.
    - I got away with the first change by changing test-import to terms but now I've got to try deleting `production`?
    - I need to figure out how to burn the farm.

## Jan 21, 2022
### R&D: Sanity data imports
#### Goal
What's the best process for burning the farm and importing my terminology data from scratch?

#### Plan
- If I have a list of terms in YAML, how do I import them into Sanity so that they use categories that I've already entered into Sanity? 
- What are the CRUD implications of referencing categories in an import (if possible)?

#### Brute Force
- Looking up [bulk imports](https://www.sanity.io/docs/importing-data), found in the last session.
- I'm learning things! There's a such thing as NDJSON - Newline delimited JSON. So, no: it doesn't look like Sanity supports yaml imports. 
    - yaml -> json -> ndjson will need to be a thing.
- From the docs: 
    - Documents should follow the [structure of your data model](https://www.sanity.io/docs/content-modelling):
    - most importantly, the requirement of a `_type` attribute. 
    - the `_id` field is optional – but helpful – in case you want to make references or be able to re-import your data replacing data from an old import.
    - `_id`s in Sanity are usually a GUID, but any string containing only letters, numbers, hyphens and underscores are valid.
        - Can slug act as effective ids? How about overloaded terms like "view" or "property"? 
            - `property-object` vs `property-css`
            - Maybe there's a way to auto-append categories to the term to make them unique(ish)?
                - I can't think of an instance where a term with the exact same categories would be different. It just means that categories would need to be used for repeating terms.

---

## Jan 20, 2022
### R&D: Hello Sanity
#### Goal
What's the best starting config for a Terminology module?

#### Plan it out
1. `sanity init` the blog template because that's what [Kapehe did](https://www.youtube.com/watch?v=32RP-sG1njE)
2. Sniff around the template and see what's what.
3. Depending on what I find out, try creating a terms schema.
4. What's the easiest way to import bulk data, preferably YAML?

#### Brute force
- Tried a few video tutorials and Kapehe comes up as a guest on a few YT Channels. Her script is the same for most so I'm following [the one from Traversy](https://www.youtube.com/watch?v=32RP-sG1njE).
- Installing a vanilla blog: `$ sanity init`
    - have to login
    - Project: hello-sanity
    - Dataset: production
    - Template: blog 
        - Kapehe says this has a schema but no data. The others have sample data.
- The README links:
    - [Read “getting started” in the docs](https://www.sanity.io/docs/introduction/getting-started?utm_source=readme)
    - Check out the example frontend: [React/Next.js](https://github.com/sanity-io/tutorial-sanity-blog-react-next)
    - [Read the blog post about this template](https://www.sanity.io/blog/build-your-own-blog-with-sanity-and-next-js?utm_source=readme)
    - [Join the community Slack](https://slack.sanity.io/?utm_source=readme)
    - [Extend and build plugins](https://www.sanity.io/docs/content-studio/extending?utm_source=readme)    
- At first glance, the directory is pretty light. That's a good sign.
- It looks like the `schemas` directory is where all the action is.
- Question: how do I boot this up locally?
    - The docs look pretty polished so far: [Getting started with Sanity CLI](https://www.sanity.io/docs/getting-started-with-sanity-cli)
        - `sanity start`
- I just realized `sanity init` installs everything in the `pwd`. Moving into a directory...
- `sanity start` works as expected. But now I have questions about where to go from here.
    - What's the relationship between `localhost:3333` and the data?
    - I've just started a `hello-sanity` project. Is this using up quotas on my free Sanity account?
        - Looks like I'm limited to two [datasets](https://www.sanity.io/docs/datasets).
    - How many projects can I set up with `sanity init` before I have to pay? So far I have `hello-sanity` and `browser-therapy` but the second has no studio code installed.
        - Ahhh, that's what `production` is: the dataset. So it looks like I can choose my dataset when I build a new project.
            - So what if I delete schemas that another project depends on?
- Where do I store data for the terms project? 
    - `production`? 
    - Where do I put the `hello-sanity` data?
    - It doesn't look like I can rename the production dataset.
    - Maybe the blog schema is general enough that I can just keep those and add the terms schema to it. Whatevs, I can just burn it all and create a new account with my GH login.
- I'll try adding terms and maybe move the project out of `_hello` once there's data/code that I don't want to lose.

Goal: add terms schema
- fields:
    - term
    - description
    - slug
    - any other Sanity fields that seem handy
- First decision: what schema type do I use?
    - [Schema Types Docs](https://www.sanity.io/docs/schema-types)
    - Looks like `document` is the base type. Good enough for now.
- Next decision: what field type should I use for term description?
    - Is not a string, but `blockContent` seems like overkill. Is there a middle type?
    - Kinda? There's the `text` field type but it looks like all `blockContent` is, is an array of special `text` fields.
    So the question is do I want to use formatting like bold and italic?
    - Given that I still need to import all these terms, I'll go simple and use `text`. Maybe there's a plugin to add markdown to the import and use `blockContent` later.
- OK, created `./schemas/term.js` and it's not showing up in Studio. Looks like I have to register it...
    - Boom. Just have to add it to `./schemas/schema.js`. Pretty slick.
- I think I'm going to stop here for now and look into [bulk imports](https://www.sanity.io/docs/importing-data) next session.


---

## Jan 15, 2022
- Links from Ellie's guest speaker session
    - [Laws of UX](https://lawsofux.com/)
        > Laws of UX is a collection of best practices that designers can consider when building user interfaces.
    - [Frontend Mentor](https://www.frontendmentor.io/)
        > Solve real-world HTML, CSS and JavaScript challenges whilst working to professional designs. Join 282,931 developers building projects, reviewing code, and helping each other get better.

## Jan 10, 2022
- First class of `#winter-2022`. Totally forgot to add markdown to W1W Prep :/
    - search: "[github flavoured markdown cheatsheet](https://www.google.com/search?q=github+flavoured+markdown+cheatsheet)"

Goal: Continue to add content to the Library, and test the `_.vue` file for [Unknown Dynamic Nested Routes](https://nuxtjs.org/docs/features/file-system-routing/#unknown-dynamic-nested-routes).
- Time to try this Nuxt feature out to easily migrate last semester's content to an expanded Library directory:
    - breakdowns: synopsis files for individual articles, videos, tutorials, etc (aka resources).
    - takeaways: summaries of topics that aren't associated with one resource.
    - tools: Summaries of cheats, tutorials and documentation for VS Code, bash apps, Firefox, etc.
- if the `_.vue` file works as advertized, I should be able to add markdown to the `content/library` subdirectories without adding sister `vue` components to the `pages` directory.
- [starting commit](https://github.com/sait-wbdv/winter-2022/commit/0dded36ff7927cb0bcb97602b55a80c16c1b44da)

## Jan 9, 2022
Goal: Figure out how to get [Unknown Dynamic Nested Routes](https://nuxtjs.org/docs/features/file-system-routing/#unknown-dynamic-nested-routes) working. 
- Specifically, I want [this page](https://github.com/sait-wbdv/winter-2022/commit/9111f41120862367d77337d1931b6b483863a676) (also my initial commit) to render when I enter this url:
    - `/library/breakdowns/making-clickable-elements-recognizable`
    - plus, I don't want to fiddle with the `pages` directory if I organize these md files in sub directories.
- Recap: got it working pretty quickly. `_.vue` files are a nice feature (for now).
    - [finishing commit](https://github.com/sait-wbdv/winter-2022/commit/ea246d6cf3a8bb62182a281c9242fb070c2f5782)

## Jan 8, 2022
### Winter-2022 rollout
- Repo: [winter-2022](https://github.com/sait-wbdv/winter-2022)
- Live Site TBD ([to be deployed](https://github.com/sait-wbdv/winter-2022/issues/21))

### To Dos
- Migrations:
    - [x] Schedule -> Home page
    - [x] Home page -> Test Page
    - f21 -> 
      - [x] House Rules
      - [x] Library
- Research: Takeaways
    - [ ] do slugs include directory segments?
- Schedule: 
    - [ ] Dasa day fix
    - [ ] Add two 201 days!
- add home work to 
    - [x] 201 day 1 and 2
    - [ ] 270 day 1-4
    - [ ] 262 day 1-5

Goal: Knock as many down as I can and throw Assignment ideas in the parking lot
- [starting commit](https://github.com/sait-wbdv/winter-2022/commit/d4239468bcba6ef5ca424c003783c18d577b358b)
- Starting to get the hang of Nuxt Content. 
- Taking a break. [commit: migrated page content](https://github.com/sait-wbdv/winter-2022/commit/a2a84a4eef97253b8d9b411feaa1e179234fe809)
- Back from break. [Added checkboxes to the todos](https://github.com/acidtone/code-journal/commit/ed3ac088a079cef7c3b93a69ce8141516d5c0085).

Goal: Add Homework and Labs to 201 Lessons
- Plan
    1. [x] Migrate: Orientation -> Day 0
    2. Test homework component: Add issues as needed
    3. Create Labs content directory
        - Day 1: Follow the white rabbit
        - Day 2: [Project: Publish a webpage with Git and GitHub Pages](https://gist.github.com/acidtone/5d45f96bc11fada75038e552f9ba1a5c)
- commit: [201 day0](https://github.com/sait-wbdv/winter-2022/commit/280fdc89d9ff29041614c921b5655a2f869bc993)

Goal: Add Homework
- TODO: Change "Homework" to "Prep" or similar.
- commits: 
    - [Added some content and tested some homework](https://github.com/sait-wbdv/winter-2022/commit/4a2372b56cac0f732fac68eacbac3a7e9a475826)
    - [added test pages](https://github.com/sait-wbdv/winter-2022/commit/8f7ab9d47034c468cc819f1960ab32877a38fc9b)

## Jan 7, 2022
Goal: Create a `store` if a flat chronological array of lesson slugs.
- Video: [Vuex Store with Nuxt and Vue](https://www.youtube.com/watch?v=53tRnb2d6r0)
    - `/store/index.js`

        ```js
        // state
        export const state = () => ({})

        // getters
        export const getters = {}
        // actions
        export const actions = {}

        // mutations
        export const mutations = {}
        ```
- Video: [Learn Vuex in 30 MINUTES! (Vue JS 3)](https://www.youtube.com/watch?v=nFh7-HfODYY)
    
    - `/store/index.js`

        ```js
        import { createStore } from 'vuex'
        
        export default createStore({
            state: {},
            mutations: {},
            actions: {},
            getters: {},
            modules: {}
        })
        ```
- These two videos show different ways to create a store but then I realized:
    1. There's Vue 2 and 3
    2. There's Veux 3 and 4
        - turnes out `createStore()` is newer and probs won't work with our site
    3. I'm creating a store in Nuxt 2
- Search: "nuxt 2 vuex"
    - Nuxt Docs: [Store directory](https://nuxtjs.org/docs/directory-structure/store/)
        - These docs give a pretty decent summary of what I need to do.

- -> Rick
- Goal: Set up a `/store/index.js` file
- Sources: 
    - [Nuxt Docs - Store directory](https://nuxtjs.org/docs/directory-structure/store/)
    - [Learn Vuex in 30 MINUTES! (Vue JS 3)](https://www.youtube.com/watch?v=nFh7-HfODYY)
- Been staring at nonsensical articles and documentation for awhile. [I've gotten this far](https://github.com/sait-wbdv/winter-2022/commit/66378af5d5ac378be78560bd9e1915b95d33b743)
    1. How do I initialize these `store` hooks to invoke [`getLessons()`](https://github.com/sait-wbdv/winter-2022/blob/66378af5d5ac378be78560bd9e1915b95d33b743/store/index.js#L12-L16)?
    2. How do I load the data just once when the app loads?
    3. Once the data is loaded, how do I access the `store` from the component?
- I'm giving up on the page nav for now. I posted to Pixels but no response yet.

## Jan 5, 2022
Goal: [Master schedule by week array](https://github.com/sait-wbdv/winter-2022/issues/16)
- After talking with Ash, I'm going to try a quick and dirty brute force of a Schedule by week that will work in the template
- Plan
    1. Create an additional array of weeks, each with lesson list
    2. export that array to the `template`
    3. Output component with two `v-for` (one for weeks and the other for lessons)
- Brute force: [initial commit](https://github.com/sait-wbdv/winter-2022/blob/b1f27a016e07490320cea1b1c73ade2d69bedc10/pages/schedule.vue#L65-L66)
- I've got [the week array mostly working](https://github.com/sait-wbdv/winter-2022/blob/3f7573e66b37e62ec37039a0679fd860bf3fa73e/pages/schedule.vue#L65-L80) but there's a bug that I think is caused by the hacky way I calculated weeks. But that's good enough to move on.
- TODO: Add new issue about the bug
- -> Morty

Goal: Learn some `v-for`, and output the buggy nested array I just built :)
- I'm weak on `v-anything` atm so setting up [a new branch with a test `v-for`](https://github.com/sait-wbdv/winter-2022/commit/c8d5ae89cac580de110271407a6d9d06190c4ff1)
- Noice. Got it working; just have to figure out how to output `index + 1` in the heading
    - Search: "v-for index"
    - 
- Noice! In hindsight, maybe a branch wasn't needed.
    - [base functionality](https://github.com/sait-wbdv/winter-2022/commit/ca1d16c8cc81dd72d2c9a24e3df33d378067dd1d)
- -> Rick
    - Ugh, now I have to merge and delete the branch
    - [merged commit](https://github.com/sait-wbdv/winter-2022/commit/ca1d16c8cc81dd72d2c9a24e3df33d378067dd1d)
    - Aaaaan how do I delete a remote and local branch again (for the 50th time)?
        - Search: "git delete branch"
        - Top result (again): [How to Delete a Git Branch Both Locally and Remotely](https://www.freecodecamp.org/news/how-to-delete-a-git-branch-both-locally-and-remotely/)

            ```shell
            // delete branch locally
            git branch -d localBranchName

            // delete branch remotely
            git push origin --delete remoteBranchName
            ```
- [Cleaned up the code and added the date](https://github.com/sait-wbdv/winter-2022/blob/1e536d673188ae579fb9631cf9de0619abedf8d9/pages/schedule.vue)

Goal: Output pretty dates on the Schedule
- research: [content:file:beforeInsert](https://content.nuxtjs.org/advanced/#contentfilebeforeinsert)
    - cleanup: `beforeInsert` is only for saving data to the file
    - `beforeParse` is for changing data before it's used in the app but you only have access to the raw front matter.
- Given the limitations of the Content module, I ended up just explicitly setting the time (adjusted for UTC) on every lesson.
- I find that the Content module breaks down quickly for end cases.

Goal: Give individual lessons access to a global index of lessons for Prev/Next links.
- Research: As soon as I need data that is accessed by mutliple components, it looks like I need state and `vuex` is how I do it.
- Search: "nuxt state mutation patterns"
- Top result: [
Scalable state management with Vuex and Nuxt.js](https://blog.logrocket.com/scalable-state-management-with-vuex-and-nuxt-js/)
    - Gotta be honest, I didn't read it, yet. One of the images is from the docs so I went there first: [What is a state management pattern?](https://vuex.vuejs.org/#what-is-a-state-management-pattern)
    - From there the Vuex docs gave me [When Should I Use It?](https://vuex.vuejs.org/#when-should-i-use-it):
        > "A simple [store pattern](https://vuejs.org/v2/guide/state-management.html#Simple-State-Management-from-Scratch) may be all you need"
- Pet Peeve: I'm still reading through the previous link and sample code like suffers from a common fault that I know I'm guilty of (and would like to rectify):
    ```js
    var store = {
      debug: true,
        state: {
            message: 'Hello!'
        },
        setMessageAction (newValue) {
            if (this.debug) console.log('setMessageAction triggered with', newValue)
            this.state.message = newValue
        },
        clearMessageAction () {
            if (this.debug) console.log('clearMessageAction triggered')
            this.state.message = ''
        }
    }
    ```

    - This is a bad example (for me) but a beginner will ask the question: "do I need to name it `state`" or can I choose my own name?
        - There are soooo many examples of framework code where this applies, particularly to lifecycle hooks. For example, `setup()` (I think?) is now a Vue 3 hook but how does the beginner know that it's a reserved keyword?
    - TODO: Bring this up with Ash (and team) about how to have our lessons/documentation take this into account.
    - Another example:

        ```js
        var vmA = new Vue({
            data: {
                privateState: {},
                sharedState: store.state
            }
        })
        ```
        - does `data` need to be named `data`?

## Jan 4, 2022
Goal: Test DayJS in Nuxt
- Ash installed `$dayjs` as a build module and I'm figuring out how to use it to solve the timezone issues.
- Last resort: If I can't get tz to work, I'm just going to explicitly set a full UTC datetime string as `lesson.date` in all the markdown files.
    - If you can't tell, I'm losing faith in Nuxt. Yesterday's rage still lingers.
- [setting the timezone in the config](https://www.npmjs.com/package/@nuxtjs/dayjs) so far has no effect on `item.date` output from Nuxt Content.
- According to the [timezone plugin docs](https://day.js.org/docs/en/plugin/timezone) I might have to explicitly extend DayJS:

    ```js
    var utc = require('dayjs/plugin/utc')
    var timezone = require('dayjs/plugin/timezone') // dependent on utc plugin
    dayjs.extend(utc)
    dayjs.extend(timezone)

    dayjs.tz("2014-06-01 12:00", "America/New_York")

    dayjs("2014-06-01 12:00").tz("America/New_York")

    dayjs.tz.guess()

    dayjs.tz.setDefault("America/New_York")
    ```

    - First of all, `var`? How old are these docs?
    - Going to try to use `dayjs.tz.guess()` to confirm the set timezone. Ash already listed `utc` and `timezone` as plugins in the config but how do I use `.extend(utc)` from within Nuxt?
    - [DayJS might not support global plugins](https://github.com/nuxt-community/dayjs-module/issues/182) but I just imported locally to the schedule component. It's outputting `America/Edmonton` as the timezone but the problem still seems to be the conversion Nuxt applies to front matter dates. DayJS doesn't seem to be used for this conversion.
    - It might be time for the last resort...
- [final session commit](https://github.com/sait-wbdv/winter-2022/commit/73644a6be11438e97f0d6d9b559c808c125fd354)

Goal: Add some hierarchy to the Schedule.
- Plan: 
    1. Add some draft titles to the lessons for testing.
    2. Remove duplicate "Day X" from lesson titles (if they're allowed to be empty)
    3. Add "Week X" headings to the for loop.
        - Code Spike: how to add dynamic containers for each week from a flat `v-for` loop?
        - It might be easier to build weeks into the data structure.
- Content added:
    - [commit](https://github.com/sait-wbdv/winter-2022/commit/4794d171609dd9b42ca673bf478cebab1f202331)
- Now to add some "Week X" headings. This may quickly turn into a pair session with Ash.
- I notice that the `v-for` is inside a `ul` where it outputs the `li`s. I can't immediately visualize how to add conditional headings and list containers to a flat loop.
- I think my time is better spent adding pagination to the lesson pages.

Goal: Research - what's the best way to add Nuxt pagination for post dates in the future?
- Search: "nuxt pagination"
    - First red herring: [Adding Pagination With Nuxt Content](https://redfern.dev/articles/adding-pagination-nuxt-content-blog/)
        - I tried cloning [the same blog](https://github.com/garethredfern/nuxt-basic-blog) but it won't boot.
        - Moving on...
- Most of the tutorials are specific to blogs whereas we're an edge case. 
    - dates are often in the future
    - no pagination nav needed besides Prev/Next links
    - content is spread out over multiple content modules and manually concatenated.
- I think the easiest solution would be to simply add `prev` and `next` properties to the `lessons` array. 
    - `index - 1` and `index + 1` should give the loop access to the `dir` and `slug` properties.
    - Problem: how do we share the dynamic data from `schedule.vue` with the individual lessons?
- It's looking more and more like we need a master store for the program schedule that will power the Schedule and lesson Prev/Next nav?

Goal: Migrate layout templates from `fall-2021` to `winter-2021`
- Trying to copy the structure as much as possible so I removed the separate primary and utility nav. Merging into one primary nav is good enough and will let me keep most of the styling.
    - Pair code with Ash: add Utility nav later
- Question: when to use `nuxt-link`?
    - Search: "nuxt-link vs a"
    - A: [nuxt-link is for all internal links](https://nuxtjs.org/docs/features/nuxt-components/#the-nuxtlink-component), use `a` for external links
- Question: what goes into `static` and what goes into `assets`?
    - Search: "nuxt assets vs static"
    - [`assets` is processed by Webpack](https://stackoverflow.com/questions/48808182/nuxt-assets-and-static-folder-when-to-use-which) but what about Vite (maybe doesn't matter)?
- Question: How do I link to internal images?
    - Search: "nuxt img src"
    - Looks like [`~/assets` is the way to go](https://nuxtjs.org/docs/directory-structure/assets/)
- Moving on to adding css to the `head` using `vue-meta` (I think).
    - Noice. Ash links [to the docs](https://nuxtjs.org/docs/configuration-glossary/configuration-css/) again.
    - Hooking up the css was almost trivial.
- I know I said I was going to keep the original design of the header but I can't help myself.
    - Inspiration: [CSS Tricks](https://css-tricks.com/)
        - It's a nice, compact design that runs fro end to end of the page. That'll leave room for the extra links and the Utility nav can go to the far left.
- Done!
    - [finishing commit](https://github.com/sait-wbdv/winter-2022/commit/58bff0eac85803e1f543141898dedcfa7e20a896)

## Jan 3, 2022
Goal: Split the WBDV schedule into Weeks
- [Repo](https://github.com/sait-wbdv/winter-2022)
- [Issue](https://github.com/sait-wbdv/winter-2022/issues/5)
- recap: installing Luxon was problematic and DayJS is an optional build module that might work. BUT, I think I'm just going to do the Week math manually and maybe refactor for a library later.
- Got to find the article that gave me the math before:
    - Search: "js date week number"
        - Top result: [Get Week Number of the Year in JavaScript](https://www.delftstack.com/howto/javascript/javascript-get-week-number/)
        - relevant code example from article:

            ```js
            currentdate = new Date();
            var oneJan = new Date(currentdate.getFullYear(),0,1);
            var numberOfDays = Math.floor((currentdate - oneJan) / (24 * 60 * 60 * 1000));
            var result = Math.ceil(( currentdate.getDay() + 1 + numberOfDays) / 7);
            console.log(`The week number of the current date (${currentdate}) is ${result}.`);
            ```

            The code seems to be based on the 1st of Jan and then defining a week as +7 days. The Schedule only needs to count starting on the first Monday as Week 1.
- The logic shouldn't be too far off from above. Pseudo-code:
    1. Loop through sorted list of days in Schedule `script`.
    2. Define the start of Week 1 as the Monday of the first date in the list (if first day is a Tuesday, use the day before)
    3. Create date object of this Monday as `startDate`
    4. Set `week` iterator to 1.
    5. foreach date:
        1. Subtract `startDate` from `current`
        2. divide by 7 days
        3. round down
        4. If: not equal to `week` iterator, set `week` to new value
        5. set `item.week` to `week`
- Once the week is set for each item, it should be straight forward (hopefully) to build Past, Current, Upcoming weeks into the Schedule `template`.
- [starting commit](https://github.com/sait-wbdv/winter-2022/commit/04465debba12eb9ef849ad95a3ad071f47bc78a4)
- -> Morty
- Added link to pair session with Ash!
- Back to weeks thing...

[Step 1. Declare base `week` and `startDate`](https://github.com/sait-wbdv/winter-2022/blob/04465debba12eb9ef849ad95a3ad071f47bc78a4/pages/schedule.vue#L36)
- `firstDate` is better than `startDate`?
- Oooo, I almost got distracted from the goal by trying to set the lesson date to 8am. Midnight is good enough for now.

Step 2: 
- Search: "intl date day of week"
    - I really want to use `Intl` but `Date` object is good enough for now
- Managed to maybe figure out some really sloppy code but I'll test after I finish the steps. Cuz I'm crazy like that.

[ Step 3. Else: subtract `startDate` from `currentDate`, divide by 7 and round down (`currentWeek`)](https://github.com/sait-wbdv/winter-2022/blob/04465debba12eb9ef849ad95a3ad071f47bc78a4/pages/schedule.vue#L39)
- I'm pretty proud of this one. Steps 3 and 4 complete in one line:

    ```js
    week = week + (floor((currentDate - firstMon) / 24 / 60 / 60 / 7))
    ```
- Time for the reckoning: let's boot this puppy up...
    - `ReferenceError: floor is not defined`
        - lolz.
    - Well, week 1 is correct :)
    - the Date Object is kicking me in the ass. 
- Good to know - front matter dates in YYYY-MM-DD are converted to the following in Nuxt Content:
    
    ```
    2022-01-10T00:00:00.000Z
    ```
    - TODO: Set the timezone in Nuxt/Vue config
    - TODO: Possible to set default start time (8am) to lessons?
- taking a break
- -> Rick
- [mid-session commit](https://github.com/sait-wbdv/winter-2022/commit/3d9515fc2d9359911480f3d530b0765c2e0c7f61)
- Just realized the message for the above commit is not appropriate. Instead of "mid-session transfer to Rick", it should have been more descriptive of the code, like "buggy - added first draft of Schedule by Week"
- Logic error 1:
    - `week` was proceeding from 1, to 144, to 430, to 589 but not sure why
    - `week` is now returning NaN after playing with some date formatting
    - going to try figuring out how Nuxt Content is affecting my code with it's date object
    - it looks like dates are converted to a UTC string?
    - My math is also off for subtracting a day's worth of seconds if the first day of class isn't a Monday:
        - search: "js new date minus 1 day"
        - top result: [How to subtract days from a plain Date?](https://stackoverflow.com/questions/1296358/how-to-subtract-days-from-a-plain-date)
        - it worked!
- Logic error 2
    - `firstMon` is inheriting the time that the code is running
    - going to explicitly set the time to 8am when creating `itemDate`
    - Fixed, but it's needs optimization
- Logic error 3
    - Monday is turning to Sunday because of timezone (-7 hours)
    - This was such a pain in the a$$! I tapped out and just added a day to `itemDate` to compensate for the timezone. 
    - The problem: Nuxt Content converting YYYY-MM-DD to a date object in UTC. Very frustrating.
- The proper `week` is now inserting into the lesson objects.
    - [brute force commit](https://github.com/sait-wbdv/winter-2022/commit/af33d08067fd3dcc462b14be5db07bbd04cf184a)

Walk-through
- [cleaned up commit](https://github.com/sait-wbdv/winter-2022/commit/68b5bcd69334fabccf71531f39559695d09e1e7a)
    - Will eventually fix a [potential bug with adding a day for timezone](https://github.com/sait-wbdv/winter-2022/blob/68b5bcd69334fabccf71531f39559695d09e1e7a/pages/schedule.vue#L43-L44) but not today

Reflection
- In hindsight, I should have spent more time figuring out how to add a proper datetime library to Nuxt. Nuxt auto converting the YYYY-MM-DD dates in the lesson front matter burned soooo much time.

## Jan 2, 2022
- [Pair Code with Ash](https://hackmd.io/Brs_kxvrQbuHCB1NlVzqbw)

## Jan 1, 2022
- Recap from last night: ended up working on Rick, where I didn't clone the code journal
    - Added placeholder lessons so I could work on the schedule
    - At some point, Nuxt broke and is giving the error:
        ```
        TypeError: Unsupported field type for full text search
        ```
    - It's very irritating that the location of the error isn't shown; just the stack trace from within the framework. Ugh.
- Goal: Since I didn't commit at any point, I have to copy the current files and gradually drag them over from scratch in order to narrow down the issue. I should have done a test build early to make sure my new placeholder files were working :/
- This is why I don't like branches: there's no way to copy files to a new branch without copying a branch to a new folder.
- Fixed! I was using `[description here]` as a placeholder in the front matter, which is reserved for arrays in YAML. Still sucks that the errors wouldn't tell me that.
- I deleted the Dasa days during troubleshooting last night which I'll have to add back manually.
- BUT, first I'm going to commit :)
- Added Dasa days and committed. Moving on...

Goal: fetch all lesson days and merge into a single array.
- Plan: going to try `Promise.all()` to pull all lessons using code from existing Courses pages.
- Done. It was fairly straight forward but the `.then` syntax was yucky so figured out how to do it with `await`; much nicer.

Goal: sort an array of objects by date
- Plan:
    1. figure out how Nuxt handles dates
    2. sort by date
- I read a little into setting up filters and decided to go native `Date` object. Sorting is a little magical but the syntax is pretty straight forward.

Goal: Divide a list of sorted days into weeks.
- First decision: do I do this in `script` or `template`?
    - Since I'm not familiar with Vue syntax, I'll do this in vanilla js and build a nested array of weeks so I can move complete weeks to a different container in the `template`.
- Next decision: Do I go native `Date` again or try Luxon or something?
    - While it would be fun to figure out how to calculate week number programmatically, I'll need to install a date library eventually so I might as well try Luxon.
- Got a weird "fromMarkdown is not a function error" when running `npm run dev` which side tracked me.
    - switched to main branch and it still gave the error.
    - deleting `package-lock.json` and `node_modules` with an install and audit finally fixed the issue (for now). Not sure how this could have broken?
- Not sure how to install luxon as a nuxt module? I've `npm install vue-luxon` but adding it to the Nuxt `plugin` property in the config gives me a "plugin not found" error.
    - most likely have to add this on the vue level but need to learn more about how that works within Nuxt.
    - Starting to feel the rage. This must be how the students must feel. Constant searching on Google is failing me "nuxt vue-luxon", and any variation on it, returns the same list of useless pages.
        - Note for teaching: rage quitting is real. Right now I want to punch every Nuxt dev in the face for making shitty assumptions about me.
    - Docs to the rescue! Unless this page sucks.
        - [Nuxt Plugins directory](https://nuxtjs.org/docs/directory-structure/plugins/)
- Successfully installed `vue-luxon` (I think) but hitting a roadblock trying to access it from a component. 
    - [`vue-luxon` docs](https://github.com/casbloem/vue-luxon) say use `this`:

        ```js
        this.$luxon("2020-10-05T14:48:00.000Z")
        ```

        But `this` is not defined.
    - no variations I can think of are working :(
        - [current broken code](https://github.com/sait-wbdv/winter-2022/commit/62096aa839e1bf3bc4f2a9ada421a26d57a1c695)

Goal: Install `vue-luxon` locally, or I'll try installing vanilla luxon.
- After a break, and some research, I found this tutorial:
    - [How to make use of Vue.js Plugins in Nuxt - [Vue-awesome-swiper] ](https://dev.to/olawanle_joel/how-to-make-use-of-vue-js-plugins-in-nuxt-2bao)
- Ugh, giving up on Luxon for now. Going to try doing the week-number math manually.
    - [current commit](https://github.com/sait-wbdv/winter-2022/commit/d3f2b0082477e16a16e30591b3370aaf529a54d6)

Goal: Auto prepend the lesson plan titles with the course code without having to add it to every markdown file.
- Does the nuxt content module support directory json/yaml files that contain default front matter variables for lessons?
- It doesn't look like it? 
- Backup plan: build a superset yaml file that feeds the fetch loop for all the content that's date centric
    - lessons
    - holidays
    - assignment due dates
    - important milestones 
        - "last day to get 4 dailies in before end of course"
- didn't get very far. 

## Dec 31, 2021
Goal: Add lesson placeholders for building WBDV W22 Schedule.
- Plan: Start with all of Jan, plus Dasa Days and the first day of each course transition.
- forgot to commit last night. Starting back on the main branch and had to get rid of the changes. 
    - R+D: `git stash` 

## Dec 30, 2021
### Session: Nuxt with extended markdown
- [Repo](https://github.com/sait-wbdv/winter-2022)
- Goal: Test `markdownit` install
- where's the documentation?
- Nice! Ash linked to [the documentation](https://github.com/remarkjs/remark/blob/main/doc/plugins.md#list-of-plugins) in `nuxt.config.js`.
- Search: `remark vs markdown-it`
    - featured: [Super nerdy critique in favour of remark](https://github.com/benrbray/noteworthy/discussions/16)
    - `remark` looks pretty cool
    - damn, it doesn't look like there's a way to add classes?
    - never mind! It's all about [`remark-directive`](https://github.com/remarkjs/remark-directive) (for now, until all the new plugins are ported to remark version whatever)
- Anyway, back onto testing definition lists.
- can't seem to get it to work. Going to try adding `remark-directive` to see if I can narrow this down.
- `remark-directive` doesn't work either. I think it's time to give up for the night and ask Ash. Maybe a pair coding session?

