# Metadata
## NYPL-produced Metadata
NYPL will provide vendors with the following item-level metadata files in spreadsheets named as follows:
* (SOW ID)_shipmentInventory.xls
* (SOW ID)_metadataInventory.xls

## Vendor-produced Metadata

###### JSON Format and Specifications
NYPL AMI requires metadata to be created according to the NYPL JSON metadata schema. The schema and samples can be found at github.com/nypl/ami-metadata.
* Metadata must be packaged as a single JSON file for each media file.
* Metadata must validate against the digitized.json JSON Schema.
* Metadata files must be named with the same root as the media file to which they pertain, but must not include the extension of the media file. Examples:
  * Correct: [filename].json
  * Incorrect: [filename].mov.json

###### Content
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

###### Notes
Please refer to the following usage guide for the various "notes" fields included in the schema:
* **bibliographic.accessNotes**: NYPL added - no additional input required
* **bibliographic.contentNotes**: any additional content notes (“title program ends mid-tape, followed by another program”; "contains explicit content")
* **technical.signalNotes**: audiovisual signal characteristics noticed during capture ("tracking errors; audio buzz")
* **source.notes.physicalConditionDigitizationNotes**: description of pre-existing damage or decay not already noted in PreShip notes (“broken leader”; "bad splices")
* **source.notes.physicalConditionPreShipNotes**: NYPL added - no additional input required
* **digitizationProcess.notes.processNotes**: rehousing, adding leader, baking, cleaning

###### Metadata Errors
* If you find that NYPL has misidentified/miscataloged the technical characteristics or format of an asset and provided incorrect metadata (i.e. our metadata lists an asset as an Audio Cassette and its actually a PXL video recording), we'd like to receive corrected JSON, which will contradict the metadata spreadsheet we provided. Please make any changes to source object metadata elements that would be appropriate, and note your correction in the JSON files "digitization.notes.processNotes" in the following way:
  *"NYPL-provided metadata incorrectly listed source object format as [insert wrong format here]; JSON reflects correct source object format"

## Audio Configurations and Terminology Grid
Please refer to this PDF for reference on the expected soundfields, number of audio tracks, track configurations, and tape head types for various audio formats. This list is a work-in-progress and we welcome any additions and input.
**[Audio Configurations and Terminology Grid](https://drive.google.com/a/nypl.org/file/d/0BxjaNNRY523keGxseWROZWZDUVk/view?usp=sharing)**

## File Packaging and Storage
###### Hard drive requirements
* Hard drives must be formatted in exFAT in a Mac environment.
* Hard drives must be labeled with a unique ID.
* Hard drives must not contain deliverables for more than one Project / Statement of Work.
* Deliverables must be separated by media type and stored in directories "Audio" and "Video".
  * Audio and Video directories must be stored in the root directory of the hard drive, and must ONLY contain completed bags.
  * Examples: If a drive contains both audio and video bags, both directories must be present; if a drive only contains video bags, they must be within a "Video" directory.

###### BagIt requirements

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

###### Recommendations
* The bag should be created with a tool using BagIt Library 4.4 or higher.
* The bag may include tagmanifest files. Some bagging software produces these files automatically. It it is not required to produce or delete these files.

###### Sample Bag Structures

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


