---
title: The Big Map of Cologne Parking Meters / Die große Karte der Parkscheinautomaten in Köln
layout: default
keywords:
  - cologne
  - köln
  - parking meters
  - parkautomat
  - parkautomaten
  - parkscheinautomat
  - parkscheinautomaten
  - map
---

# The Big Map of Cologne Parking Meters<br/> Die große Karte der Kölner Parkscheinautomaten

<center>
<a href="/assets/2024-12-20--cologne-parking-meters/final.jpg">
<img src="/assets/2024-12-20--cologne-parking-meters/final-small.jpg" alt="drawing" width="880" border="1"/>
</a>
<figcaption>Cologne parking meters. Click on the image for the high-resolution version.<br/>
Script to generate the map available <a href="https://github.com/AlbertoMarnetto/annali-code/tree/master/2024-12-20--cologne-park-meters-map"><b>here</b></a><br/>
Map data from <a href="openstreetmap.org/copyright">OpenStreetMap</a><br/>
Powered by <a href="https://www.geoapify.com/">Geoapify</a><br/>
</figcaption>
</center>

<br/><br/>

<center>
<a href="/assets/2024-12-20--cologne-parking-meters/only-park-meters.jpg">
<img src="/assets/2024-12-20--cologne-parking-meters/only-park-meters.jpg" alt="drawing" width="440" border="1"/>
</a>
<figcaption>A black and white view of the parkmeters, outlining the heart of the city.<br/>
Thanks to <a href="https://anthonyhanswidjaja.com">Hans Widjaja</a> for the idea!
</figcaption>
</center>

## Intro

