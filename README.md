# Open Playlist Format (spec)
## 1. Front matter
```yaml
version: 0.2.0
author: Denperidge
created_on: 17/01/2024
last_updated: 21/01/2024
date_format: dd/mm/yyyy
```

## 2. Introduction
### 2.a. Problem Description
The experience of sharing music is incredibly fragmented.

#### 2.a.i. Music services: Local playback & streaming ...
Back in the early days of digital music, **local playback** was the only way to go. The best you could do to share your favourite music with your friends was links to the artist on the iTunes Store, or sending over ripped CD files.

**Music streaming** has since seen wider and wider usage. Partly partly due to convenience, and partly due to crackdowns on the illegal streaming and/or downloading of music. This has allowed for things like easy playlist sharing (this will be expanded upon later), but also further locked off.

#### 2.a.ii. ... are both important
But this does not mean that **local playback** should be discarded.
- There are many reasons for local playback:
    - Music that was only ever recorded to physical media without a proper release.
    - Music that has been taken off music streaming services.
    - People buying CD's from the artists merch stand or a local music store, wanting to be able to listen to it on their phones.
    - People with low internet availability.
- Additionally, streaming services do not guarantee whether you keep the content you paid for. Articles featuring examples on [netflix.com](https://www.netflix.com/tudum/articles/whats-leaving-netflix), [theverge.com](https://www.theverge.com/2022/7/8/23199861/playstation-store-film-tv-show-removed-austria-germany-studiocanal).
- Additionally, there are artists that *refuse* to release on specific platforms. An example can be found in the FAQ on the [free99.net website](https://free99.net/faq), stating politically motivated reasons for not releasing on Spotify.
- Streaming services are all locked in ecosystems. [This will be expanded upon later](#).

But this does not mean that **streaming services** should be discarded:
- Large install bases.
- Large playlist communities, even if locked into the platform.

#### 2.a.iii. "Hey, listen to this song/playlist!"
Sharing links to music and playlists is possible, but only if you're willing to make sacrifices:
- Ideally, those around you use the *same* services...
- ... But if one or multiple people do *not*:
    - They have to be willing to deal with *account creations*, *limited quality*, *advertisements*, and sometimes even not being able to *select* the song they want.
    - They have to *manually* find the equivalent song(s) on the platform of their choice, [if it's even there](#are-both-important).
    - You could use an external sync platform to bring your playlist to the on your friend(s) use(s), with heavy restrictions for non-premium users and subsequent account creation on every platform you may want to link to


#### 2.a.iv. Fix that
The Open Playlist Format is an attempt to create an all encompassing playlist format. A way of playlist definition that is future-proof; no matter what future artists, songs or services pop up. One that is *non-proprietary*, but can easily be used for services that are. And one that isn't a database, but contains enough data to confidently search a song in one. A format designef to be as unaffected by link rot or future changes as possible.

Use cases can then include services for automatic playlist transferring/syncing, or a website where a playlist can be shared and subsequently played/imported to any streaming service.

### 2.b. Glossary
Some words like `service` are inherently ambiguous, and as such will be clarified here. Words like `song` and `playlist` have preconceived and specific meanings, and as such, a *song or playlist presented in the Open Playlist Format will be written in `ALL CAPS`*. For example: `You can use a PLAYLIST to create a playlist in Spotify`.

- **Service**: anything that allows for *specific song playback*
    - (+) **Streaming online** (YouTube videos, Deezer)
    - (+) **Streaming locally** (e.g. Spotify local saves, internal of the app)
    - (+) **Locally downloaded songs** (e.g. ripped CD's on a file explorer or Bandcamp downloads)
    - (-) Radio stations/devices: out of scope

- **SONG**: an object containing all thats needed to
    - Back-end: get a direct link to a song.
    - Front-end: minimal display information for the user.
- **PLAYLIST**: an ordered collection of SONGS. This can be none, one or multiple.


### 2.c. Context
#### 2.c.i. Locked-in ecosystem restrictions
|
| ---------
| 

#### 2.c.ii. Existing services
- Songwhip:
    - (+) Can find songs on most services, from a search query or a link
    - (-) Does not allow playlist creations
    - (-) Still requires manual clickthrough

#### 2.c.iii. Existing formats
- XSPF (XML Shareable Playlist Format):
    - (-) Only works with local file links or remote file links, no streaming services

### 2.d. Future goals
The format should be enough to create the following:
- Create a website that (after a user configures Spotify as their main service) can redirect `/song/Powderpaint/Constellation` to `https://open.spotify.com/track/3DKPLEn6QaffVMoSdSHdRO?si=57f2ff9a159a4a16`
- Create an app that will automatically sync a PLAYLIST across multiple services
- Create a website that will display a PLAYLIST, and allows playing its songs on any service the user selects.


### 2.e. Assumptions
#### A universal code
As described later ([3.a. Design philosophies](#3a-design-philosophies)), an ISRC lookup will be the first 

## 3. Solution
### 3.a. Design philosophies
Now, there are a few things that are important:
- **Compatibility (listener):** this format should be applicable for as many services as possible in the past, present and future. This means no service specific things in the spec.
- **Compatibility (creator):** With the current state of music, songs should not be identified from what used to be considered standard. Not every song has an International Standard Recording Code. Not every artist has MusicBrainz entries. Base functionality cannot be locked behind anything aside from the most basic things every song has: a set of characters for a title and a set of characters for (a) creator(s).
    - First, check using the SONGs' ISRC (as of writing, this seems to be the most universal ID)
    - If that fails, use song name & artist name stored in a SONG
- **Minimal**: There should be as little as possible hardcoded into an OPF that can change down the line. If a song gets renamed, that's probably just a new song. But songs moving albums is very realistic.
- **Not a database**. This is a format. A database allows for manual corrections, this does not. But as such, OPF can be designed to not have a single point of failure. If Tidal shuts down, OPF should survive - and ideally, be unaffected by - the fallout.

#### 3.b. Data structure
#### SONG
Every SONG is represented as follows
```json
{
    "isrc": "ISRCODE",
    "song_name": "The title of a song. Used for basic display and/or looking up a missing ISRC, if applicable",
    "artist_name": "The main artist of a song. Used for basic display and/or looking up a missing ISRC, if applicable",
}
```

#### PLAYLIST
Every PLAYLIST is represented as follows
```json
{
    "title": "The title of the playlist",
    "description": "The description of the playlist",
    "image": "Either an url to an image, or a base64 encoding of one",
    "songs": [
        /* SONG objects: zero, one or multiple*/
    ]
}
```


```py
def by_isrc(isrc: str):
    # ...

def
```

opf-deezer should have a:
- function isrc_to_opf & names_to_opf
    - If nothing is found, return False


The following repos will be created to support the Open Playlist Format:
- `osf-to-${service}`: Allow the following actions from a .osf.json
    - `osf-to-links`: Turn a PLAYLIST object (with one or multiple songs) into links to each song on that service
    - `import-osf-playlist`: Turn a PLAYLIST object into a full-fledged playlist

- `${service}-to-osf`: Allow the following actions:
    - From a link to a song on that service, create a SONG object
    - From a link to a playlist on that service, create a PLAYLIST object

- osf-tools:
    - Implements all `osf-to-${service}` & `${service}-to-osf`

## Temp
Notes that will possibly be used or removed 
```
- For determining some variables:
    - Valid sources of information on an artist is the artists Wikipedia, official website, and services (digital music store/streaming pages). For the Wikipedia and artist websites, it can be a good idea to check it both in a language *not* in the commonly used language of the artist, and one *in* a language not usually used by them
    - Two methods will frequently pop up:
        - **Most commonly used**, when it is used in this spec, is the most commonly used spelling/term on...

        TODO finish

        #### Practical examples

        **Most commonly used artist name for Fear, and Loathing in Las Vegas ([English wikipedia](https://en.wikipedia.org/wiki/Fear,_and_Loathing_in_Las_Vegas), [Japanese Wikipedia](https://ja.wikipedia.org/wiki/Fear,_and_Loathing_in_Las_Vegas))
        ```
        Fear, and Loathing in Las Vegas
        ```
        - **Most prominently used**, which is the one used in page titles


- **Anti-ambiguous**: The 
    - A song gets an ID due to a *hash* of the song *name*, *artist* and *album*
        - A song from a single re-released in an album will as such be counted 

#### Design pitfalls
Terminology:

Pitfalls:
- Multiple people using the same artist name 
  - Example: Dog Park and Dog Park
  - Possible solution: Maybe a year_first_song_released_by_artist? This would still have some collusion chance, but it *is* immutable
  - Possible pitfall: multiple first song release dates per service. Aries Doomzilla (2016, findable in archives and old tweets), Aries DEADMAN WUNDERLAND (2017, Spotify). Might require too much digging?
  - Possible solution:  Country of origin?
  - Possible pitfall: High collusion chance
  - City of origin: not always known
- Multiple spellings of an artists name (Jake Hill, iamjakehill. Find better example i dont really want to write him into my spec). Solution: TODO
- Abbrevations (G.L.O.S.S. - Girls Living Outside Society's shit, Left At London). Solution: use the one with the LONGEST length. This lowers collusion chance for commonly used abbrevations
- Re-releasing a song under the same title (does this happen?)
- Specific spelling quirks (Måneskin) Solution: unsure. Most commonly used, or most commonly used and then normalised?
- Capitalisation: solution: use most common capitalisation, or normalise?
- Artists with different spellings
- Artists with atypical capitalisation
- Artists with translated names
    - Example 1: `https://en.wikipedia.org/wiki/Fear,_and_Loathing_in_Las_Vegas` - `フィアー·アンド·ロージング·イン·ラスベガス` ([Artist Wikipedia](https://en.wikipedia.org/wiki/Fear,_and_Loathing_in_Las_Vegas))
    - Proposable solution: select spelling of country of origin
    - Problem: multiple ways of writing the same name (see above)
    - > Solution: **artist_name** and **song_name** should use the most **common** spelling used. Check both the website and/or wikipedia of the artist in a language *you know* and *the native language of the artist*. 
- This standard should work everywhere.

| Variable name | Description | Important note |
| ------------- | ----------- | -------------- |
| song_name     | The title of the song
| artist_name   | The artists name | For maximum unambiguity, the *longest form* of the artists name must be selected at all times, with as little abbrevations and as many characters possible |


##### *_name
To ensure that the artist and song names are consistent (different spellings - of the same), the following operations must be performed
- Grab the **longest** form of the artist 
    - Do *not* 
- Capitalisation *does not* matter

Examples:
- 


Every song has an opf_id, which is created in the following method:
- The song name and artist name run through the following regex:  [TODO: Songs that don't use the alphabnert]
```

## Notice
This spec was developed under the structure laid out in Zara Cooper's article on StackOverflow, which can be read on [stackoverflow.blog](https://stackoverflow.blog/2020/04/06/a-practical-guide-to-writing-technical-specs/).
