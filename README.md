# Library Index

This is a media library indexer for NodeJS servers to asynchronously traverse a root folder, expecting artist folders within it, and album folders within them, to catalog any media files that it finds.  After cataloging the media the indexer will save the JSON data, optionally compressed with GZIP.

Multiple folders can be indexed.  Each will contain their own JSON catalogs, that merge into a single catalog when loading.

- [Supported folder structure](#supported-folder-structure)
- [Indexing media from command line](#indexing-media-from-command-line)
- [Indexing media with NodeJS](#indexing-media-with-nodejs)
- [Using the media index with NodeJS](#using-the-media-index-with-nodejs)
- [Library data structure](#library-data-structure)

## Supported folder structure

/library/artist/file
/library/artist/album/file
/library/artist/album/subfolders/file

[Top of page](#)

## Indexing media from command line 

    $ git clone https//github.com/openaudioserver/library-scanner
    $ cd library-scanner
    $ npm install

Scan then load library path:

    $ node indexer.js scan /path/to/music
    $ node indexer.js load /path/to/music

Scan a library with multiple folders:

    $ node indexer.js scan /path/to/music /path/to/more/music /path/to/other/music
    $ node indexer.js load /path/to/music /path/to/more/music /path/to/other/music

Compress the index with gzip:

    $ GZIP=true node indexer.js scan /path/to/music
    $ node indexer.js load /path/to/music

[Top of page](#)

## Indexing media with NodeJS

    const libraryIndex = require('@openaudioserver/library-index')

Run the indexer:

    await libraryIndex.index([
      '/path/to/music',
      '/path/to/more/music',
      '/path/to/other/music'
    ])

Load the library:

    const library = await libraryIndex.load([
      '/path/to/music',
      '/path/to/more/music',
      '/path/to/other/music'
    ])

[Top of page](#)

## Using the media index with NodeJS

Library objects can be retrieved with an ID value.  When using this method arrays of ids will be converted into names.

    library.getObject(id)

Library arrays can be filtered, sorted, and paginated for you.  Any array field can be used as a filter with different matching methods.  When using this method arrays of ids will be converted into names.  Default matching is `equal` and default sort is `asc`.

    library.getObjects(array, {
      sort                 string
      sortDirection        string asc|desc
      offset               integer
      limit                integer
      keyword              string
      keywordMatch         string equal|start|end|exclude|contain
      <field>              string
      <field>Match         string
    })

[Top of page](#)

## Library data structure

This is the data structure of the index.

    LIBRARY OBJECT {
      media [{
        type               track
        id                 string
        path               string
        size               integer
        title              string
        comment            string
        displayTitle       string
        sortTitle          string
        year               integer
        artists            array [ artistids ]
        composers          array [ composerids ]
        genres             array [ genreids ]
        album              string
        albumFolder        string
        albumArtist        string
        artistFolder       string
        libraryPath        string
        bitRate            integer
        codec              string
        fileContainer      string
        duration           float
        lossless           boolean
        numberOfChannels   integer
        sampleRate         integer
      }],
      albums [{
        type               album
        id                 string
        name               string
        displayName        string
        sortName           string
        artists            array [ artistids ]
        composers          array [ composerids ]
        genres             array [ genreids ]
        tracks             array [ trackids ]
      }],
      artists [{
        type               artist
        id                 string
        name               string
        displayName        string
        sortName           string
        albums             array [ albumids ]
        composers          array [ composerids ]
        genres             array [ genreids ]
        tracks             array [ trackids ]
      }],
      composers [{
        type               composer
        id                 string
        name               string
        displayName        string
        sortName           string
        albums             array [ albumids ]
        artists            array [ artistids ]
        composers          array [ composerids ]
        genres             array [ genreids ]
        tracks             array [ trackids ]
      }],
      genres [{
        type               genre
        id                 string
        name               string
        displayName        string
        sortName           string
        albums             array [ albumids ]
        artists            array [ artistids ]
        composers          array [ composerids ] 
        tracks             array [ trackids ]
      }]
    }


[Top of page](#)
## License

MIT
