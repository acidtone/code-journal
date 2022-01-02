# Code Journal
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

## Dec 31, 2021
Goal: Add lesson placeholders for building WBDV W22 Schedule.
- Plan: Start with all of Jan, plus Dasa Days and the first day of each course transition.
- forgot to commit last night. Starting back on the main branch and had to get rid of the changes. 
    - R+D: `git stash` 

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