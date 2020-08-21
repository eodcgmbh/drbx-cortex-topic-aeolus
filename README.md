# The DRBX Cortex Topic Definition for Aeolus

## Introduction.

The topic definition is a Java package that allows identifying Aeolus products by applying regular expressions-like file name matching rules. The central element of this package is the ontology definition file ([cortex-index.owl](src/main/resources/META-INF/cortex-index.owl)) located in the META-INF folder inside the resources directory.

The topic is currently designed to support the following product types from the Aeolus satellite mission:

- ALD\_U\_N\_1B
- ALD\_U\_N\_2B

For decoding the binary datasets, which is necessary for extracting the footprint coordinates, product-specific XML schema definitions are contained in the ([xsd](src/main/resources/META-INF/xsd)) folder inside the resources directory. The metadata information is extracted from the XML header files provided within the product package.

The file name matching patterns and the XSD files were created based on the official product specification documents:

- [Aeolus Level 1b Input/Output Data Definitions Interface Control Document](https://earth.esa.int/pi/esa?type=file&table=aotarget&cmd=image&alias=Aeolus_L1B_Input_Output_DD_ICD)
- [Aeolus Level 2b/2c Input/Output Data Definitions Interface Control Document](https://earth.esa.int/pi/esa?type=file&table=aotarget&cmd=image&alias=Aeolus_L2B_2C_Input_Output_DD_ICD)

Apache Maven is used for building the Java project. The Project Object Model XML file ([pom.xml](pom.xml)) specifies the overall package configuration.

## Product Format and Composition

The individual Aeolus Level-1B and Level-2B product datasets are provided as tarballs. The archives are always composed of two physical files:

- an ASCII-based XML header file having the extension .HDR
- a mixed ASCII/BINARY data file having the extension .DBL

Metadata is extracted from the XML header, footprints are created based on the records contained within the DBL file.

The following are examples illustrating the product structure for the ALD\_U\_N\_1B and ALD\_U\_N\_2B products, respectively:

- AE\_OPER\_ALD\_U\_N\_1B\_20200504T070059025\_005423999\_009838\_0001.TGZ
  - AE\_OPER\_ALD\_U\_N\_1B\_20200504T070059025\_005423999\_009838\_0001.HDR
  - AE\_OPER\_ALD\_U\_N\_1B\_20200504T070059025\_005423999\_009838\_0001.DBL
- AE\_OPER\_ALD\_U\_N\_2B\_20200503T051747\_20200503T064835\_0001.TGZ
  - AE\_OPER\_ALD\_U\_N\_2B\_20200503T051747\_20200503T064835\_0001.HDR
  - AE\_OPER\_ALD\_U\_N\_2B\_20200503T051747\_20200503T064835\_0001.DBL 

## Defined Classes for Aeolus

### Class &aeolus;product

The &aeolus;product class identifies the gzipped tarballs based on file name matching rules. It represents the base class for all Aeolus products. This class must match the class name referenced by the metadata extractor in the Aeolus addon (separate Java package). 

### Class &aeolus;productXmlHeader

The &aeolus;productXmlHeader class identifies the XML header. This is an auxiliary class not directly required for ingesting the product, but for enabling the exploration of the XML header when using the visual product viewer in DHuS (lower right panel).

### Class &aeolus;product1bDbl

By using the SDF (Structured Data File) implementation for the the DRB (Data Request Broker) it is possible to break down binary files into a tree of nodes on the basis of XML schema defintions. The &aeolus;product1bDbl class identifies the ALD\_U\_N\_1B DBL file and uses SDF to decode the binary file. The location of the XSD needs to be specified for this to work.  

### Class &aeolus;product2bDbl

Analogous to &aeolus;product1bDbl but for the ALD\_U\_N\_2B product.