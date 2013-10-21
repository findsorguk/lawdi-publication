# Mining Citations, Linking Texts (early draft)

## [Matteo Romanello]()

Canonical citations are the standard way of citing primary sources (i.e. the ancient texts) in Classics: the ability to read them, that is knowing what the various abbreviations--very often ambiguous and at times even cryptic--that normally appear in such citations stand for is part of the early training of any classicist. However, having an expert system to capture automatically these citations and their meaning, is an issue that has been long known [ref], only recently studied/tackled [@romanello_singap; @Romanello2013] and not yet solved and is one of the aims of the project of which the research presented in this paper is part. Such a system has a great potential both for scholars in Classics and for the study of Classics as a discipline: capturing the citations of ancient texts that are contained in journal articles, commentaries, monographs and other secondary sources, allows us, for example, to track over time how and how much such texts were studied, an essential piece of information for a data-driven study of the discipline and its evolution. <!-- another example: intertextuality and reception of classics -->

<!-- but rephrase! -->Citations to ancient texts that are contained in journal articles or commentaries are already a way of creating links between objects. Previous studies have already tackled the technical problem of how such citations can be transformed into links and have immediately seen/recognised the potential benefit for the users [refs]. Therefore, extracting canonical citations from modern publications means essentially reconstruct or make explicit the links, already existing in some form, between the citing and the cited texts. At the same time, the arbitrary act of transforming a citation into a link between web resources can also misrepresent the linking function played by citations, and particularly by canonical citations.

<!--...add some stuff...-->

## Mining Citations: Extraction and Disambiguation

Before going into more details concerning the ontological modelling of canonical texts and citations, let us consider briefly how the extraction of such citations from texts is performed. Extracting citations means in fact performing two separate/distinct tasks: firstly, capturing the string(s) that constitutes/contain the citation and secondly determining the actual meaning of that citation or, in other words, to which specific section of which text is the citation referring to. In Natural Language Processing jargon these two steps are called respectively Named Entity Recognition (or extraction) and Named Entity Disambiguation. 

In fact, my approach to citation extraction (see Fig. 1, no. 1 and 2) is essentially based on state-of-the-art NER techniques with the only difference being what it takes to adapt these techniques to the new domain to which they are applied [ref]. Instead of considering only the usual NEs--such as names of people, places and organizations--I treat as NEs the different components of a citation in addition to any mention of ancient authors and works occurring in the context that surrounds the citation itself. For this purpose four distinct entities were identified: `aauthor`, `awork`, `refauwork` and `refscope`.<!-- perhaps explain what they are? --> In its current definition, a citation is a relation between any two entities, where one is always the indication of the citation's scope (i.e. `refscope`) and the other can be any of the other entities (i.e. `aauthor`, `awork` and `refauwork`). 

### Figure 1: TBD

<img src="extraction_steps.png" width="90%" />

	Once captured, citations need to be disambiguated: this is done by assigning to each citation its corresponding CTS URN [@hmt-doku-ctsurns]. CTS URNs are a kind of identifiers that follow the Uniform Resource Name standard that were developed in the framework of the Multitext Homer project as part of the CITE architecture and were designed to become the equivalent of canonical citations in a digital environment, in the sense that they allow one to "identify and retrieve digital representations of texts" [ref, @HMT-doku][^1]. What this means in practice is that the citation "Hell. 3.3.1-4" of the example showed in Fig. 1 (no. 3) is turned into its corresponding URN, namely `urn:cts:greekLit:tlg0032.tlg001:3.3.1-3.3.4`.

[^1]: To date one of the main adopters of this technology is the Perseus project that has built on top of it to provide several functionalities of its digital library and catalog [internal ref?].

## A Knowledge Base of Canonical Texts (Linked Authorities?)

NER systems of this kind typically require/rely on a surrogate of domain knowledge, such as a gazetteer or a knowledge base, to support both the extraction and disambiguation of NEs, and even more so in the case of open domain applications. To support the disambiguation of canonical citations such a knowledge base needs to contain, for example, all possible abbreviations of the name of an author or the title of a work, possibly in multiple languages to work on multi-lingual corpora.  Since the texts we are dealing with are canonical it is possible to store in this knowledge base, In addition to abbreviations, also detailed information about the citable structures of each text such as for example how many books are contained in Thucydides' *Histories*, how many chapters are contained in book 1 etc. 
Being able to query this sort of information allows one to validate the automatically extracted citations, thus making possible to identify, if not to recover, those citations that are just *impossible*. An example of this phenomenon is the string "Thuc. 5. 14. 1. 41.": although it looks as a plausible citation, it is not a valid one as the work here referred to--Thucydides' *Histories*--is made of three, not four, citable, hierarchical levels, (i.e. book/chapter/section). Such errors are very common when working with OCRed texts where the lack of structural markup causes, as in this case, the footnote number to be mistakenly interpreted as being part of the canonical citation "Thuc. 5. 14. 1". 

<!-- 
my KB:
underlying ontological model -> HuCit
get data from CWKB and the Perseus Digital Library via it CTS-compliant interface
links to the Perseus catalog and CWKB
is stored as RDF and can be queried by using SPARQL
-->

