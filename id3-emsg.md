
Carriage of ID3 Timed Metadata in the Common Media Application Format (CMAF)
============================================================
{:.no_toc }

v1.0.0, 6 April 2020

##### This version: 
{:.no_toc }
   [GitHub Version]

##### Issue tracking: 
{:.no_toc }
   [GitHub Issues]

##### Editors: 
{:.no_toc }

   Krasimir Kolarov, kolarov@apple.com; 
   John Simmons, johnsim@microsoft.com 

##### Copyright
{:.no_toc }
Copyright 2020, The Alliance for Open Media

Licensing information is available at http://aomedia.org/license/

The MATERIALS ARE PROVIDED “AS IS.” The Alliance for Open Media, its members, and its contributors expressly disclaim any warranties (express, implied, or otherwise), including implied warranties of merchantability, non-infringement, fitness for a particular purpose, or title, related to the materials. The entire risk as to implementing or otherwise using the materials is assumed by the implementer and user. IN NO EVENT WILL THE ALLIANCE FOR OPEN MEDIA, ITS MEMBERS, OR CONTRIBUTORS BE LIABLE TO ANY OTHER PARTY FOR LOST PROFITS OR ANY FORM OF INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES OF ANY CHARACTER FROM ANY CAUSES OF ACTION OF ANY KIND WITH RESPECT TO THIS DELIVERABLE OR ITS GOVERNING AGREEMENT, WHETHER BASED ON BREACH OF CONTRACT, TORT (INCLUDING NEGLIGENCE), OR OTHERWISE, AND WHETHER OR NOT THE OTHER MEMBER HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## Abstract
{:.no_toc }

This specification defines how ID3 metadata can be carried as timed metadata in Common Media Application Format (CMAF) compatible fragmented MP4 streams using Event Message ('emsg') boxes.


## Contents
{:.no_toc }

* TOC
{:toc}


## Introduction

HTTP Live Streaming (HLS) \[[HLS]\] supports the inclusion of timed metadata in ID3 format \[[ID3]\] in various container formats, as described in \[[TM-HLS]\].

A large ecosystem has built up around carrying timed ID3 metadata in HLS for applications such as ad delivery & audience measurement. There are many benefits to adopting CMAF for HLS media delivery, but without a specification for carrying ID3 as sparse timed metadata in CMAF, deployment by companies in this ecosystem is blocked.

This specification describes how such ID3 metadata can be carried as timed metadata in a CMAF-compatible fragmented MP4 (fMP4) stream \[[CMAF]\] as used by the HLS protocol.

CMAF-compatible fragmented MP4 can also be used in DASH. The elements defined in this specification may also be used with DASH.

## Conformance
Conformance requirements are expressed with a combination of descriptive assertions and RFC 2119 terminology. The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in the normative parts of this document are to be interpreted as described in \[[RFC2119]\]. For readability, these words do not appear in all uppercase letters in this specification.

## Timed Metadata in a CMAF-compatible stream 

#### Overview
{:.no_toc }
Timed Metadata in a CMAF-compatible stream is signaled via one or more Event Message boxes ('emsg') \[[CMAF]\] per segment.  

Event messages with the scheme specified in this document will identify boxes that carry ID3v2 metadata \[[ID3]\].


#### ID3 Metadata in an Event Message Box
{:.no_toc }
      
##### Introduction
{:.no_toc }

One or more Event Message boxes ('emsg') \[[CMAF]\] can be included per segment. Version 1 of the Event Message box \[[DASH]\] must be used. 


##### Syntax
{:.no_toc }

For convenience, the follow box definition is reproduced from \[[DASH]\], section 5.10.3.3.3. 

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


##### Semantics
{:.no_toc }

`scheme_id_uri` MUST be set to `https://aomedia.org/emsg/ID3` to identify ID3v2 metadata \[[ID3]\].

`value` may either be an absolute or relative user-specified URI which defines the semantics of the id field. Any relative URI is considered to be relative to the `scheme_id_uri`.

`message_data` MUST contain complete ID3 version 2.4 data \[[ID3]\].


In general, ID3 don't carry a duration and in those cases the `event_duration` field should be set to 0xFFFFFFFF. If in a particular case, the ID3 message carries a duration, it should be reflected in the `event_duration` field.

The `presentation_time` must be within the time interval of the fragment.

The `id` field is not restricted in this version of the specification.



##### Signaling
{:.no_toc }

Files compliant to this specification should signal it using the brand `aid3` as part of the list compatible brands in the file type box. Manifest formats using files compliant to this specification may signal these files using the following URN: `urn:aomedia:cmaf:id3`.

## References

The following documents are cited in this specification.

#### Normative References
{:.no_toc }
  * **\[[CMAF]\]**  
    International Organization for Standardization, "Information technology -- Multimedia application format (MPEG-A)
    -- Part 19: Common media application format (CMAF) for segmented media",
    ISO/IEC 23000-19:2018(E), 2018, <http://www.iso.org/iso/catalogue_detail?csnumber=71975>.

  * **\[[DASH]\]**  
    International Organization for Standardization, "Information
    technology -- Dynamic adaptive streaming over HTTP (DASH)
    -- Part 1: Media presentation description and segment formats", ISO/IEC
    23009-1:2014(E): Draft third edition, 2018-07-26, <https://www.iso.org/standard/65274.html>.

  * **\[[RFC2119]\]**  
    Internet Engineering Task Force, "Key words for use in RFCs to Indicate Requirement Levels", S. Bradner, RFC2119, March 1997, <https://tools.ietf.org/html/rfc2119>.

#### Informative References
{:.no_toc }
  * **\[[HLS]\]**  
    Internet Engineering Task Force, "HTTP Live Streaming", R. Pantos, W. May, RFC8216, August 2017, <https://tools.ietf.org/html/rfc8216>

  * **\[[ID3]\]**  
    ID3.org, "The ID3 audio file data tagging format", <http://www.id3.org/Developer_Information>.

  * **\[[TM-HLS]\]**  
    Apple Inc., "Timed Metadata for HTTP Live Streaming", <https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/HTTP_Live_Streaming_Metadata_Spec/Introduction/Introduction.html>.

[HLS]: https://tools.ietf.org/html/rfc8216
[CMAF]: http://www.iso.org/iso/catalogue_detail?csnumber=71975
[ID3]: https://id3.org/Developer%20Information
[TM-HLS]: https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/HTTP_Live_Streaming_Metadata_Spec/Introduction/Introduction.html
[DASH]: https://www.iso.org/standard/65274.html
[GitHub Issues]: https://github.com/AOMediaCodec/id3-emsg/issues
[GitHub Version]: https://aomediacodec.github.io/id3-emsg/
[RFC2119]: https://tools.ietf.org/html/rfc2119
