# Open Playlist Format (spec)
## 1. Front matter
```yaml
version: 0.1.0
author: Denperidge
created_on: 17/01/2024
last_updated: 17/01/2024
date_format: dd/mm/yyyy
```

## 2. Introduction
### a. Overview
Currently, music - from the listeners side - is incredibly fragmented. Partly due to convenience and partly due to crackdowns on illegal streaming/downloading, streaming services now have a wider and wider usage. Sharing music and playlists is possible, but only if your friends are on the same music platform. Unless you're willing to deal with oftenly heavy restrictions outlined in [`2.c.i: Context: locked in ecosystem restrictions`](#locked-in-ecosystem-restrictions), from forced account signups to not even being able to specifically select the song you wish to listen to.

The Open Playlist Format is an attempt to create an al encompassing playlist format. One that is non-proprietary, yet is designed to allow anyone using it to quickly find links to every possible service the user might use, without the drawbacks of some of the currently existing services (see [`2.c.ii: Existing services`](#existing-services)).

Use cases can then include services for automatic playlist transferring/syncing, or full on a website where a playlist can be shared and subsequently played/imported to any streaming service.

### b. Glossary
Some words like `service` are inherently ambiguous, and as such will be clarified here. However, words like `song` and `playlist` have preconceived and specific meanings, and as such, a song or playlist presented in the Open Playlist Format will be written in `ALL CAPS`. For example: `You can use a PLAYLIST to create a playlist in Spotify`.

- **Service**: any music service, whether it is for streaming or downloading.
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
#### SONG
Every SONG is represented as follows
```json
{
    "name": "The title of a song",
    "artist_id": "An artist id, including the artist name but - to assure no colussions - a hash of external_lookup_id. The hash method used will be written down here as soon as decided",
    "external_lookup_id": "This will be used to lookup the song across services"
}
```

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
