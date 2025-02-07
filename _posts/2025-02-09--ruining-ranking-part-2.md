---
title: How to ruin your site reputation in two easy steps (part 2)
layout: default
---

## How to ruin your site reputation in two easy steps

### Part 2: convince Google to ignore your site

After clearing my criminal record as described in [part 1](/2025/02/06/ruining-ranking-part-1), I was looking forward to see my site indexed again on Google. One day passed, then two, but nothing changed: googling for keywords associated to my post yielded no results, even when I was searching for unique sentence fragments that were likely to only appear on my pages. Curiously, Bing and related search engines (like my favorite, [DuckDuckGo](duckduckgo.com)) were showing my site with no problem. 

I would have fumbled in the dark for quite a long time, were it not for the good design decision of some Google engineer, who decided to pay tribute to the lost art of discoverability.

<center>
<a href="/assets/2025-02-09--ruining-ranking-part-2/site-not-indexed.png">
<img src="/assets/2025-02-09--ruining-ranking-part-2/site-not-indexed.png" alt="Google showing no results, put suggesting I use its Search Console" width="700" border="1"/>
</a>
</center>

Try the Search Console, suggests Google. I had a vague idea that Google offers instruments for SEO, but I thought they were paid tools for professional webmasters. But no, the Console is free and available to anyone. It took some effort to prove ownership of the various copies of my site but, once done that, I was good to go.


<center>
<a href="/assets/2025-02-09--ruining-ranking-part-2/google-search-console.png">
<img src="/assets/2025-02-09--ruining-ranking-part-2/google-search-console.png" alt="Google Search Console" width="700" border="1"/>
</a>
</center>

The fact that I needed to add four different “properties”, as Google calls them, is due to the chaotic start of this site:
* I began to build up using Netflix's free service, creating [annali.netlify.app](annali.netlify.app)
* The success of my [post on Hacker News](https://news.ycombinator.com/item?id=42305954) brought my site into HN's mortal embrace. I burned half of my monthly bandwidth allowance in a handful of hours
* Therefore, I <em>very quickly</em> moved my site to Cloudflare, creating [annali-af6.pages.dev](annali-af6.pages.dev)...
* ...and replaced the Netlify page with a redirect to it. In the haste, instead of linking to the main site I linked to one of the pinned builds, [fe9aefb4.annali-af6.pages.dev](fe9aefb4.annali-af6.pages.dev)
* Finally, I bought the domain name [marnetto.net](marnetto.net), which is where you are probably reading this page

Linking to the pinned build was a mistake, but not a big one – so I thought. Yes, the URL was not pleasant looking, the updates on the master branch of this blog would not be reflected there and the page would sooner or later be removed by Cloudflare, but there was nothing to worry. The complicated URL was a small thing since addresses are usually not typed manually, and the other two problems were quickly solved by instructing Cloudflare to redirect the URL to my main page. Problem solved? Not for Google, as the Search Console kindly explained to me:

<center>
<a href="/assets/2025-02-09--ruining-ranking-part-2/page-not-indexed-1.jpg">
<img src="/assets/2025-02-09--ruining-ranking-part-2/page-not-indexed-1.jpg" width="350" border="1"/>
</a>
<a href="/assets/2025-02-09--ruining-ranking-part-2/page-not-indexed-2.jpg">
<img src="/assets/2025-02-09--ruining-ranking-part-2/page-not-indexed-2.jpg" width="350" border="1"/>
</a>
</center>

The crux of the matter lies in the “canonical page” system used by Google: when it finds pages that are identical or very similar, the engine indexes only one of the copies, which it calls “canonical”. Therefore:
* the pages on marnetto.net and netlify.app are not indexed because Google chose as canonical the copy available in fe9aefb4.annali-af6. 
* the page on fe9aefb4.annali-af6 is not indexed because it is marked with a “noindex” tag, as Cloudflare automatically does for pinned builds

So we are in a Pirandellian situation: my site is available under three or four different URLs, but Google only wanted to index one of them, and ended up indexing none! 

Once understood the problem, the solution is simple: first I redirected the URL of the pinned build to the main domain name, then I requested Google to re-visit the pages. I thought the issue would be fixed in a matter of minutes, but I was disappointed: it took more than one week for the crawler to fulfill my re-index request. Such kind of delay is surely a good way to prevent abuse by SEO professionals, but it was frustrating for me. Anyway, I am not in the position to handle with Google; around New Year's Eve my site was fully relisted. I had my happy end, and this is what matters.

## Postscript

I used the Search Console to fix a bug, but I found it useful in general and I recommend every site owner to check it out. Not only it points out if a page has issues that prevent it to be indexed, but it shows which user queries led your site to be displayed in Google searches. My favorite function, however, is the list of sites that are providing incoming links.

<center>
<a href="/assets/2025-02-09--ruining-ranking-part-2/incoming-links.png">
<img src="/assets/2025-02-09--ruining-ranking-part-2/incoming-links.png" alt="Top linking sites for marnetto.net" width="500" border="1"/>
</a>
<figcaption>My audience is very international.</figcaption>
</center>

Not only the list allowed me to enjoy reading a couple of articles speaking about my Stunt Island modding efforts, but it led me to discover the site [Two Stop Bits](https://twostopbits.com), a Hacker News clone focused on retrocomputing. A very pleasant discovery that I take as reward for my effort.

Now that I have fixed my SEO issues, it's time to go back to more familiar technology domains. See you soon for my writeup about modding Brøderbund's Stunts. You can already download the provisional fruits of my work in the [dedicated page](/projects/stunts.html), or try to beat my mediocre driving skills in the running [ZakStunts Championship](https://zak.stunts.hu/).


