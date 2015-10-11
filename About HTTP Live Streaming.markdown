# About HTTP Live Streaming

- Concepts 
HTTP Live Streaming (HLS)

**What is HLS?**
using the HTTP protocol to delivery content.

## HLS Workflow
1. A video encoder which supports HLS receives a live video file;
2. The encoder creates multiple versions(variants) of that file at different resolution;
3. Then the encoder segments the version into small files, at the same time, it makes a files conained URLs pointing to these small files, meanwhile, the encoder send the files to a web server; (files connection: *master playlist* => *media playlist* => *media segments*)
4. How to access the video content? 
- Embedding a link to the *master playlist* in web page
- or download the *master playlist* in your own application

## Media Segments
**How does the encoder creat the media segments?**
By dividing the event data into short MPEG-2 stream files.

## Media Playlist Files
- *file format:* M3U, the encoder save the media playlist as text files in M3U format.
- *saving:* the files contained URLs to the media segments and playback information.
- *types:* live, event or video on demand (VOD), determines how the stream can be navigated. 
 - *live:* fast forward and reverse playback within a limited time range.
 - *event:* can rewind to the beginning of the stream.
 - *VOD:* can fully navigate from beginning to end.

## Live and Event Media Playlists
These two type playlists need to update the newly created media segments available to the server. 
The encoder adds the new media segments reference to the playlists and upload the updated playlists to the server.

**live media playlist sample**

    #EXTM3U
    #EXT-X-VERSION:6
    #EXT-X-TAGETDUARTION:10
    #EXT-X-MEDIA-SEQUENCE:26
    #EXTINF:9.901,
    http://media.example.com/wifi/segment26.ts // old media segment
    #EXTINF:9.901,
    http://media.example.com/wifi/segment27.ts // newly media segment
    #EXTINF:9.501,
    http://media.example.com/wifi/segment28.ts // newest meida semgent

In this live playlist, the older reference (URL) can be removed.

**event media playlist**

    #EXTM3U
    #EXT-X-VERSION:6
    #EXT-X-TARGETDURATION:10
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-PLAYLIST-TYPE:EVENT // here's differenct from live
    #EXTINF:9.9001,
    http://media.example.com/wifi/segment0.ts
    #EXTINF:9.9001,
    http://media.example.com/wifi/segment1.ts
    #EXTINF:9.9001,
    http://media.example.com/wifi/segment2.ts
The older media segments will NOT be removed from the meida segments.

**VOD media playlist**

    #EXTM3U
    #EXT-X-VERSION:6
    #EXT-X-TARGETDURATION:10
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-PLAYLIST-TYPE:VOD // Here's the VOD media playlist.
    #EXTINF:9.9001,
    http://media.example.com/wifi/segment0.ts
    #EXTINF:9.9001,
    http://media.example.com/wifi/segment1.ts
    #EXTINF:9.9001,
    http://media.example.com/wifi/segment2.ts
    #EXT-X-ENDLIST // Tell you the end of the meida segments
A VOD playlist contains the references to all media segments.

## Master Playlists
### Playlist Relationship

![Playlist Relationship](https://developer.apple.com/library/mac/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/Art/stream_playlists_2x.png)

**A master playlist with four variants**

    #EXTM3U
    #EXT-X-VERSION:6
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=2855600,CODECS="avc1.4d001f,mp4a.40.2",RESOLUTION=960x540
    live/medium.m3u8 // variant one
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=5605600,CODECS="avc1.640028,mp4a.40.2",RESOLUTION=1280x720
    live/high.m3u8 // variant two
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=1755600,CODECS="avc1.42001f,mp4a.40.2",RESOLUTION=640x360
    live/low.m3u8 // variant three
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=545600,CODECS="avc1.42001e,mp4a.40.2",RESOLUTION=416x234
    live/cellular.m3u8 // variant four
A player downloads the master playlist **only once**, however, the number of **media playlist** downloads is different which depend on the playlist type, live, event or VOD.

## Publishing Your HLS Streams
**How to access the stream?**

- By adding a link to the *master playlist* in the web page.
- Download the *master playlist* in your own application built with the AV Foundation or Media Player frameworks.

## Key Features

- Closed Captions and Subtitles
- Fast Forward and Reverse Playback
- Alternate Audio and Video Renditions
- Fallback with Stream Alternates
- Timed Metadata
- Ad Insertion
- Content Protection

## Requirements for iOS Apps
- If your app delivers video over cellular networks, you must use HLS.
- If your app uses HLS over cellular networks, you must provide the 64Kbps or lower bandwidth.




