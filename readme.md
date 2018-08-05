# Transcription Description File Schema

This is a repository for the schema for Transcription Description Files,
maintained by the SCTA community [http://scta.info](http://scta.info)

The TDF-SCHEMA is used to associate Transcriptions (and Transcription version chains) with Text Editions at the structureItem level and associating transcriptions with Articles.

# Description

## List

A transcription file begins with `<List>` as the root element, which takes a list of `<manifestations>` elements to which transcriptions will be associated.

## Manifestation

Each `<manifestation>` takes the following attributes and elements.

- A `@manifestationDefault` **MUST** be present and the value **MUST** be `true` on one manifestation element which indicates which manifestation among a plurality of manifestations should be used as the default.
- The `<name>` element **MUST** be present and **MUST** be unique. If a Manifestation identifier already exists in a corresponding EDF file, this should be used. If a Manifestation is being created (this generally will only happen in the case of born-digital manifestation). The identifier will be guaranteed to be unique system wide if it is unique within the file.
- The `<title>` element **MUST** be present and should provide a human readable label. If the a the manifestation corresponds to the manifestation in an existing EDF this title will be overwritten by EDF information.
- If the `<list>` has an `@type=articles`, the `<Manifestation>` element **SHOULD** contain a least one `<isArticleOf>` element which associates the article with a given SCTA resource.
  - Note: in the case of text editions, such associations are not needed as they are managed by the EDF file.

## Transcriptions

`<Transcriptions>` is a wrapper element that **MUST** be present, even if there is only one transcription being listed, which will likely be the case with respect to Articles.

## Transcription

The `<Transcription>` element contains a version chain. There will often only be one main transcription here (again especially in the case of articles).

- A `@transcriptionDefault` **MUST** be present and the value **MUST** be `true` on one transcription element which indicates which transcription among a plurality of transcriptions should be used as the default. This is the case, even if only one `<transcription>` element is listed.
- A `<title>` element **MAY** be listed if there is a need for human readable label for this transcription.
- A `<type>` element **MAY** be listed if there is an enumerated list of different kinds of transcription types (e.g. critical, diplomatic, translation). These values are defined outside the scope of the TDF.

## Version

The `<Version>` element begins a version chain.

- A `@versionDefault` **MUST** be present and the value **MUST** be `true` on one version element which indicates which version among a plurality of version should be used as the default.
  - Note the `@versionDefault` should be distinguished from the `versionHead`. The versionHead corresponds to whichever element stands in the first position of the list of `<Version>` elements. The `versionHead` is considered the current version of the file, as such its content is dynamic. The version refers, not to a specific snapshot of the file, but to whatever the current content of the file is. As such, a reviewed version should never be listed as the `versionHead` because a reviewed version should point to a static file whose content does not change. If there are any reviewed versions, there should always be at least to `<Versions>`, the `versionHead` followed by the reviewed version. The `@versionDefault` is a property used to indicate which version should be treated as the default version. This can be either the `versionHead` or any earlier version.
- A `@reviewed` **MAY** be present, whose value, if the attribute is present, **MUST** be `true`. The `@reviewed` indicates that this specific version has been reviewed and alerts aggregator and clients that a review for this version that can be de-referenced at a review look-up service (for example: https://dll-review-registry.digitallatin.org).
- A `<hash>` element must be present. If the `<Version>` is the `versionHead` this hash must simply be a string-id that is unique from all other versions in the version chain. If it is not the `versionHead` the IPFS hash should be used.
  - Note: If this `versionHead` in the `defaultTranscription`, the id `transcription` should be used, as this will align this transcription with all other `defaultTranscription` versions, allowing aggregators to be a larger transcription file composed of multiple `structureItems.`  
- A `<versionNo>` element **MUST** be present which **MUST** contain an `@n` attribute with a machine readable version number and **MUST** contain a human readable version label as its text value.
- A `<url>` value **MUST** contain a url pointer to the version.
  - If version is the `versionHead` a relative path can be used. In the case of a text edition transcription, the relative path is relative to the directory containing the `transcriptions.xml` file. In the case of article transcriptions, the relative path is relative articles directory which is a sibling to the articles list file, e.g. `articles/<article-file-name>.xml`

# Examples

An example transcription file associating transcriptions of text editions with manifestations looks as follows:

An live example can be viewed here: https://github.com/scta-texts/graciliscommentary/blob/master/pg-b1q1/transcriptions.xml

```xml

<?xml version="1.0" encoding="UTF-8"?>
<list>
  <manifestation manifestationDefault="true">
    <name>critical</name>
    <title>Critical</title>
    <transcriptions>
      <transcription transcriptionDefault="true">
        <title>Critical</title>
        <type>critical</type>
        <version versionDefault="true">
          <hash>transcription</hash>
          <versionNo n="1.0.0-dev">Version 1.0.0-dev</versionNo>
          <url>pg-b1q1.xml</url>
        </version>
        <version>
          <hash>QmT6MXgMLCgXVehKQn76vc2hMAYS4yn5n1h48VRe81A3Rz</hash>
          <versionNo n="1.0.0-dev">Version 1.0.0-dev</versionNo>
          <url>https://gateway.scta.info/ipfs/QmT6MXgMLCgXVehKQn76vc2hMAYS4yn5n1h48VRe81A3Rz</url>
        </version>
      </transcription>
      <transcription>
        <title>Critical Alternative</title>
        <type>critical</type>
        <version versionDefault="true">
          <hash>transcription2</hash>
          <versionNo n="1.0.0-dev">Version 1.0.0-dev-t2</versionNo>
          <url>pg-b1q1.xml</url>
        </version>
        <version>
          <hash>QmT6MXgMLCgXVehKQn76vc2hMAYS4yn5n1h48VRe81A3Rz-t2</hash>
          <versionNo n="1.0.0-dev">Version 1.0.0-dev-tw</versionNo>
          <url>https://gateway.scta.info/ipfs/QmT6MXgMLCgXVehKQn76vc2hMAYS4yn5n1h48VRe81A3Rz</url>
        </version>
      </transcription>
    </transcriptions>
  </manifestation>
  <manifestation>
    <name>en</name>
    <title>English</title>
    <language>en</language>
    <transcriptions>
      <transcription transcriptionDefault="true">
        <type>translation</type>
        <version versionDefault="true">
          <hash>transcription</hash>
          <versionNo n="1.0.0-dev">Version 1.0.0-dev</versionNo>
          <url>en_pg-b1q1.xml</url>
        </version>
        <version>
          <hash>QmP4FPs1fhvTHDkbuRgEsYuKdSUXG1Dr2ApSCiDBYFW7K8</hash>
          <versionNo n="1.0.0-dev">Version 1.0.0-dev</versionNo>
          <url>https://gateway.scta.info/ipfs/QmP4FPs1fhvTHDkbuRgEsYuKdSUXG1Dr2ApSCiDBYFW7K8</url>
        </version>
      </transcription>
    </transcriptions>
  </manifestation>  
</list>

```

An example transcription file associating transcriptions with an Article looks as follows:
Note that the `@type=articles` indicates that this list is for "articles" which allows the use of a view extra properties,
most notably the `isArticleOf` element.

A live example can be viewed here https://github.com/scta/scta-articles/blob/master/plaoul-articles.xml

```xml

<list type="articles">
  <!-- Plaoul related articles -->
  <manifestation manifestationDefault="true">
    <name>pp-about</name>
    <title>About the Sentences Commentaries</title>
    <type>about</type>
    <isArticleOf>http://scta.info/resource/plaoulcommentary</isArticleOf>
    <isArticleOf>http://scta.info/resource/plaoulreportatio</isArticleOf>
    <isArticleOf>http://scta.info/resource/plaoulreportatioA</isArticleOf>
    <transcriptions>
      <transcription transcriptionDefault="true">
        <version versionDefault="true">
          <hash>head</hash>
          <versionNo n="1.0.0-dev">Version 1.0.0-dev</versionNo>
          <url>pp-about.xml</url>
        </version>
      </transcription>
    </transcriptions>
  </manifestation>
</list>

```