The content in the knowledge base is structured mostly using a combination of CIDOC-CRM and FRBRoo ontologies[^2]:  the Functional Requirements for Bibliographic Records (FRBR) model, in particular, is suitable for modelling information related to Classical (canonical) texts, as was showed by @babeu_named_2007 [ref], and has influenced substantially the design of the CTS protocol. In those few cases where these ontologies did not suffice to model the data we have extended some of the classes they provide in what we called the HUmanities CITation Ontology (HuCit)[^3].  

### Figure 2: Excerpt of the RDF/Turtle serialization of the record for Thucydides' *Histories*

	@prefix ecrm: <http://erlangen-crm.org/current/> .
	@prefix efrbroo: <http://erlangen-crm.org/efrbroo/> .
	@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

	<http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001> a efrbroo:F1_Work;
	    ecrm:P131_is_identified_by <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#cts_urn>;
	    efrbroo:P102_has_title <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#title>;
		owl:sameAs <http://catalog.perseus.org/catalog/urn:cts:greekLit:tlg0003.tlg001> .

	<http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#creation_event> a efrbroo:F27_Work_Conception;
	    efrbroo:R16_initiated <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001> .

	<http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#cts_urn> a ecrm:E42_Identifier;
	    rdfs:label "urn:cts:greekLit:tlg0003.tlg001";
	    ecrm:P2_has_type <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#type_CTS_URN> .

	<http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#title> a efrbroo:E35_Title;
		ecrm:P139_has_alternative_form <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#abbr1> .
	    rdfs:label "Der Peloponnesische Krieg"@ger,
	        "History of the Peloponnesian War"@eng,
	        "La Guerra del Peloponneso"@ita,
	        "l’Histoire de la guerre du Péloponnèse"@fre .	

	<http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#abbr1> a ecrm:E41_Appellation;
	    rdfs:label "Thuc.";
	    ecrm:P2_has_type <http://data.mr56k.info/type_abbreviation> .
	
As showed in Fig. 2 our record is linked to the one contained in the Perseus Catalog; the CTS URN associated to the work as well as the abbreviations of its title are explicitly modelled by using respectively the CIDOC-CRM classes `E42_Identifier` and  `E41_Appellation`.

[^2]: mention Erlangen implementation.

[^3]: The HuCit namespace is <http://purl.org/net/hucit>; the source code and some examples can be found in the code repository at <https://bitbucket.org/56k/hucit/>.


## Extracted Citations as Annotations

In addition to being important because of their function, canonical citations are also an interesting artifact in itself. They were designed, well before the advent of digital technologies, to refer to texts in a very precise and interoperable way: *precise* because texts are the fundamental object of philological research, therefore a scholarly discourse about texts needs a very accurate way of referring to them; *interoperable* because although texts may exist in different editions and translations, scholars need to be able to refer to specific sections of them without having to worry about the many possible variations in pagination or layout each single edition may present.

The act of transforming citations into links may lead to a misrepresentation of their nature and specifically of their intrinsic interoperability: a canonical citation should not be tight to the referred passage in a specific edition, but should rather work as a resolvable pointer, that can be resolved to a given portion of text in any (available) edition or translation. Therefore, when modelling a canonical citation found in a journal article by means of HuCit the citation is not linked directly to the text but points to an intermediate object called `hucit:TextElement` which in turn links to the digital representations of the cited passage. 

<!-- The main conclusion my colleague Michele Pasin and I came to [ref] is that canonical citations are representations (or information objects) having both *form* and *content*: the form of a citation is its style, typically expressed by means of formatting, whereas the content is the abstract element, within a canonical structure of a text, that identifies a text portion.  -->

### Figure 3: tbd

	@prefix ecrm: <http://erlangen-crm.org/current/> .
	@prefix hucit: <http://purl.org/net/hucit#> .
	@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

	<http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001:1.101> a hucit:TextElement;
	    rdfs:label "book 1, chapter 101 of Thucydides' Histories"@en,
	    ecrm:P1_is_identified_by [ a ecrm:E42_Identifier;
	            rdfs:label "urn:cts:greekLit:tlg0003.tlg001:1.101";
	            ecrm:P2_has_type <http://data.mr56k.info/CTS_URN> ];
	    hucit:element_part_of <http://data.mr56k.info/urn:cts:greekLit:tlg0012.tlg001_CanTextStruct>;
	    hucit:is_part_of <http://data.mr56k.info/urn:cts:greekLit:tlg0012.tlg001:1>;
		hucit:precedes <http://data.mr56k.info/urn:cts:greekLit:tlg0012.tlg001:1.100>;
		hucit:precedes <http://data.mr56k.info/urn:cts:greekLit:tlg0012.tlg001:1.102>;
	    hucit:resolves_to <http://data.perseus.org/citations/urn:cts:greekLit:tlg0012.tlg001.perseus-eng1:1.1>,
	        <http://data.perseus.org/citations/urn:cts:greekLit:tlg0012.tlg001.perseus-eng2:1.1>,
	        <http://data.perseus.org/citations/urn:cts:greekLit:tlg0012.tlg001.perseus-grc1:1.1> . 

### Figure 4: tbd

	<http://hellespont.org/annotations/jstor#16> a oac:Annotation;
	    rdfs:label "Thuc. 1. 101";
	    oac:hasBody <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001:1.101>;
	    oac:hasTarget <http://jstor.org/stable/10.2307/268729>;
	    oac:motivatedBy oac:linking .
	
### Figure 5: Secondary literature view of the GapVis-based reading interface developed for the Hellespont project.
	
![](hellespont_seclit.png)


## Works Cited

<!-- notes -->
