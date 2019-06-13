
Timed Metadata in Common Media Application Format (CMAF)
========================================================
{:.no_toc }

Version 1.1  
6/13/2019

## Abstract
{:.no_toc }

[FIXME]


## Contents
{:.no_toc }

* TOC
{:toc}


## Introduction

HTTP Live Streaming (HLS) [RFC8216] supports the inclusion of timed metadata in
ID3 format [ID3], carried in the CMAF-compatible HLS stream [ISO_23000_19]. This
document describes how ID3 metadata is carried as timed metadata in
CMAF-compatible fragmented MP4 (fMP4) stream as used by the HLS protocol.


## Overview

Timed Metadata in CMAF-compatible HLS stream is signaled via one or more Event
Message boxes ('emsg') [ISO_23000_19] per segment. Version 1 of the Event
Message box [ISO_23009_1] must be used.

Event messages with "https://developer.apple.com/streaming/emsg-id3" scheme will
identify boxes that carry ID3v2 metadata [ID3].


### Event Message Box ('emsg')


#### Introduction

One or more Event Message boxes ('emsg') [ISO_23000_19] can be included per
segment. Version 1 of the Event Message box [ISO_23009_1] must be used.

Event messages with "https://developer.apple.com/streaming/emsg-id3" scheme will
identify boxes that carry ID3v2 metadata [ID3].


#### Syntax

From [ISO_23009_1] 5.10.3.3.3

~~~~~ c
aligned(8) class DASHEventMessageBox extends FullBox('emsg’, version, flags = 0) {
    if (version==0) {
        string              scheme_id_uri;
        string              value;
        unsigned int(32)    timescale;
        unsigned int(32)    presentation_time_delta;
        unsigned int(32)    event_duration;
        unsigned int(32)    id;
    } else if (version==1) {
        unsigned int(32)    timescale;
        unsigned int(64)    presentation_time;
        unsigned int(32)    event_duration;
        unsigned int(32)    id;
        string              scheme_id_uri;
        string              value;
    }
    unsigned int(8) message_data[];
}
~~~~~


#### Semantics

`scheme_id_uri` set to "https://developer.apple.com/streaming/emsg-id3" to
identify ID3v2 metadata [ID3].

`value` may either be an absolute or relative user-specified URI which defines
the semantics of the id field. Any relative URI is considered to be relative to
the scheme_id_uri.

`message_data` contains complete ID3 version 2.x.x data [ID3].


## References

The following documents are cited in this specification

  * **[RFC8216]**  
    IETF RFC 8216 “HTTP Live Streaming”, August 2017

  * **[ISO_23000_19]**  
    International Organization for Standardization, "Information
    technology -- Multimedia application format (MPEG-A)
    -- Part 19: Common media application format (CMAF) for segmented media",
    ISO/IEC 23000-19:2018(E), 2018,

  * **ISO_23009_1**  
    International Organization for Standardization, "Information
    technology -- Dynamic adaptive streaming over HTTP (DASH)
    -- Part 1: Media presentation description and segment formats", ISO/IEC
    23009-1:2014(E): Draft third edition, 2018-07-26

  * **[ID3]**  
    ID3.org, "The ID3 audio file data tagging format"


[RFC8216]: https://tools.ietf.org/html/rfc8216
[ISO_23000_19]: http://www.iso.org/iso/catalogue_detail?csnumber=71975
[ID3]: http://www.id3.org/Developer_Information
[AV1]: https://aomedia.org/av1-bitstream-and-decoding-process-specification/

