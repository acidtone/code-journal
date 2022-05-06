# Tony's Code Journal
Learnings, reminders and frustrations written in the moment.

---

## May 5, 2022
**Project: Danger Carpet**
- **Goal**: Refactor the original tutorial code that fetches the markdown files, BUT instead use page endpoints.
- **Plan**: 
    1. Migrate some content from the legacy journal to create some dated markdown posts.
    2. Get it working with the original code
    3. Refactor it to use only page endpoints without the `load()`, if possible.
- **Brute force**:
    - Added some posts. A format decision I need to make:
        - Should there be a 1-to-1 for posts per day? For example, I might have two sessions on the same day that don't relate to each other.
            - Option 1: one file per day named `MMM-MM-DD.md`
            - Option 2: multiple posts allowed per day but will have to add more to the file name, like `YYYY-MM-DD-some-title.md`
        - Not sure which I like best yet. One grievance is I need to repeat the date in the file name and in the front matter. It's a slight inconvenience but also rubs against my goal of lightening quick posting. Right now I just add a date to a mile-long README.
    - `import.meta.glob` is a nice SvelteKit feature? 

---

## May 4, 2022
Thinking about data visualization. 

### Data Visualization Proof-of-concepts
**Walk-through: Chartjs getting started**
- **Goal**: Display a demo of chartjs in the centre of a Codepen.
- **Plan**: 
    1. Use a CDN to import Chartjs;
    2. Find some code in the docs I can copy and paste to get a cart to show.
    3. Sprinkle some CSS 
