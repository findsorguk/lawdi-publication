To compile run

	#pandoc index.md -o index.html
	#pandoc -s -S -c ../isaw-papers-7.css index.md -o index.html
	pandoc --bibliography=/Users/rromanello/Documents/library.bib -s -S -c ../isaw-papers-7.css index.md -o index.html
	
## Random notes
	
	
	<!-- screenshot of Hellespont how the extracted data can be used in context of a reading tool 
	![Secondary literature view of the GapVis-based reading interface developed for the Hellespont project.](hellespont_seclit.png)
	-->

	
	
<!-- 
my KB:
underlying ontological model -> HuCit
get data from CWKB and the Perseus Digital Library via it CTS-compliant interface
links to the Perseus catalog and CWKB
is stored as RDF and can be queried by using SPARQL
-->

		@prefix ecrm: <http://erlangen-crm.org/current/> .
		@prefix efrbroo: <http://erlangen-crm.org/efrbroo/> .
		@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

		<http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001> a efrbroo:F1_Work;
		    ecrm:P131_is_identified_by <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#cts_urn>;
		    efrbroo:P102_has_title <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001#title> .

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

		@prefix oac: <http://www.w3.org/ns/oa#> .
		@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

		<http://hellespont.org/annotations/jstor#16> a oac:Annotation;
		    rdfs:label "Thuc. 1. 101";
		    oac:hasBody <http://data.mr56k.info/urn:cts:greekLit:tlg0003.tlg001:1.101>;
		    oac:hasTarget <http://jstor.org/stable/10.2307/268729>;
		    oac:motivatedBy oac:linking .

	For this reason I have tried to create an ontological model of canonical citations in order to identify what are the intrinsic properties of such citations. The main conclusion my colleague Michele Pasin and I came to [ref] is that canonical citations are representations (or information objects) having both *form* and *content*: the form of a citation is its style, typically expressed by means of formatting, whereas the content is the abstract element, within a canonical structure of a text, that identifies a text portion.  

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