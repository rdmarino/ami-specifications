# Metadata
## NYPL-produced Metadata
NYPL will provide vendors with the following item-level metadata files in spreadsheets named as follows:
* (SOW ID)_shipmentInventory.xls
* (SOW ID)_metadataInventory.xls

## Vendor-produced Metadata

### JSON Format and Specifications
NYPL AMI requires metadata to be created according to the NYPL JSON metadata schema. The schema and samples can be found at **[ami-metadata](https://github.com/NYPL/ami-metadata)**.

* Metadata must be packaged as a single JSON file for each media file.
* Metadata must validate against the digitized.json JSON Schema.
* Metadata files must be named with the same root as the media file to which they pertain, but must not include the extension of the media file. Examples:
  * Correct: [filename].json
  * Incorrect: [filename].mov.json

### Content
The required contents of the metadata files are defined by the JSON metadata schema provided by NYPL, described in the previous section. According to that schema, vendors must produce metadata for each deliverable that includes the following categories:
* Bibliographic metadata about the physical media item (provided by NYPL).
* Source metadata about the technical characteristics of the physical media item.
* Technical metadata about the files created from digitization.
* Digitization process metadata about the equipment used to playback the physical media item.
* Metadata about the vendor and operator that digitized the physical media item. 
Metadata must use the only the fields and values defined in the NYPL Vendor Metadata Specification.
* Fields listed as "Required" for a given format category must be given a value.
  * If that information is not known, the value must be “unknown”.
* Fields listed as “Optional”, may contain a value. If there is no value, they must not be included in the metadata file
* Fields listed as “Not Applicable” to a given format or media type must not be included in the metadata file.
*If there is a question about a field, or if the provided vocabulary does not accurately describe an object or process, contact NYPL for guidance on how to proceed.

#### Notes
Please refer to the following usage guide for the various "notes" fields included in the schema:
* **bibliographic.accessNotes**: NYPL added - no additional input required
* **bibliographic.contentNotes**: any additional content notes (“title program ends mid-tape, followed by another program”; "contains explicit content")
* **technical.signalNotes**: audiovisual signal characteristics noticed during capture ("tracking errors; audio buzz")
* **source.notes.physicalConditionDigitizationNotes**: description of pre-existing damage or decay not already noted in PreShip notes (“broken leader”; "bad splices")
* **source.notes.physicalConditionPreShipNotes**: NYPL added - no additional input required
* **digitizationProcess.notes.processNotes**: rehousing, adding leader, baking, cleaning

#### Metadata Errors
* If you find that NYPL has misidentified/miscataloged the technical characteristics or format of an asset and provided incorrect metadata (i.e. our metadata lists an asset as an Audio Cassette and its actually a PXL video recording), we'd like to receive corrected JSON, which will contradict the metadata spreadsheet we provided. Please make any changes to source object metadata elements that would be appropriate, and note your correction in the JSON files "digitization.notes.processNotes" in the following way:
  *"NYPL-provided metadata incorrectly listed source object format as [insert wrong format here]; JSON reflects correct source object format"

## Audio Configurations and Terminology Grid
Please refer to this PDF for reference on the expected soundfields, number of audio tracks, track configurations, and tape head types for various audio formats. This list is a work-in-progress and we welcome any additions and input.
**[Audio Configurations and Terminology Grid](https://drive.google.com/a/nypl.org/file/d/0BxjaNNRY523keGxseWROZWZDUVk/view?usp=sharing)**

# File Packaging and Storage
## Hard drive requirements
* Hard drives must be formatted in exFAT in a Mac environment.
* Hard drives must be labeled with a unique ID.
* Hard drives must not contain deliverables for more than one Project / Statement of Work.
* Deliverables must be separated by media type and stored in directories "Audio" and "Video".
  * Audio and Video directories must be stored in the root directory of the hard drive, and must ONLY contain completed bags.
  * Examples: If a drive contains both audio and video bags, both directories must be present; if a drive only contains video bags, they must be within a "Video" directory.

## BagIt requirements

All files produced by digitization must be delivered in valid Bags. See the **[BagIt File Packaging Format V0.97](https://tools.ietf.org/html/draft-kunze-bagit-13)**

* The bag name must be the Primary ID of the physical object represented by the files in the bag.
* The bag must contain an md5 manifest that lists every file in the data directory and their md5 checksums.
* The bag must contain all of the media and metadata files from one, and only one, physical object.
* Video bags must contain sub-directories named PreservationMasters and ServiceCopies within the data directory.*
* Audio bags must contain sub-directories named  PreservationMasters and EditMasters within the data directory.*
* All media files in a PreservationMasters directory must be represented by files in the ServiceCopies or EditMasters directory.
* Each media file in the bag must have a corresponding metadata file.
* For video files with closed captioning, a sidecar file (filename.srt) must accompany the preservation master file inside the Preservation Masters folder.
* The bag must not include any system files such as .DS_Store and thumbs.db.
* The bag and the files within it must not be compressed with zip, tar, etc.

**Please note:** It is important that the sub-directories be named as they are written here, in plural (PreservationMasters / ServiceCopies / EditMasters) rather than singular form (PreservationMaster / ServiceCopy / EditMaster), regardless of the number of media files in the sub-directory.

### Recommendations
* The bag should be created with a tool using BagIt Library 4.4 or higher.
* The bag may include tagmanifest files. Some bagging software produces these files automatically. It it is not required to produce or delete these files.

### Sample Bag Structures

**Sample Audio Bag Structure**
- Audio
  * PrimaryID
    * data
      * PreservationMasters
        * division_PrimaryID_v01f01_pm.wav
        * division_PrimaryID_v01f01_pm.json
        * division_PrimaryID_v01f02_pm.wav
        * division_PrimaryID_v01f02_pm.json
      * EditMasters
        * division_PrimaryID_v01f01_em.wav
        * division_PrimaryID_v01f01_em.json
        * division_PrimaryID_v01f02_em.wav
        * division_PrimaryID_v01f02_em.json
    * bag-info.txt
    * bagit.txt
    * manifest-md5.txt
    * tagmanifest-md5.txt

**Sample Video Bag Structure**
- Video
  * PrimaryID
    * data
      * PreservationMasters
        * division_PrimaryID_v01_pm.mov
        * division_PrimaryID_v01_pm.json
      * ServiceCopies
        * division_PrimaryID_v01_sc.mp4
        * division_PrimaryID_v01_sc.json
    * bag-info.txt
    * bagit.txt
    * manifest-md5.txt
    * tagmanifest-md5.txt [optional]

**Sample Video Bag Structure (with closed captions sidecar file)**
- Video
  * PrimaryID
    * data
      * PreservationMasters
        * division_PrimaryID_v01_pm.mov
        * division_PrimaryID_v01_pm.json
        * division_PrimaryID_v01_pm.srt

      * ServiceCopies
        * division_PrimaryID_v01_sc.mp4
        * division_PrimaryID_v01_sc.json
    * bag-info.txt
    * bagit.txt
    * manifest-md5.txt
    * tagmanifest-md5.txt [optional]

# File Naming and Structure
## File Names
NYPL will provide the vendor with a filename root for each original item, consisting of a three-letter prefix, the primary ID, a volume number (and a face number for audio items), and a two-letter suffix indicating the role of the file.  These sections are each separated by an underscore. For example:

**(prefix)_(primary ID)_(item structure)_(file role).(ext)**
* Video: myd_mgzic1234_v01_pm
* Audio: myh_lt10a2345_v01f01_pm

The vendor is responsible for creating file names in order to represent additional face, region, stream, or part elements and file roles.

### Item Structure
A file name must use the following components to represent the portion of a physical media item represented by the file:

|Component | Formats | Note|
| --- | ----| ----|
| Volume: v## | <ul><li>A Volume is one video or audio object in a set of objects, when that set has been assigned a single primary identifier (i.e. NYPL classmark)</li></ul> | Audio and video|
| Face: f## | <ul><li>A Face is a stream or track, or a group of streams or tracks which play synchronously, within an audio object. Every audio object has at least one Face.</li></ul> | Audio only|
| Region: r## | <ul><li>A Region is a subdivision of a Face.</li><li>Regions are most often defined by a required change in playback characteristics (speed, EQ, track configuration, etc) of an object's Face.</li><li>This is rarely used for video objects.</li></ul> | Audio and video|
| Stream: s## | <ul><li>The Stream element is used to describe multi-track and multi-channel audio objects only.</li><li>Streams are one-channel or interleaved two-channel audio streams which comprise a multi-channel or multi-track audio object.</li></ul> | Audio only|
| Part: p## | <ul><li>The part element is used when digitization of a single face of an audio or video object requires interruption because the size of the resulting file would exceed technical limits if captured all at once.</li><li>The part element may also be used when a single tape contains sections of content that are each given different unique identifiers (each section would be a distinct Part).</li></ul> | Audio and video|
| Take: t## | <ul><li>A Take is an alternate preservation capture of an entire object face, produced with alternate playback characteristics in order to compare quality or results (stylus, EQ, speed, etc.). Rare occurrence.</li></ul> | Audio only|

#### Item structure example
* The file representing the second take of the second part of the third stream of the second region of the second face of an item marked as volume 1 is named as **_myd_259382_v01f02r02s03p02t02_pm.wav_**.

### File Role
A file name must record its intended use using one of the following codes.
* pm: Preservation Master, created for every physical media item
* em: Edit Master, created for audio files
* sc: Service Copy, created for video files

# Original Media
The following media formats have been organized into groups for reference in the Library's vendor contracts.

**Note:** The formats on these lists may be described differently in the NYPL metadata schema. As vendor contracts are updated, we may adjust the lists to align with our metadata schema.

## Audio
### Audio Group 1

|Type | Formats |
| --- | ----|
| Audio Reels Analog | <ul><li>half-inch open-reel audio</li><li>one-inch open-reel audio</li></ul><ul><li>quarter-inch open-reel audio</li><li>two-inch open-reel audio</li></ul>
| Audio Cassettes Analog | <ul><li>8-track</li><li>Compact Cassette</li></ul><ul><li>Microcassette</li><li>Minicassette</li></ul><ul><li>Fidelipac Cartridge</li><li>RCA Tape Cartridge</li></ul><ul><li>Elcaset (L-Cassette)</li>|
| Audio Wire | <ul><li>Magnetic Wire (Webster Chicago)</li>|

### Audio Group 2

|Type | Formats |
| --- | ----|
| Audio Cassettes Digital | <ul><li>ADAT</li><li>DAT</li></ul><ul><li>DA-88</li><li>Digital Compact Cassette</li></ul><ul><li>Betamax (PCM)</li><li>U-Matic (PCM)</li></ul><ul>|

### Audio Group 3

|Type | Formats |
| --- | ----|
| Audio Optical Disc | <ul><li>Audio CD-DA</li><li>Audio CD-R</li></ul><ul><li>Audio CD, pressed</li><li>Minidisc</li></ul><ul>

## Video
### Video Group 1
|Type | Formats |
| --- | ----|
| Video Cassette Analog | <ul><li>Betacam</li><li>Betacam SP</li><li>Betamax</li><li>Hi8</li><li>MII</li><li>S-VHS</li><li>S-VHS-C</li><li>U-matic</li><li>U-matic SP</li><li>VHS</li><li>VHS-C</li><li>Video8</li></ul>
| Video Cassette Digital | <ul><li>D-1</li><li>D-2</li><li>D-3</li><li>D-5</li><li>Digital Betacam</li><li>HDCAM</li><li>HDCAM SR</li><li>Digital8</li><li>Betacam SX</li><li>MPEG IMX</li></ul>|
| Video Reel Analog | <ul><li>2” quad open-reel</li><li>1” open-reel, Type A</li><li>1” open-reel, Type C</li><li>½” open-reel, Sony CV</li><li>½” open-reel, EIAJ Standard</li><li>½” open-reel, other/non-standard</li></ul>

### Video Group 2
|Type | Formats |
| --- | ----|
| Video Cassette DV | <ul><li>DVCam</li><li>MiniDV</li><li>HDV</li><li>DVCPRO</li><li>DVCPRO 50</li><li>DVCPRO HD</li></ul>|

### Video Group 3
|Type | Formats |
| --- | ----|
| Video Optical Disc | <ul><li>DVD/DVD-R</li></ul>|

# Digital Asset Technical Specifications
## Audio
For each original audio recording, the vendor shall produce:
- one (or more) preservation master file(s)
- one (or more) edit master file(s)

The deliverable audio files will be produced according to the following descriptions and specifications:

### Preservation Master File Specifications: Audio
#### All Audio Groups
##### General Guidelines
* The preservation master (PM) is the highest level derivative of the original recording, and is created in an effort to produce an authentic sonic facsimile of the original recording.  Therefore, the preservation master recording will represent an unedited, unaltered, direct, and complete capture of the signal from the output of the playback device.
* The production of preservation master files will comply with the technical recommendations, practices and strategies outlined in the International Association of Audiovisual Archives’, Guidelines on the Production and Preservation of Digital Audio Objects, IASA-TC 04, 2nd edition.
  * **Recommended citation:** IASA Technical Committee, Guidelines on the Production and Preservation of Digital Audio Objects, ed. by Kevin Bradley. Second edition 2009. (= Standards, Recommended Practices and Strategies, IASA-TC 04). www.iasa-web.org/tc04/audio-preservation
* The production of preservation master files will comply with the philosophical recommendations, practices and strategies outlined in the International Association of Audiovisual Archives’, The Safeguarding of the Audio Heritage: Ethics, Principles and Preservation Strategy, IASA-TC 03, version 3.
  * **Recommended citation:** IASA Technical Committee, The safeguarding of the Audio Heritage: Ethics, Principles and Preservation Strategy, ed. by Dietrich Schüller. Version 3, 2005 (= Standards, Recommended Practices and Strategies, IASA-TC 03). International Association of Sound and Audiovisual Archives. www.iasa-web.org/tc03/ethics-principles-preservation-strategy

##### Specific NYPL Requirements
* In general, one preservation master file will be generated for each physically or technically discrete recording area of the original object. For example, each side of an audio cassette will be recorded as a separate preservation master file, identified with a designated Face number.
* Considerations and circumstances that impact the number of preservation masters generated for each physical object, and/or for each discreet recording area:
  * For an object with two regions recorded at different speeds, a separate PM must be created for each Region (filename_v01f01r01_pm).
  * When a recording must be divided into parts due to file size limitations (4GB limit), a separate PM must be created for each Part.
  * There may be rare circumstances in which just the EM from a PM exceeds the 4GB limit (i.e. when mixing mono to stereo). In this case, the PM must be split prior to transcoding to ensure consistency of the 1PM:1EM requirement.
* If multiple PMs are created for a single recording due to either speed changes or size limitations (Regions or Parts), the cut should be made at a logical break in the content (if at all possible), and there must be exactly 5 seconds of overlap between the tail of the first PM and the head of the following PM, OR, if applicable, 5 seconds of audible/distinguishable content (in the event that the tail of r01 is silent or indistinguishable from the head of r02 in some other way) so that the parts may be combined in the future if necessary.
* Optimal signal extraction for the production of Preservation Master files should aim to capture the complete dynamic and frequency ranges of the original recording.
* An optimal signal extraction from the original recording will be a flat (unmodified) transfer, free of signal processing, equalization, level adjustment, noise reduction, etc.  Exceptions would include:
  * De-emphasis of a recording’s stated pre-emphasis (playback equalization)
  * Decoding  of a recording’s stated noise-reduction encoding
  * Signal extraction from analog original audio recordings will comply with the technical recommendations, practices and strategies outlined in the International Association of Audiovisual Archives’, Guidelines on the Production and Preservation of Digital Audio Objects, IASA-TC 04, 2nd edition, Chapter 5 (Signal Extraction from Original Carriers).
  * Optimal signal extraction from original sources includes the extraction of the intended signal, along with any unintended signal (such as artifacts and anomalies in the signal associated with the inherent limitations of historic recording technologies).
  * Preservation master files may be one or two-channel (interleaved), and the configuration employed will be determined by the needs of the original recording.
  * Levels may be adjusted ONLY if there is severe distortion or digital clipping from the source, and this adjustment must be noted clearly in the JSON metadata.

#### Audio Group 1: Analog Audio
The specifications for the preservation master files produced for analog sources are as follows:
* Format: Broadcast Wave (BWF)
* Audio data encoding: PCM
* Bit depth:  24-bit
* Sampling rate: 96,000 Hz
* Number of audio channels: Same as source

* Optimal signal extraction from analog sources seeks to be complete, and includes the transfer of the “lead-in” and “play-out” portions of a recording.
* Analog signals will be converted to a digital bitstream by means of an Analog-to-Digital converter which complies with the specifications in FADGI’s Audio Analog-to-Digital Converter Performance Specification and Test Method.
* No signal processing will be applied to the Analog-to-Digital converter’s digital bitstream; this would include equalization, level adjustment, dither, noise reduction, etc.

#### Audio Group 2: Digital Audio
* Preservation master files for DATs should be made at the bit depth and sampling rate of the original object.  For example, a DAT recorded at 48/16 should be captured as a 48/16 Broadcast WAV file.

#### Audio Group 3: Optical Audio
* CDs should be captured as a single Broadcast WAV file that matches the bit depth and sampling rate of the original object.
* In addition, the vendor will create a corresponding AES31-3 ADL for each CD.

### Edit Master File Specifications: Audio
#### All Audio Groups
The Edit Master file specifications are as follows:
* Format: Broadcast Wave (BWF)
* Audio data encoding: PCM
* Bit depth:  equal to the Preservation Master
* Sampling rate: equal to the Preservation Master
* Number of audio channels: 2 (dual-mono or stereo)

* The Edit Master serves as a production master – the recording from which all Service Copies for the Preservation Master will be derived.  It is a version of the Preservation Master which has been edited and/or processed to enhance the continuity and intelligibility of its content.
* One Edit Master file will be created for each Preservation Master file with two or fewer streams.
  * For multi-track recordings (objects with more than 2 streams), Edit Masters should not be created (and the JSON field source.subObject.streamNumber must be included in metadata records).
  * Technicians should be aware of the possibility that an Edit Master may in some instances exceed the WAV file size limit (4GB), requiring it to be split into parts. They must ensure that there is always 1 PM for every EM. See **Specific NYPL Requirements** for Preservation Master Files of All Audio Groups, above for details.

##### Edit Master Alterations
The Edit Master will be altered from the Preservation Master in the following ways, as necessary:

* Head and tail edits; to eliminate unrecorded portions of the Archive Original object which may have been captured in the production of the Preservation Master
*Elimination of the 5-second overlap included on any Preservation Masters that have been split out into multiple files for separate regions / streams / etc.
* Level Adjustment, when balance and/or overall level are insufficient (peak level adjustment max. -2db as necessary).
* Channel adjustment: ensuring that "mono" is true mono, conversion of one-channel audio to two-channel audio (mono to dual-mono)

## Video
For each original video recording, the following shall be produced:
* one Preservation Master file
* one service copy file
* Characteristics intrinsic to the broadcast standard of the source material, including frame rate, pixel aspect ratio, interlacing, resolution, and recording standard (NTSC, PAL, SECAM) should be preserved.
* Luma, chroma and black levels may be adjusted as required on playback equipment or time base corrector to best represent source material.
* The transfer should capture all content recorded on the original item, including any bars and tone, slates, or other material coming before the start of the recorded program.
For videos that contain closed captioning, a sidecar .SRT closed captioning file must be created and accompany the Preservation Master file, and closed captioning must be embedded in the Service Copy (refer to File Packaging / Bagit Requirements above).
* The recording should run until the end of the recorded content (picture and sound).  If this endpoint cannot be unambiguously determined, the recording should run until the end of the original object.

### Preservation Master File Specifications: Video
#### Video Group 1
* Preferred video codec: 10-bit uncompressed YUV (v210)
* Data compression: none (1:1, uncompressed)
* Chroma sub-sampling: 4:2:2 YUV
* Bit depth: 10-bit
* File wrapper: QuickTime
* Audio format: PCM
* Audio bit depth: 24-bit
* Audio sampling rate: 48 kHz
* Audio channels: Same as source
* Broadcast standard: (Same as original media)

#### Video Group 2
* Preservation master files for digital video original materials should maintain the codec and other characteristics of the original materials.  For example, DV/MiniDV objects should be captured with the DV codec; DVCPRO objects with the DVCPRO codec, etc.  These files should be wrapped as QuickTime (.mov).

#### Video Group 3
* DVDs should be captured as ISO 9660 disc image files to capture all data contained on the disc.

### Service Copy File Specifications: Video
#### All Video Groups
* Codec: H.264/MPEG-4 AVC (ISO/IEC 14496-10 - MPEG4 Part 10, Advanced Video Coding)
* Video bit rate: 3.5 Mbit/second
* File wrapper: MP4
* Frame rate: same as Preservation Master
* Frame size: same as Preservation Master
* Pixel Aspect Ratio: Square
* Audio Bit Rate: 192 kbps
* Audio Sampling Rate: 48 kHz
* Audio Codec: AAC
* Audio channels: 2
* Broadcast standard: (same as original media)
