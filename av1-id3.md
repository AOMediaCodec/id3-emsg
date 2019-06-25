
Timed Metadata in the Common Media Application Format (CMAF)
============================================================
{:.no_toc }

Editors Draft, 25 June 2019

##### This version: 
{:.no_toc }
   https://aomediacodec.github.io/av1-id3/

##### Issue tracking: 
{:.no_toc }
   [GitHub]  

##### Editors: 
{:.no_toc }

   Krasimir Kolarov, kolarov@apple.com
   John Simmons, johnsim@microsoft.com 

##### Copyright
Copyright 2019, The Alliance for Open Media

Licensing information is available at http://aomedia.org/license/

The MATERIALS ARE PROVIDED “AS IS.” The Alliance for Open Media, its members, and its contributors expressly disclaim any warranties (express, implied, or otherwise), including implied warranties of merchantability, non-infringement, fitness for a particular purpose, or title, related to the materials. The entire risk as to implementing or otherwise using the materials is assumed by the implementer and user. IN NO EVENT WILL THE ALLIANCE FOR OPEN MEDIA, ITS MEMBERS, OR CONTRIBUTORS BE LIABLE TO ANY OTHER PARTY FOR LOST PROFITS OR ANY FORM OF INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES OF ANY CHARACTER FROM ANY CAUSES OF ACTION OF ANY KIND WITH RESPECT TO THIS DELIVERABLE OR ITS GOVERNING AGREEMENT, WHETHER BASED ON BREACH OF CONTRACT, TORT (INCLUDING NEGLIGENCE), OR OTHERWISE, AND WHETHER OR NOT THE OTHER MEMBER HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## Abstract
{:.no_toc }

How ID3 metadata is to be carried as timed metadata in Common Media Application Format (CMAF) compatible fragmented MP4 streams using Event Message ('emsg') boxes.


## Contents
{:.no_toc }

* TOC
{:toc}


## Introduction

HTTP Live Streaming (HLS) \[[RFC8216]\] supports the inclusion of timed metadata in ID3 format [ID3] in various container formats \[[TM-HLS]\].

A large ecosystem has built up around carrying timed ID3 metadata in HLS for applications such as ad delivery & audience measurement. Companies in this ecosystem include Disney, Sony, and Nielsen. There are many benefits to adopting CMAF for HLS media delivery, but without a specification for carrying ID3 as sparse timed metadata in CMAF, deployment by these companies is blocked.

This document describes how such ID3 metadata is carried as timed metadata in a CMAF-compatible fragmented MP4 (fMP4) stream \[[ISO_23000_19]\] as used by the HLS protocol.

## Overview

Timed Metadata in a CMAF-compatible HLS stream is signaled via one or more Event Message boxes ('emsg') \[[ISO_23000_19]\] per segment. Version 1 of the Event Message box \[[ISO_23009_1]\] must be used. 

Event messages with the scheme specified in this document will identify boxes that carry ID3v2 metadata \[[ID3]\].


### Event Message Box ('emsg')


#### Introduction

One or more Event Message boxes ('emsg') \[[ISO_23000_19]\] can be included per segment. Version 1 of the Event Message box \[[ISO_23009_1]\] must be used. 


#### Syntax

For convenience, the follow box definition is reproduced from \[[ISO_23009_1]\], section 5.10.3.3.3. 

~~~~~ c
aligned(8) class DASHEventMessageBox extends FullBox('emsg', version, flags = 0) {
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

`scheme_id_uri` set to "https://aomedia.org/emsg/ID3" to identify ID3v2 metadata \[[ID3]\].

`value` may either be an absolute or relative user-specified URI which defines the semantics of the id field. Any relative URI is considered to be relative to the scheme_id_uri.

`message_data` contains complete ID3 version 2.4 data \[[ID3]\].



## References

The following documents are cited in this specification.

#### Normative References

  * **\[[ISO_23000_19]\]**  
    International Organization for Standardization, "Information
    technology -- Multimedia application format (MPEG-A)
    -- Part 19: Common media application format (CMAF) for segmented media",
    ISO/IEC 23000-19:2018(E), 2018, <http://www.iso.org/iso/catalogue_detail?csnumber=71975>.

#### Informative References

  * **\[[RFC8216]\]**  
    IETF RFC 8216 "HTTP Live Streaming", R. Pantos, Ed. Apple, Inc. W. May, MLB Advanced Media, August 2017

  * **\[[ISO_23009_1]\]**  
    International Organization for Standardization, "Information
    technology -- Dynamic adaptive streaming over HTTP (DASH)
    -- Part 1: Media presentation description and segment formats", ISO/IEC
    23009-1:2014(E): Draft third edition, 2018-07-26

  * **\[[ID3]\]**  
    ID3.org, "The ID3 audio file data tagging format", <http://www.id3.org/Developer_Information>.


[RFC8216]: https://tools.ietf.org/html/rfc8216
[ISO_23000_19]: http://www.iso.org/iso/catalogue_detail?csnumber=71975
[ID3]: http://www.id3.org/Developer_Information
[TM-HLS]: https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/HTTP_Live_Streaming_Metadata_Spec/Introduction/Introduction.html
[ISO_23009_1]: https://www.iso.org/standard/65274.html
[GitHub]: https://github.com/AOMediaCodec/av1-id3/issues
