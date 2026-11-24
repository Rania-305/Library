## Library Final Project Proposal

## What I am building

A personal library site where I log the books I read, write down *why* I loved or quit each one, and get a "read next" pick based on my actual history.

## Who it is for

It's for me first, and then a few friends and family members who'll test it this week.

My problem is that the books I actually want to read (Islamic history, Seerah, some business and startup stuff) are hard to find through normal recommendation apps. Those apps mostly push whatever's popular. I also forget what I thought about a book a few months after I finish it, so I end up picking my next read pretty randomly. I want one place that remembers what I read, why it worked for me or didn't, and uses that to point me to the next one.

## The API

- **Name:** Open Library Search API (free, no key needed)
- **Docs:** <https://openlibrary.org/dev/docs/api/search>
- **URL I opened in the browser:**
  <https://openlibrary.org/search.json?q=ottoman+empire&fields=key,title,author_name,first_publish_year,cover_i,edition_count&limit=5>
- **Fields I'll use from the response:**
    - `numFound`: how many results matched, so I can show "142 books found"
    - `docs`: the list of books that came back
    - `key`: the book's ID, which I'll save to someone's shelf
    - `title`: the book title
    - `author_name`: the author(s)
    - `first_publish_year`: when the book first came out
    - `cover_i`: the cover ID, which I can turn into an image link (`https://covers.openlibrary.org/b/id/{cover_i}-M.jpg`)
    - `edition_count`: how many editions exist, which I'm using as a rough "how established is this book" signal

## What the user chooses

- **The search:** you type a topic, title or author ("seerah," "ottoman empire," "startups"), and the page shows matching books with covers.
- **A topic filter:** quick buttons for the topics I care about. Clicking one changes the search and the results.
- **A sort:** newest, oldest, or most editions.
- **A status for each book you save:** *Want to read*, *Reading*, *Finished*, or *Quit*, plus a one-line reason ("too slow," "loved the storytelling"). This choice changes what shows up on your shelf and what the page recommends next.

## What the page works out

The page builds a **ranked "Read Next" list** for whoever's logged in. It takes the books in your *Want to read* pile and the latest search results and scores them from your history. Books by authors you finished go up, books by authors you quit go down, and books in topics you read a lot of get a boost. Edition count breaks ties.

The top pick also gets a short explanation written by a model (the DeepSeek API we used in class). It's based on your saved reasons, something like "You quit the last two slow, academic ones, so this one is more narrative."

It also works out a few simple **shelf stats**: books finished this month, your most-read topic, and the percentage of books you quit vs. finished.

## Features

1. **Log in and get your own shelf.** Each person only sees their own books and notes.
2. **Search Open Library** by topic, title or author, with covers, filters and sorting.
3. **Save books with a status and a reason.** This is the "memory" part that makes the recommendations personal.
4. **A Read Next ranking** with a model-written "why this one" for the top pick.
5. **Shelf stats** so you can see your reading habits at a glance.

*If I have time:* a shared shelf so a small group (like friends or an MSA reading circle) can see what everyone's reading and get a group pick.

## What I do not know yet

- How to set up login and save each person's shelf to a database so it's still there next time they come back.
- How to write the scoring logic for the Read Next list in JavaScript and then sort by it.
- How to call DeepSeek from the site without exposing my API key in the browser.
- How to keep the page from sending a new request on every single letter someone types.