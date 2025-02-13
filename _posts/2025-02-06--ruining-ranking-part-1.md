---
title: How to ruin your site reputation in two easy steps (part 1)
layout: default
---

# How to ruin your site reputation in two easy steps

My blog is a newborn one and still almost unknown. My experience with search engine optimization is basically zero, but this is not a problem – I am not aiming to be on the top of any web search. 

That said, I was surely not happy when my site suddenly disappeared from Google search for good. After spending hours to find out why it happened, I discovered that I had managed to commit not one but two crimes against visibility. I haven't managed to return my few blog pages to their previous ranking, but at least I have learnt some lessons - and got material for a new war story.

## Part 1: become a dangerous cybercriminal

<center>
<a href="/assets/2025-02-06--ruining-ranking-part-1/benigni.png">
<img src="/assets/2025-02-06--ruining-ranking-part-1/benigni.png" alt="Benigni in Johnny Stecchino, holding a banana" width="400" border="1"/>
</a>
<figcaption>Forgive me this one meme image. <a href="https://www.imdb.com/title/tt0102164/">This film</a> is one of the pillars of Italian pop culture of my generation.</figcaption>
</center>

### The discovery

My problems did not start with Google search. On December 17th, I got a mail from Netlify, my host, stating that they took down my site for violating the TOS. At first I thought that they suspected piracy because of my [Stunt Island patch](/2024/11/20/tweaking-stunt-island.html#download). I was much more surprised when I typed the address of my site and got this:

<center>
<a href="/assets/2025-02-06--ruining-ranking-part-1/ublock-says-my-site-is-malware.png">
<img src="/assets/2025-02-06--ruining-ranking-part-1/ublock-says-my-site-is-malware.png" alt="A uBlock screen saying my site is blocked" width="600" border="1"/>
</a>
</center>

My site was in a blacklist? Apparently yes. Some furious clicking led to see my URL listed in the [URLhaus Database](https://urlhaus.abuse.ch/browse/). I did not take a screenshot, but the addition is recorded in [their log](https://urlhaus.abuse.ch/downloads/misp/de271768-61c5-432a-a10d-892dab5a4ba4.json).


```
            {
                "type": "url",
                "category": "Payload delivery",
                "to_ids": true,
                "timestamp": 1734347842,
                "uuid": "526ac1e9-bb9f-11ef-afe5-82aee05be84a",
                "comment": "Malware distribution site",
                "value": "https://annali.netlify.app/assets/2024-11-20--tweaking-stunt-island/si-8x.zip",
                "Tag": [
                    {
                        "name": "Rozena",
                        "colour": "#def27c",
                        "exportable": true,
                        "hide_tag": false
                    }
                ]
            },
```

As I suspected, my Stunt Island patch led me in trouble, not because of copyright issues but because it is mistakenly detected as malware. I had a couple of contacts telling me that their browser would prevent them from downloading the file because it resembled a virus, but it had not crossed my mind that this could led me to be banned from polite company.

It was not only URLhaus which had me marked as criminal. The site Virustotal has a comprehensive list of detections, and showed that 10 different security programs were blocking access to my Netlify site. Thankfully, my new home marnetto.net, hosted on Cloudflare, was still on and was only blacklisted by two products, but this was a meager consolation since the totality of my incoming links was pointing to the Netlify URL.

<center>
<a href="/assets/2025-02-06--ruining-ranking-part-1/virustotal-url-blocked.jpg">
<img src="/assets/2025-02-06--ruining-ranking-part-1/virustotal-url-blocked.jpg" alt="A Virustotal screenshot reporting my site is detected as malware by 2 antivirus software" width="350" border="1"/>
</a>
<a href="/assets/2025-02-06--ruining-ranking-part-1/virustotal-url2-blocked.jpg">
<img src="/assets/2025-02-06--ruining-ranking-part-1/virustotal-url2-blocked.jpg" alt="A Virustotal screenshot reporting my patch is detected as malware by 10 antivirus software" width="350" border="1"/>
</a>
</center>


### The cleanup

There was not much to do but try to get myself removed by at least some of these lists. Fortunately, Virustotal has a very practical [contact directory](https://docs.virustotal.com/docs/false-positive-contacts) for false positives. I had not time to write to 42 different vendors, however, so I decided to focus on the most important things:
* Contact URLhaus and ask them to remove me from their blocklist
* Contact Bitdefender and GData and request them to adjust their virus detection engines
* Ask Netlify to put my site online again
* Remove the zip file containing my patch and only leave an encrypted 7z version, that cannot be scanned by automatical tools

Concerning the last point, I considered rewriting the patch in a slightly different way, so that it does not trigger the antivirus software. But the effort seemed not justified and anyway the damage was done.

My efforts to contact blocklist managers and vendors were more successful than expected. URLhaus whitelisted me in a few hours, and both Bitdefender and GData also updated their scanners very quickly. Netlify was very responsive in putting my site back online, after making me promise never to publish malware again (complied, my site there is now just a redirect to marnetto.net). 


<center>
<a href="/assets/2025-02-06--ruining-ranking-part-1/mail-from-bitdefender.png">
<img src="/assets/2025-02-06--ruining-ranking-part-1/mail-from-bitdefender.png" alt="Bitdefender mail confirming I am clean" width="600" border="1"/>
</a>
<figcaption>Some good news, a nice Christmas present</figcaption>
</center>

My domain name has now a [immaculate reputation](https://www.virustotal.com/gui/domain/marnetto.net) again; the [report for the Netlify URL](https://www.virustotal.com/gui/domain/annali.netlify.app) looks less great (flagged as malicious by 11 products as I am writing this, even more than during the crisis) but I hope that people will eventually update their links and bypass it completely.

Harder to repair is the downranking of my site. One day before the incident my Stunt Island writeup was on page #2 when googling for the game, now it's as good as invisible. Lesson learned: before submitting original binaries I'll always scan them in advance, and thanks to Virustotal doing that is quite fast and simple.

See you soon for [step 2](/2025/02/09/ruining-ranking-part-2.html), where I compounded my first error with another mistake, granting myself complete invisibility from Google for a couple of weeks.
