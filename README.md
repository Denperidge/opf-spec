# Open Playlist Format (spec)
## 1. Front matter
```yaml
version: 0.1.1
author: Denperidge
created_on: 17/01/2024
last_updated: 19/01/2024
date_format: dd/mm/yyyy
```

## 2. Introduction
### a. Problem Description
The experience of sharing music is incredibly fragmented.

#### Services: Local playback & streaming ...
Back in the early days of digital music, **local playback** was the only way to go. The best you could do to share your favourite music with your friends was links to the artist on the iTunes Store, or sending over ripped CD files.

**Muisic streaming** have since seen wider and wider usage, partly due to convenience and partly due to crackdowns on the illegal streaming and/or downloading of music.

#### ... are both important

But this does not mean that **local playback** should be discarded.
- There are many reasons for local playback
    - Music only ever recorded to physical media without a proper release.
    - Music that has been taken off 
    This has allowed for things like easy playlist sharing (this will be expanded upon later).
    - People buying CD's from the artists merch stand or a local music store, but wanting to be able to listen to it on their phones.
    - People with low internet availability.
- Additionally, streaming services do not guarantee whether you keep the content you paid for. Articles featuring examples on [netflix.com](https://www.netflix.com/tudum/articles/whats-leaving-netflix), [theverge.com](https://www.theverge.com/2022/7/8/23199861/playstation-store-film-tv-show-removed-austria-germany-studiocanal).
- Additionally, there are artists that *refuse* to release on specific platforms. An example can be found in the FAQ on the [free99.net website](https://free99.net/faq), stating politically motivated reasons for not releasing on Spotify.
- Their ecosystems are all locked in. [This will be expanded upon later](#).

But this does not mean that **streaming services** should be discarded:
- Large install bases.
- Large playlist communities, even if locked into the platform.

#### "Hey, listen to this song/playlist!"
Sharing links to music and playlists is possible, but only if you're willing to make sacrifices
- Ideally, those around you use the **same services** If one or multiple people do not:
    - Listen on their own serviceThey have to *manually find* the equivalent song(s).
    - They you're willing to deal with oftenly heavy restrictions outlined in [`2.c.i: Context: locked in ecosystem restrictions`](#locked-in-ecosystem-restrictions), from forced account signups to not even being able to specifically select the song you wish to listen to.

The Open Playlist Format is an attempt to create an al encompassing playlist format. One that is non-proprietary, yet is designed to allow anyone using it to quickly find links to every possible service the user might use, without the drawbacks of some of the currently existing services (see [`2.c.ii: Existing services`](#existing-services)).

Use cases can then include services for automatic playlist transferring/syncing, or full on a website where a playlist can be shared and subsequently played/imported to any streaming service.

### b. Glossary
Some words like `service` are inherently ambiguous, and as such will be clarified here. Words like `song` and `playlist` have preconceived and specific meanings, and as such, a *song or playlist presented in the Open Playlist Format will be written in `ALL CAPS`*. For example: `You can use a PLAYLIST to create a playlist in Spotify`.

- **Service**: anything that allows for *specific song playback*
    - (+) **Streaming online** (YouTube videos, Deezer music)
    - (+) **streaming locally** (e.g. Spotify local saves, internal of the app)
    - (+) **Locally downloaded songs** (e.g. ripped CD's on a file explorer or Bandcamp)
    - (-) Radio stations/devices: out of scope
- **Most commonly used**, when it is used in this spec, is the most commonly used spelling/term on...
  - The artists wikipedia and/or website and/or store/streaming pages of a language *not* in the native language of the artist
  - The artists wikipedia and/or website and/or store/streaming pages *in* the native langauge of the artist
  - 

#### Practical examples

**Most commonly used artist name for Fear, and Loathing in Las Vegas ([English wikipedia](https://en.wikipedia.org/wiki/Fear,_and_Loathing_in_Las_Vegas), [Japanese Wikipedia](https://ja.wikipedia.org/wiki/Fear,_and_Loathing_in_Las_Vegas))
```
Fear, and Loathing in Las Vegas
```


- **SONG**: an object containing all thats needed to
    - Back-end: get a direct link to a song.
    - Front-end: minimal display information for the user.
- **PLAYLIST**: an ordered collection of SONGS. This can be none, one or multiple.


### c. Context
#### i. Locked-in ecosystem restrictions
|
| ---------
| 

#### ii. Existing services
- Songwhip:
    - (+) Can find songs on most services, from a search query or a link
    - (-) Does not allow playlist creations
    - (-) Still requires manual clickthrough

#### iii. Existing formats
- XSPF (XML Shareable Playlist Format):
    - (-) Only works with local file links or remote file links, no streaming services

### d. Future goals

The format should be enough to create the following:
- Create a website that (after a user configures Spotify as their main service) can redirect `/song/Powderpaint/Constellation` to `https://open.spotify.com/track/3DKPLEn6QaffVMoSdSHdRO?si=57f2ff9a159a4a16`
- Create an app that will automatically sync a PLAYLIST across multiple services
- Create a website that will display a PLAYLIST, and allows playing its songs on any service the user selects.


### e. Assumptions

## 3. Solution

### b. Design

Now, there are a few things that are important:
- **Compatibility (listener):** this format should be applicable for . This means no service specific things in the spec.
- **Compatibility (creator):** With the current state of music, songs should not be identified from what used to be considered standard. Not every song has an International Standard Recording Code. Not every artist has MusicBrainz entries. Base functionality cannot be locked behind anything aside from things every song has: a set of characters for a title, a set of characters for (a) creator(s), and sometimes a set of characters for the album.
- **Minimal**: There should be nothing hardcoded into an OPF. Information about oftenly changes.
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

#### SONG
Every SONG is represented as follows
```json
{
    "song_name": "The title of a song",
    "artist_name": "The artists name. ",
    "opf_id": "A unique id, hashed as described below",
}
```
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


#### PLAYLIST
Every PLAYLIST is represented as follows
```json
{
    "title": "The title of the playlist",
    "songs": [
        /* Song objects: zero, one or multiple*/
    ]
}
```

The following repos will be created to support the Open Playlist Format:
- `osf-to-${service}`: Allow the following actions from a .osf.json
    - `osf-to-links`: Turn a PLAYLIST object (with one or multiple songs) into links to each song on that service
    - `import-osf-playlist`: Turn a PLAYLIST object into a full-fledged playlist

- `${service}-to-osf`: Allow the following actions:
    - From a link to a song on that service, create a SONG object
    - From a link to a playlist on that service, create a PLAYLIST object

- osf-tools:
    - Implements all `osf-to-${service}` & `${service}-to-osf`


## Notice
This spec was developed under the structure laid out in Zara Cooper's article on StackOverflow, which can be read on [stackoverflow.blog](https://stackoverflow.blog/2020/04/06/a-practical-guide-to-writing-technical-specs/).