Before Corona I used to have a car, to travel to Cologne frequently for [some](https://www.sorriso-sport-tanz.de/) [Brazilian](http://brasilonia.koelnrio.de/) [dance](https://forrodecolonia.com/) [party](https://forroindeutschland.wordpress.com/2010/11/11/ein-abend-beim-forro-koln/), to dislike paying for parking and to like maps. The pandemic freed me of many of these inconvenient habits, allowing me to spend more time at home in front of a screen instead of wasting time staying outside, exercising the body, having fun and making friends.

That said, my love for maps remains, and while doing some cleanup in my old files I rediscovered a small project I did some years ago, a map of the parking meters of the city of Cologne, which I created for my strategic attempts of saving a couple of euros while going downtown on Saturday evening. Why not publish an updated edition? Maybe a couple of people might find it useful, another couple may think it looks nice, and it's a fun little project anyway.

The city of Cologne does not publish a map of the parking zones, but it kindly supports an open data project with many interesting datasets. One of them lists all parking meters in the city, with their geographical coordinates as well as their activity times and prices.

The dataset is published every several years. At the time I took the 2018 edition, now the [2023 version](https://www.offenedaten-koeln.de/dataset/parkscheinautomaten-koeln/resource/ef089b7e-1f84-4c88-a548-3868c384bf72) seems to be the most up-to-date.

<center>
<a href="/assets/2024-12-20--cologne-parking-meters/koeln-open-data-page.png">
<img src="/assets/2024-12-20--cologne-parking-meters/koeln-open-data-page.png" alt="drawing" width="640" border="1"/>
</a>
<figcaption>Just don't ever try to resize the columns!</figcaption>
</center>

The dataset lists all the geographical coordinates of the parking meters. Now we only have to superimpose it on a map. I previously used a quick-and-dirty method for that, building a Google Maps URL containing the meters coordinates as marker. The feature seems to have been removed in the last years, but one can still use the waypoints as workaround. The main limitation is that the URL length is limited and one can put at most some hundred of markers there, way less than the 2700 that we need. We can still extract a subset and plot it; useful if one is willing to filter the entries, e.g. to only select a district.

<center>
<a href="/assets/2024-12-20--cologne-parking-meters/google-maps.png">
<img src="/assets/2024-12-20--cologne-parking-meters/google-maps.png" alt="drawing" width="640" border="1"/>
</a>
<figcaption>The GMaps version of the parking meters in North Lindenthaler (
<b><a href="https://www.google.com/maps/dir/50.93652715,6.92051486/50.9365254,6.91968636/50.93650212,6.92261673/50.93648764,6.92345256/50.93627027,6.9150552/50.93624556,6.92418903/50.93613701,6.91752893/50.93610625,6.91843955/50.93604015,6.92189593/50.93588356,6.91669018/50.93588344,6.91540712/50.93565869,6.92413464/50.93555283,6.92174024/50.93553012,6.91870833/50.93552041,6.91797748/50.93551765,6.9200711/50.93549982,6.92382122/50.93549971,6.92271288/50.9352021,6.91348715/50.93437764,6.91361947/50.93397718,6.92327274/50.93387705,6.92207665/50.93383527,6.91367939/50.93375522,6.92446041/50.93360982,6.91884134/50.9334617,6.92205657/50.93314338,6.92204833/50.93289051,6.92048303/50.93259027,6.91387408/50.93256,6.91519942/50.93223304,6.91843763/50.93205103,6.91496965/50.93181237,6.91796152/50.9316878,6.91642919/50.93157724,6.91580504/50.93068614,6.91481648/0.93180819,6.91700844/50.9366814,6.9100094/50.9366627,6.91107/50.9366117,6.9123504/50.9361844,6.9128619/50.93615,6.91006/50.93607,6.91095/50.9359139,6.9093242/50.93556158,6.90816338/50.935551,6.9131808/50.93527422,6.90812583/50.93517,6.90948/50.93498,6.91111/50.9349637,6.9106849/50.9348373,6.908427/50.9345593,6.9106856/50.9345593,6.9106856/50.9344919,6.91297778/50.93402028,6.91151259/50.9338,6.91074/50.93379,6.91005/50.93373676,6.91152723/50.93360033,6.91291673/50.93359903,6.91215556/50.93343954,6.90866901/50.93309221,6.91108704/50.93301145,6.91116293/50.932944,6.9107243/50.93292728,6.91371572/50.9328684,6.91265665/50.9327484,6.9082802/50.93266032,6.90787951/50.9325156,6.9096527/50.9325006,6.9083116/50.93241746,6.91271474/50.9323803,6.90804/50.9323803,6.90804/50.9322796,6.9118099/50.93224168,6.9078898/50.9319697,6.9133083/50.93184253,6.90956896/50.93183598,6.90790589/50.9317885,6.9083186/50.9317754,6.9112734/50.93173194,6.9125352/50.93159,6.91385/50.9315196,6.9089674/50.93148952,6.91139624/50.93145422,6.91218601/50.93145398,6.91063154/50.9313579,6.909931/50.9312195,6.9090012/50.9312165,6.910588/50.9310163,6.9126102/50.93091131,6.91455906/50.93090352,6.90978359/50.93075833,6.91167883/50.93070337,6.90804/50.93062762,6.90985187/50.93053522,6.91187291/50.93052191,6.91266802/50.9304817,6.910631/50.92994893,6.91292013/50.9298073,6.9108939/50.92968324,6.90846641/50.92957,6.91011869/50.92904687,6.91139373/50.9285128,6.9104123/51.0016992,6.8784514/50.994806,6.8725288/50.9946562,6.8724891/50.9662454,6.9432774/50.9662454,6.9432774/50.9662454,6.9432774/50.940812,6.934948/50.940812,6.934948/50.937531,6.9602786/50.937531,6.9602786/50.937531,6.9602786/50.937531,6.9602786/50.937531,6.9602786/50.937531,6.9602786/50.937531,6.9602786/50.937531,6.9602786/50.937531,6.9602786/50.9354467,6.8411195/50.93293,6.92355/50.932363,6.9218077/50.9319174,6.9194854/50.9317375,6.9198049/50.9315108,6.9206916/50.9315108,6.9206916/50.9315108,6.9206916/50.9315108,6.9206916/50.9310135,6.9254469/50.9309974,6.9189757/50.9309737,6.9174195/50.9309666,6.9189124/50.9308,6.9219/50.9307843,6.9197927/50.9307544,6.9187831/50.9306823,6.9205561/50.9304339,6.9169132/50.9303931,6.9185945/50.930388,6.9171593/50.9302052,6.9267002/50.9301258,6.9250274/50.9300985,6.9201948/50.93005,6.91792/50.9300388,6.9199983/50.9300011,6.9261241/50.9300011,6.9261241/50.9297967,6.917191/50.9296072,6.9195555/50.9295232,6.9265223/50.9294664,6.9203245/50.9293302,6.9272048/50.9293302,6.9272048/50.9293302,6.9272048/50.9293302,6.9272048/50.9293302,6.9272048/50.92924,6.91801/50.92909,6.9222575/50.92909,6.9222575/50.9290831,6.9204186/50.9289906,6.925609/50.9289615,6.9238775/50.9287948,6.92551/50.9287948,6.92551/50.9287948,6.92551/50.9286164,6.9227803/50.9286164,6.9227803/50.9286164,6.9227803/50.9286164,6.9227803/50.9286164,6.9227803/50.9280716,6.9244173/50.9280716,6.9244173/50.9280716,6.9244173/50.9280716,6.9244173/50.9280716,6.9244173/50.9280716,6.9244173/50.9280716,6.9244173/50.9280716,6.9244173/50.9279194,6.9279692/50.9276289,6.9251481/50.9276289,6.9251481/50.9276289,6.9251481/50.9274071,6.9244993/50.9264458,6.9195736/50.9264458,6.9195736/50.9264458,6.9195736/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9263208,6.9205319/50.9262648,6.9080419/50.925865,6.9224815/50.925865,6.9224815/50.9253154,6.9199217/50.9253154,6.9199217/50.9252797,6.9285039/50.9252797,6.9285039/50.9252797,6.9285039/50.9252797,6.9285039/50.9251879,6.9314166/50.9251879,6.9314166/50.9251139,6.9233451/50.9249381,6.9291388/50.92493,6.92525/50.92481229,6.92015827/50.9245201,6.9268614/50.9244537,6.9265223/50.92445,6.92542/50.9244388,6.924738/50.9244006,6.9282037/50.924304,6.9234157/50.924304,6.9234157/50.9241368,6.9233084/50.9241368,6.9233084/50.9238711,6.9261887/50.9236225,6.9214649/50.9233843,6.924214/50.9233814,6.9223731/50.9233814,6.9223731/50.9233814,6.9223731/50.9233771,6.9250049/50.9232092,6.9212953/50.9232092,6.9212953/50.9232092,6.9212953/50.9229854,6.9234141/50.9228581,6.923519/50.9662454,6.9432774/50.9662454,6.9432774/50.9442827,6.8982895/50.9319174,6.9194854/50.9319174,6.9194854/50.9319174,6.9194854/50.9304513,6.916064/50.9298331,6.9145076/50.9294665,6.913584/50.9291043,6.9127087/50.9287472,6.9115768/50.9287317,6.9108661/50.9281779,6.9101087/50.9271135,6.9091993/50.9266219,6.910059/50.9264229,6.909787/50.9252941,6.9110131/50.9220574,6.9208918/50.92164,6.9206/50.9206837,6.9172933/50.957161,6.970983/50.956345,6.972462/50.952707,6.9689153/50.95101512,6.96714252/50.949246,6.9652736/50.9979228,7.0373173/50.9979228,7.0373173/50.981464,7.03348/50.9756693,7.0734583/50.975594,7.074815/50.97559152,7.07449218/50.9749005,7.0741653/50.97361,6.97057/50.9717778,7.0104277/50.9708922,7.0482464/50.970039,7.046017/50.970039,7.046017/50.96960718,7.04513047/50.9691265,7.0154871/50.9691265,7.0154871/50.9691265,7.0154871/50.9691265,7.0154871/50.9691265,7.0154871/50.9691265,7.0154871/50.9691265,7.0154871/50.96559,7.0093876/50.96559,7.0093876/50.9654877,7.0096848/50.9650241,7.0075819/50.96467155,7.00878147/50.9643889,7.0109729/50.9643889,7.0109729/50.9641817,7.0170767/50.9639592,7.0005502/50.9639142,7.0022696/50.963674,7.015116/50.963647,7.0155398/50.963506,7.013694/50.963388,7.015445/50.9633774,7.015386/50.9632102,7.0143239/50.9631914,7.0142927/50.9629993,7.0128665/50.9628951,7.0110519/50.9628,7.01064/50.962773,7.011345/50.962765,7.011788//@50.93686078,6.97249396,11.29z?entry=ttu&g_ep=EgoyMDI0MTIxMS4wIKXMDSoASAFQAw%3D%3D">link</a></b>
) </figcaption>
</center>

## The proper way

If we want a full map and more flexibility, we need to do things properly:

* download a static map of the city (a map where the coordinates of the corners are known)
* convert the parking meter coordinates into X-Y coords in the image
* plot them on the map

For the first part, static maps can be found in many places. Google has [Maps Static API](https://developers.google.com/maps/documentation/maps-static/overview) and Microsoft has [Bing Maps](https://learn.microsoft.com/en-us/bingmaps/articles/geospatial-endpoint-service). However it is easier, and safer from the legal point of view, to use OpenStreetMap (OSM). There are [lots of options](https://wiki.openstreetmap.org/wiki/Static_map_images) to get static maps from the OSM database, I go with [GeoApify](https://www.geoapify.com/).

The URL to get a static map is relatively straightforward:

```
https://maps.geoapify.com/v1/staticmap?style=osm-bright&width={map_pixel_size[0]}
&height={map_pixel_size[1]}&center=lonlat:{map_center.lon},{map_center.lat}
&zoom={zoom}&apiKey={api_key}
```

Getting suitable sizes and zoom needs some attempts. GeoApify does not crop the requested map size to a max value (Bing supports 2000x1500 max), but it seems to timeout if the values are too big. At the end, I resort to download nine maps with 2200x1800 resolution in a 3x3 matrix, and collate them. The resulting image is 6600x5400, which is enough to cover almost the whole city of Cologne at my desired zoom size.


Converting the coordinates is not hard, one just needs to have a couple of formulas at hand. The first one is documented on [StackExchange](http://gis.stackexchange.com/a/127949/93220) and [Bing Maps](https://docs.microsoft.com/en-us/bingmaps/articles/understanding-scale-and-resolution), the others are basic geometry and algebra:

```
meter_per_pixel = 1. / 2 * 156543.03392 * cos(latitude_in_deg * pi / 180) / (2**(zoom))
meter_per_deg_northing = 6378137. * 2 * pi / 360
meter_per_deg_easting = 6378137. * 2 * pi / 360 * cos(latitude_in_deg * pi / 180)
deg_per_pixel_northing = meter_per_pixel / meter_per_deg_northing
deg_per_pixel_easting = meter_per_pixel / meter_per_deg_easting
```

The rest follows quite easily. Copilot, which wasn't of much help in my [previous project]({% link _posts/2024-11-20--tweaking-stunt-island.md %}), this time is quite helpful in showing me how to plot the markers on the map using the [Pillow library](https://pypi.org/project/pillow/).

I can encode some extra information using color and shape of the markers, so I use shape/size to distinguish the expensive parking zones from the very expensive ones, and the color to give some indication about the free parking times. There are not less than forty-nine different time indications:

```
$ cat psa_offene_daten_2023.csv | awk -F';' '{ print $8 }' | sort | uniq | wc -l

49
```

<br/>
<center>
<a href="/assets/2024-12-20--cologne-parking-meters/strange-times.png">
<img src="/assets/2024-12-20--cologne-parking-meters/strange-times.png" alt="drawing" width="640" border="1"/>
</a>
<figcaption>I am sure that they had a good reason to set these times, but good luck remembering them.<br/>
Also, notice now the file still uses the Windows-1252 encoding instead of Unicode. I could set a legacy encoding and display the umlauts correctly, but I refuse to do so in 2024.
</figcaption>
</center>

<br/>

At the end, in the spirit of my weekend trips, I decide to show in which times of the weekend one can park for free, to help the partygoers to better plan their trip.

## Conclusion

That, my reader, I leave to you. My own main takeaway is that the section of Luxemburgerstraße just outside the railroad bridge seems still to offer free parking. I do not need this information anymore, but it saved me some pennies when I went dancing to [Tanzraum](https://www.tanzschule-tanzraum.de/) in Barbarossaplatz. Hope you can also get some useful info. In any case, see you next time!

P.S. The source code to generate the map is available in [<b>this repository</b>](https://github.com/AlbertoMarnetto/annali-code/tree/master/2024-12-20--cologne-park-meters-map)