- Brute force:
    1. [The Docs](https://www.chartjs.org/docs/latest/) reference [the JSDelivr CDN](https://www.jsdelivr.com/package/npm/chart.js)
    2. Copy and paste the HTML link into new Codepen
        
        ```html
        <!-- HTML -->
        <script src="https://cdn.jsdelivr.net/npm/chart.js@3.7.1/dist/chart.min.js"></script>
        ```
    3. Paste [this code](https://www.chartjs.org/docs/latest/#creating-a-chart) underneath:
        ```html
        <canvas id="myChart" width="400" height="400"></canvas>
        <script>
        const ctx = document.getElementById('myChart').getContext('2d');
        const myChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
                datasets: [{
                    label: '# of Votes',
                    data: [12, 19, 3, 5, 2, 3],
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255, 99, 132, 1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });
        </script>
        ```
    4. And we have a responsive `400px`x`400px` `canvas` element that is scrolling off the bottom of the screen.
        ```css
        #myChart {
          display: block;
          width: 25vw;
        }
        ```
        - This does not work. It seems the library is setting `width` and `height` imperatively based on screen width?
        - It looks like [it's 100% the width](https://www.chartjs.org/docs/latest/configuration/responsive.html) of its container by default.
    5. To set the size and position, I just need to style a container:
        
        ```html
        <!-- HTML -->
        <div class="container">
        <canvas id="myChart" width="400" height="400"></canvas>  
        </div>        
        ```

        ```css
        /* CSS */
        .container {
            display: flex;
            align-items: center;
            
            width: 50vw;
            min-height: 100vh;
            margin: auto;
        }
        ```
- Walk-through
    - Codepen: [Hello Chartjs](https://codepen.io/acidtone/pen/dydowod)
        - TODOs: 
            - Move the embedded `script` code to the JS Panel.
- Optimization
    - completed TODOs
- Reflection
    - Judging by my `git log` it took me about an hour to complete and optimize the ChartJS walk-through (while watching _Fringe s02e15_).
    - I'm tempted to get a Proof-of-concept using my GitHub `commit` tracker but I'm going to find and attempt a D3 tutorial. I don't think there's a Getting Started in the Docs.

**Treasure Hunt: D3 Tutorial**
**Goal**: find the quickest and easiest demo of the d3.js library using a CDN.
- If possible try to refactor for Codepen later.
- There are [tempting examples](https://observablehq.com/@d3/sunburst) in the Docs but you need to sign-in to fork them? It'd be nice to have a direct link to a repo.
- Abandoning the Docs for the moment. May Google find a way...
- A quick search for "d3 getting started" gave me this article:
    - [Short Guide to Getting Started with D3](https://www.pluralsight.com/guides/guide-getting-started-with-d3)
    - It seems pretty straight forward but doesn't provide any finished code.
    - I think I can just copy/paste the code after "_Putting Everything Together_" this in a Codepen?
- **Brute Force: Tutorial Attempt**
    - This tutorial still uses `var` and version `5.9.2`. Yuck.
    - But at least it works on version `7.4.4`.
    - Styling is a little icky but [it works](https://codepen.io/acidtone/pen/BaYNvMy).

### Data Visualization with `fetch()`
- **Goal**: Create a commits/day graph using the data returned by the GH Activity API:
    ```shell
    GET https://api.github.com/users/acidtone/events
    ```
- For this one, I'm creating an `index.html` file and adding it as a gist.
- **Plan**:
    1. Get [this Codepen](https://codepen.io/acidtone/pen/BaYNvMy) to work in a stand-alone page. 
    2. Fetch an array from the above API url;
    3. `.map()` the array to a count per day that matches the demo data.
    4. Pull the d3 code into the fetch block and use the `.map()` array as the data source;
    5. Cross fingers.
- **Brute force**
    - `index.html` worked on the first try. nbd.
    - Now, the hard part. Figure out how to reduce the raw GH data to a commits/day array.
        - Will be missing days that don't have commits but I'll figure that out later.
    - Whew, I had to use lodash to group the array by date using `_.groupBy()` but [I got it working](https://gist.github.com/acidtone/614775031e87c5bae223d404c3fd9fda).
- **Walk-through**:
    - There's got to be a cleaner way to reduce the raw data array but that's a job for another time.
    - Going to try moving the working code to Codepen.
- **Optimize**
    - Done! [Added a Codepen](https://codepen.io/acidtone/pen/gOvpEmX)

---

## May 4, 2022
Found this really great discussion about CSS and HTTP; the inflammatory named: [Is .css a bad idea? Is inlining the way forward?](https://www.youtube.com/watch?v=3sMflOp5kiQ)

---

## May 3, 2022
O.M.G. I just rediscovered opening a project in VS Code from the terminal with 

```shell
$ code .
```

When zsh said, `command not found`, I was getting ready for a battle. But apparently you can just [set it up from within VS Code](https://stackoverflow.com/questions/29955500/code-is-not-working-in-on-the-command-line-for-visual-studio-code-on-os-x-ma)!

Anyhoo, it's time to get my personal site up, starting with the journals.
- [coding](https://github.com/acidtone/code-journal) and [teaching](https://github.com/acidtone/teaching-journal) set as Categories;
- tags (ex. `mdsvex`) for easy browsing;
- dates for page title and sorting.
- two pages
    - home
        - post list
        - pagination
    - about
        - "Behold! My stuff" would be great but David Parker took it first.
        - links to Twitch, Meetup, Discord

**David Parker: sveltekit markdown nerd**
This dude's website is part of the inspiration, and conveniently created with SveltKit and MdSvex.
- [davidparker.com](https://www.davidwparker.com)
    - He only wrote 3 posts but they look like good ones:
        - [Dark Mode in SvelteKit with and without JavaScript](https://www.davidwparker.com/posts/dark-mode-in-sveltekit-with-and-without-javascript)
        - [How to make an RSS feed in SvelteKit](https://www.davidwparker.com/posts/how-to-make-an-rss-feed-in-sveltekit)
        - [A Fresh Beginning. Hello SvelteKit!](https://www.davidwparker.com/posts/2021-06-01-a-fresh-beginning-hello-sveltekit)
    - He's also got a lot of [great Youtube content](https://www.youtube.com/user/iamdavidwparker).
    - He attributes these mdsvex repos:
        - [c-bandy repo](https://github.com/furudean/website)
            - also "only" three posts, interestingly
            - beautiful colors
        - [@mikenikles repo](https://github.com/gitpod-io/website)

### Project: Danger Carpet
**Project Goal**: Get some content published on `tonygrimes.com` that:
- demonstrates a code journal;
- can be sustainably updated;
- easy to reference past posts

**Plan it out**
1. Install fresh SvelteKit with mdsvex
2. Create _First Post!_ in `/routes/journal`
    - step-by-step of the setup process
3. Create index page
    - Why I keep a journal
4. README: Attributions
    - [Let's learn SvelteKit by building a static Markdown blog from scratch](https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog)
    - [c-bandy repo](https://github.com/furudean/website)

**Brute force: notes**
A summary of the steps outlined in the above blog post for eventual inclusion in the first post(!)
1. Create project:
    ```shell
    $ npm init svelte@next tonygrimes.com
    ```
    - Skeleton App
    - No to the things
2. Install the things
    ```shell
    npm install
    ```
3. Create `src/routes/journal/index.svelte`
4. Create `src/routes/__layout.svelte`
    - Skipped adding `Header` and `Footer` components since neither are re-used in the project.
5. Add `src/lib/styles/main.css`
    - Just adding a couple resets for now
6. Import `main.css` into `__layout.svelte`
    ```js
    <!-- __layout.svelte -->
    <script>
    import '$lib/styles/style.css'
    </script>
    ```
7. Skipping SASS, obvs.
8. Install `mdsvex` and 
    ```shell
    $ npm i -D mdsvex svelte-preprocess
    ```
9. Configure `mdsvex`
    ```js
    import sveltePreprocess from 'svelte-preprocess'
    import { mdsvex } from 'mdsvex'

    const config = {
    kit: { /* Kit options here */ },
    
    extensions: ['.svelte', '.md'],

    preprocess: [
        sveltePreprocess(),
        mdsvex({
        extensions: ['.md']
        })
    ]
    }
    ```
10. Created "_First Post!_"
    - In an attempt to simplify the actual creation of a journal entry, I'm using only the `date` in file names and front matter.
11. Add a journal layout that auto adds the date as a level one heading.
    - Create `routes/journal/_post.svelte`
        ```js
        // svelte.config.js

        /* Imports here */

        const config = {
        /* ...Other config properties here */

        preprocess: [
            sveltePreprocess(),
            mdsvex({
            extensions: ['.md'],
            layout: {
                journal: 'src/routes/blog/_post.svelte'
            }
            })
        ]
        }
        ```
    
    - Add layout content
        ```js
        <!-- _post.svelte -->
        <script>
        export let date
        </script>

        <h1>{date}</h1>

        <slot /> 
        ```
        - TODO: Process the date into something more readable.
12. Uh oh: This blog post is using the `load()` method for the API. I think I'll [stop here](https://github.com/acidtone/tonygrimes.com/commits/main) and figure out if I can work out how to do the same thing with the newer page endpoint feature.

---

## April 30, 2022
- TODO: Walk-through of [Svelte Components Using Custom Markdown Renderers](https://www.youtube.com/watch?v=ZiEROAqobwM)
    - Deep dive into how to parse markdown strings for custom patterns
    - User Story: As a coding instructor, I'd like to add functionality to my code highlighter so that I can:
        - select code sections
        - add the page the code is from
        - toggle between ES5/ES6, named/anonymous functions, options/composition API, etc
    - Uses Stackblitz for code sharing:
        - [Custom Tokenizer And Renderer (forked)](https://stackblitz.com/edit/sveltejs-kit-template-default-jwtb9f?file=src%2Froutes%2Findex.svelte)
        - [Custom Tokenizer And Renderer - Selector Version](https://stackblitz.com/edit/sveltejs-kit-template-default-jz5nyu?file=src%2Froutes%2Findex.svelte)
        - User story: As a Twitch streamer, I'd like to share finished code so that stream watchers can follow along at their own pace.

---

## April 27, 2022
### Shell shortcuts
I'd love to be able to easily navigate to my project folders with bash shortcuts?
- [How to navigate directories faster with bash](https://mhoffman.github.io/2015/05/21/how-to-navigate-directories-with-the-shell.html)
    - Things this article taught me
        - `cd -` goes to the previous directory. 
            - Using it repeatedly will toggle between two directories! I can use this to toggle between a project and this journal.
    - `alias ..="cd .."` adds a shortcut!
    - `!$` is an alias for the last argument of the previous command
        ```
        mkdir -p make/new/directory
        cd !$
        ```
        - Had to look up the `-p` flag; it's for creating nested directories
    - `CDPATH` seems cool but the article doesn't explain it very well.
    - I stopped at `pushd`/`popd`. They looked pretty cool, but maybe I'll revisit if I need to write a bash script.
- Found: [bash directory shortcuts](https://unix.stackexchange.com/questions/1469/bash-directory-shortcuts)
    - I forgot I switched to zsh. It apparently has a feature called [named directories](https://askubuntu.com/questions/1042002/how-do-i-make-named-directories-permanent-in-zsh-and-how-do-i-edit-them-also-wh)
        - Noice; `cd ~journals` now goes to my journals directory.

### GitHub API: Activity
**User Story**: 
> As a coding instructor, I want to track the number of commits learners push so that I can give out Code Warrior trophies at the end of a course.

**Goal**
Set up a SvelteKit skeleton that logs public `GET /activity` data to the console.

**Plan**
1. Install latest SK skeleton
2. Add page endpoint for my `GET /activity` as an unauthenticated use (if I can).
3. Output a count of my latest number of commits to a page.

**Brute force**
- Source: [My Fave SK tutorial](https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog)
    - Stopping at the initial install to concentrate on the page endpoint
    - Getting an irritating error in VS Code because it can't find `adapter-auto`
- Now, how to use this [fancy new page endpoint feature](https://youtu.be/s6a1pbTVcUs?t=205) referenced in [Rich Harris's latest SvelteKit update](https://youtu.be/s6a1pbTVcUs)?
    - Wow, I just copied the code from his screen using JSON Placeholder and it worked like a charm. That's nice.
- Wow, [that was easy](https://github.com/acidtone/gh-activity/commit/b64d24b5a394d7733705c6eea08005e71555271e). SvelteKit is so awesome.
- Added a count of Push Events (`Array.prototype.reduce()`) and listed default 30 past events for my account.

**Walk-through**
This was all surprisingly straight forward. But there are some issues with the implementation:
- This probably shouldn't be tied to a page endpoint. It was great to get something up and running quickly but the final counter should be a component(s).
- This is just listing one user: me. The final app needs to compile the numbers of multiple users in a given course.
- Given the API calls required for each user, a data store should be implemented to cache responses. That's a biggie.

BUT, there's some nice data provided in the response:

```json
{
    "id": "21512451971",
    "type": "PushEvent",
    "actor": {
        "id": 6174466,
        "login": "acidtone",
        "display_login": "acidtone",
        "gravatar_id": "",
        "url": "https://api.github.com/users/acidtone",
        "avatar_url": "https://avatars.githubusercontent.com/u/6174466?"
    },
    "repo": {
        "id": 443250889,
        "name": "acidtone/code-journal",
        "url": "https://api.github.com/repos/acidtone/code-journal"
    },
    "payload": {
        "push_id": 9741321869,
        "size": 1,
        "distinct_size": 1,
        "ref": "refs/heads/main",
        "head": "4f23c9160c52bc48b273cda5db7b54bad2d53523",
        "before": "db05fa71fad1bc195d8225d6adcf6dc7c9b9dccb",
        "commits": [
            {
                "sha": "4f23c9160c52bc48b273cda5db7b54bad2d53523",
                "author": {
                    "email": "acidtone@tonygrimes.com",
                    "name": "Tony Grimes"
                },
                "message": "tweaks",
                "distinct": true,
                "url": "https://api.github.com/repos/acidtone/code-journal/commits/4f23c9160c52bc48b273cda5db7b54bad2d53523"
            }
        ]
    },
    "public": true,
    "created_at": "2022-04-28T06:40:10Z"
}
```
- TODO: Research the response returned for commits. 
    - User stories:
        > As a coding instructor, I want to find commits with two parents so that I can evaluate if someone has resolved a merge conflict.

        > As an event organizer, I want to count alternating commits between two coders on a single repo so that I can see if attendees have successfully completed a pair code session.

**Optimize**
- Added user stories and a link to this post in the [project README](https://github.com/acidtone/gh-activity/commit/5db12907ee22453891ba9eec126e32a1f8dd1628).

---

## Apr 24, 2022
Given the dead ends with xTerm, I think I'll put the idea on the shelf for now. Here are some quick wireframes I made for the record in case I can find a collaborator with xTerm experience.

![Wireframe-prompt](images/wireframes/cli-game-1.webp)
Screen 1: User is prompted to view the folder contents with `ls`.

---
![Wireframe-ls](images/wireframes/cli-game-2.webp)
Screen 2: User shows directory contents with `ls`.

---
![Wireframe-ls-l](images/wireframes/cli-game-3.webp)
Screen 3: User shows extra item info with `-l` flag.

---
![Wireframe-ls-la](images/wireframes/cli-game-4.webp)
Screen 4: User shows hidden secrets(!) with `-a` flag.

---

## April 23, 2022
**Project**: Browser-based "game" for learning the command line
- **Outcomes**
    - Navigate the file system using `pwd`, `ls` and `cd`
    - Create a mental picture of the file-system using visual cues of a traditional File Explorer/Finder
        - Where are you?
        - What is in the current directory?
        - How do I get to another location?
- **Research**: File/directory and/or command line emulation in the browser
    - [CoCalc](https://cocalc.com/)
        - Full-ish featured shell in the browser but looks like overkill
    - [xTerm](https://xtermjs.org/)
        - Looks promising! Still might be overkill, depending on how it works, but the install process looks pretty straight forward.
        - Top Question: what does the file system look like? Does it have access to the system's? Can I create a virtual one for _Follow the White Rabbit_?

**Goal**: First install of xTermjs
- It apparently requires npm to be installed; it's been awhile since I've tried using it for browser-based packages.
- It was a pretty straight forward install but key presses aren't registering in the web terminal. It feels like it needs a proper terminal to interface with?
    - Found this issue that hints to me that I have to write the simple stuff like echoing keys back to the terminal :/
    - This is good news I guess? Given my use-case, it's probably easier to add what I need rather than remove what I don't need.
    - ~TODO~: Research some tutorials on how to add functionality to xterm.
        - Results: There doesn't seem to he much, aside from [the documentation](https://xtermjs.org/docs/) and a few examples on [Codesandbox](https://codesandbox.io/examples/package/xterm).
        - I honestly don't get what this package does or is. I thought it would be something that you could throw in some connection info and you're good to go?

---

## April 14, 2022
**Goal**: Add image handling to [SAIT presentation template](https://github.com/sait-wbdv/finals)
- At first, I thought that SK's native image handling was what I wanted but it looks like you have to `import` individual images?
- The SvelteKit documentation mentions two packages if I want to add images directly to the markup:
    - [svelte-preprocess-import-assets](https://github.com/bluwy/svelte-preprocess-import-assets)
    - [svelte-image](https://github.com/matyunya/svelte-image)
- Also found mention of [vite-imagetools](https://www.npmjs.com/package/vite-imagetools). Not sure if SvelteKit can grab new image URLs and do its magic?
    - For example, if a `jpg` is processed into a `webp`, how are SK links changed to use the new image?
- `svelte-image` seems more mature and I'm not quite sure what `svelte-preprocess-import-assets` does just by looking at the documentation.
    - This is a nice tutorial for `svelte-image`: [live code: lazy loaded & responsive images in Svelte with svelte-image module](https://www.youtube.com/watch?v=FKNc9A8u2oE)
        - BUT:
            - It's old: Apr 2020
            - Was made for v0.1.9 (current is v0.2.9)
            - It doesn't use SvelteKit
- Pickin's are a little slim when it comes to guides to image processing in SvelteKit:
    - Rodney Labs
        - [Simple Svelte Responsive Image Gallery](https://rodneylab.com/simple-svelte-responsive-image-gallery/)
        - [SvelteKit Next‑Gen Background Image](https://rodneylab.com/sveltekit-next-gen-background-image/)
    - [Using `vite-imagetools` with SvelteKit](https://www.reddit.com/r/sveltejs/comments/mdd9ov/using_viteimagetools_with_sveltekit/)

---

## April 13, 2022
Continuing with SAIT template
- Goal: Migrate image assets and icons
- Question: where to put static image assets in SK?
    - Answer: [inside the `$lib`](https://kit.svelte.dev/docs/assets) 

---

## April 12, 2022
Continuing with SAIT template
- Through process of elimination (commenting code), the error is from the `badges` not being properly pass to the component 
    - Setting a default fixes it, assuming it was just the students with no badges that were breaking.
- There are loops within loops but Badges now work (without images so far). 
    - TODO: Optimize the Badges component; remove `.find()` nested in `.filter()`
    - [Finishing commit](https://github.com/sait-wbdv/finals/commit/f375e4eb6765c2894204d299dce6345b2179ec74)
- Going to have to refactor the Social link code to work as a module
    - Kept running to a weird error where this code errored out:
        ```js
        item.group = info.group
        item.icon = info.icon
        ```
        - `info.group` was undefined even though I confirmed it should work.

        ```js
        item = {...item, ...info};
        ```
        - But this DID work? Maybe something SvelteKit specific problem with the way it keeps track of assignments?
- Done with the core of the components
    - [finishing commit](https://github.com/sait-wbdv/finals/commit/5985cfc3ea78e4733ed563fb942efbef67e6e183)
    - TODO: 
        - Add image assets and Font Awesome icons
        - Migrate CSS

---

## April 11, 2022

### Refactoring SAIT final presentation template for SvelteKit
- Original: [sait-wbdv.github.io](https://github.com/sait-wbdv/sait-wbdv.github.io)
- New repo: [finals](https://github.com/sait-wbdv/finals/commit/7c8b9f66447c14127e387629663dbdcb16eccb69)
- First question: where should static json data live in SK?
    - Doesn't look like there's an official convention. Going with a data directory.
    - Going to import js instead of json.
    - Should `data` go into the `lib` directory?
        - unclear from the documentation what should go into `lib` besides modules
        - could use [`$app/stores`](https://kit.svelte.dev/docs/modules#$app-stores) but it looks like that will increase the complexity of the app.
        - Going with `$lib` for now
- TODO: Convert linklist.js into a Svelte component.
- Error importing the roster and finals list:
    ```js
    Module '"/Users/tony/Documents/sait/wbdv/finals/src/lib/data/f21/finals-f21"' has no default export.js(1192)
    ```

    - Looks like there's a requirement for default exports?
    - Apparently so.
- Next step: Convert the card templates into components.
    - Leaving that for later. Time for a break.
    - [finishing commit](https://github.com/sait-wbdv/finals/commit/ddc6d61b202c0722ce97cd7e5aeb4a85aad9adf9)

Goal: On to the next step - creating a card component.
- Just looking at the imperative DOM code in my vanilla JS makes me shiver:

    ```js
    const card = document.querySelector('#card').content.cloneNode(true);

    // Name, tagline
    card.querySelector('header').innerHTML = student.label;

    if (student.tagline) {
        card.querySelector('p').innerHTML = student.tagline;
    } else {
        card.querySelector('p').remove();
    }
    ```

    - Never thought I'd ever jump on the view framework bandwagon but SK seems like the right balance of weight to benefit. Queue cold Hell.
- At least I used HTML templates so it's kinda declarative:

    ```html
    <figure>
      <img src="" alt="[name]">
      <figcaption>
        <aside class="badges">[badges]</aside>
        <header>[name]</header>
        <p>[tagline]</p>
        <footer class="social">
          
        </footer>
      </figcaption>
    </figure>
    ```
- Having trouble with props. Getting this error when trying to loop through `students` and pass the data to my `Card` component:
    
    ```
    [HMR][Svelte] Error during component init: <Index> proxy.js:15:11
    TypeError: can't access property "$$", cmp is null
    ```
    - First of all, what fuck kind of error is this?
    - I'm assuming I'm not passing props down to the component correctly? 
        ```js
        {#each students as student}
          <Card student={student} />
        {/each}
        ```
    - Never mind. It's working now. I guess I have to reload after a hard error.
- Figuring out how to handle the badges and social links. 
    - Loops within loops. What's "better" for rendering alist of badges/social links for multiple students?
        - an array of objects
        - an object of objects
        - some kind of intermediary step?
    - I basically need to "reduce" the full array of badges into just an array of badges for a particular student so it can be handled by a simple `#each` loop. 
        - I could do this with a `.map()` but the info is split between two arrays?
    - Trying to figure out these cryptic errors:
        
        ```
        at eval (/src/lib/components/Badges.svelte:14:38)
        ```
        - `Badges.svelte` only has 10 lines of code. wtf.

        ![Unhelpful SvelteKit Error](images/misc/unhelpful-sveltekit-error.png)
    - Giving up for now.

---

## April 10, 2022

### Mood Board: Technical Blog

![Timeline History of Chris Coyier's life](images/mood/tech-blog/mood-aboutme-timeline.png)

- I like the idea and general layout. Not as much into the colours

---

![Stripe's page divider](images/mood/tech-blog/mood-asymmetry-section.png)

- Asymmetry is great, slanted page dividers is an oldie but a goodie. 

---

![screencap of cool colour gradient and logo](images/mood/tech-blog/mood-colour-gradient-val-head.png)

- Love the colours, slight gradient combined with a minimal vector logo

---

![screencap of nice hero section](images/mood/tech-blog/mood-curriculum-modules.png)

- Would be pretty easy to do with alternating grid cell alignment.

---

![screencap of simple code card](images/mood/tech-blog/mood-display-code-simple.png)

- Might make for a nice intro card to an activity/lesson/concept

---

![screencap of example displaying code](images/mood/tech-blog/mood-displaying-code.png)

- Would love to figure out how to make more complex cards that display code.

---

![screencap of vuejs tutorial](images/mood/tech-blog/mood-code-display-options-vuejs.png)

- Great implementation of changing options to see different code variations in a tutorial
- Could also work for 
    - ES5 functions vs fat arrow syntax
    - `.then`/`.catch` vs `async`/`await`

---

![screencap of Svelte Documentation page](images/mood/tech-blog/mood-layout-table-of-contents.png)

- Love the table of contents
- Is a nice example of documentation conventions.
- Could make a nice curriculum summary for a course?

---

![screencap of a warning notice on Svelte documentation](images/mood/tech-blog/mood-notice-warn.png)

- Simple example that could work for notices: warnings, pro-tips, etc.

---

![screencap of Mandy Michaels's Notist page](images/mood/tech-blog/mood-notist-mandy-michael.png)

- A little boring (not Mandy's fault; not her page) but the minimal nav for a video page could come in handy

---

![screencap of sara souedan's page seaprator](images/mood/tech-blog/mood-page-separator-sara.png)

- Just a cool example of how you could use an SVG instead of a horizontal rule.

---

### `remark-attr` with `mdsvex` and SvelteKit
**Goal**: be able to add custom classes to non-container elements (already handled by `remark-container`)
- main worry is that the `{}` syntax of `remark-attr` will conflict with Svelte template syntax.
- Continuing work on [sveltekit-mdsvex](https://github.com/acidtone/sveltekit-mdsvex)
- Plan:
    1. Add test code to `/tests/markdown` following `remark-attr` sample code
    2. Install `remark-attr` to SK and cross fingers
- Example syntax for test page:
    ```
    ### This is a title
    {style="color:red;"}
    ```

    ```
    Npm stand for *node*{style="color:red"} packet manager.
    ```
- Still new with SK. Going back to the original [MD SK tutorial](https://joshcollinsworth.com/blog/build-static-sveltekit-markdown-blog) to remind myself how to add a `remark` plugin.
    - Turns out it doesn't cover plugins? I guess I just figured it out last time:
        1. install the plugin

            ```
            npm install remark-attr
            ```
        2. Import it into `svelte.config.js`:
            ```js
            import attr from "remark-attr"
            ```
        3. Do I add it to `remarkPlugins` in `svelte.config.js`?
        4. [It worked!](https://github.com/acidtone/sveltekit-mdsvex/commit/b47fe6c4193d4ec146d5c60fe2b5c58afb1e076c)

---

## April 9, 2022
Thinking about a blog that:
- Displays code really well.
    - hightlight code selections (inline and line range)
    - show relevant page name as heading
- Can embed content the major vendors
- Shows a great table of contents
**Requirements**:
    - SvelteKit
    - Markdown
        - mdsvex
            - definitions
            - embeds:
                - Codepen
                - Youtube
                - Gists
                - Stack Overflow?
    - Notices
    - Code highlighter
        - Prism?
        - R&D: how do code highlighters work?
    - Easy image workflow.
        - screencaps
        - optimization
            - svelte-image?

---

## April 7, 2022
Goal: Install and start MongoDB locally; just in case I need it for a Mongo review session tomorrow (planning on focusing on Atlas).
- Found this tutorial that recommends Homebrew: [Installing MongoDB on a Mac](https://treehouse.github.io/installation-guides/mac/mongo-mac.html)
- I have a love-hate relationship with Homebrew. I always forget where it installs things (Cellar?) and I remember having to deal with duplicate installs when software overlaps with a package that comes with the mac (like php).
- Current Homebrew version is 3.2.6; current is 3.4.5. `brew update`; probably shouldn't be doing this on pub wifi...
- Ran into my first problem from the tutorial: 
    - "After downloading Mongo, create the “db” directory. This is where the Mongo data files will live. You can create the directory in the default location by running `mkdir -p /data/db`"
    - `mkdir -p /data/db` returned `mkdir: /data/db: Read-only file system`. No `/data` directory exists. I'm already close to giving up on local installation. More trouble than it's worth.
        - googled the error and found [this post](https://stdworkflow.com/684/mongodb-error-mkdir-data-db-read-only-file-system). It gives two methods to work around the error, but the author doesn't mention (or know) the root cause.
        - Going with Method 2 and storing the data in my home folder. Not sure why; just cuz.
        - Created a `~/data` directory in my home folder and then tried running:
            ```bash
            $ sudo mongod --dbpath=/Users/tony/data
            ```

            and, of course, received the error:

            ```bash
            sudo: mongod: command not found
            ```
            
            In general, if I have to add a `PATH` to my environment variables, I'm out. This isn't 1999.

        - Giving up on a local installation (yet again). Going to focus tomorrow's session on Atlas.

Goal: Figure out the pattern behind the flexbox albatross
- Articles
    - [The Flexbox Holy Albatross](https://heydonworks.com/article/the-flexbox-holy-albatross/)
        - > Critically, `min-width` and `max-width` override `flex-basis`.
    - [The Flexbox Holy Albatross Reincarnated](https://heydonworks.com/article/the-flexbox-holy-albatross-reincarnated/)
        - lol, `min-width` and `max-width` aren't even needed:

            ```css
            .container {
                display: flex;
                flex-wrap: wrap;
                --margin: 1rem;
                --modifier: calc(40rem - 100%);
                margin: calc(var(--margin) * -1);
            }

            .container > * {
                flex-grow: 1;
                flex-basis: calc(var(--modifier) * 999);
                margin: var(--margin);
            }
            ```

            - TODO: move `40rem` to a CSS variable.
            - Can sprinkle some >1 `flex-grow` to [create variable-width items](https://codepen.io/heydon/pen/pqGgbR)

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
            - ~~definition lists~~ [`remark-deflist`]
            - ~~element classes~~ [`remark-attr`]
            - ~~wrapper elements~~ ['remark-containers']
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
    - [commit](https://github.com/acidtone/sveltekit-mdsvex/commit/0f2b67ef4a41cbc4c2766e9b03d54f37aa9984bf)
    - [Netlify Deit/mo](https://admirable-pithivier-572d1d.netlify.app/) 

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

## March 9, 2022
Goal: Mood board practice
- Theme/product/company ideas
    - Rake Bomb Landscaping Inc.
    - Napkin Candy Designs
    - Toy Ticket Events
    - The Rain Whistle
    - Team Shadow (D&D Guild, probably mostly made up of Rogues)
    - Vomit Drum (metal band)

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

