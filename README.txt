README
I. File list
--------------------
A. CVE package
a.CveDownload:
	1.By specifying the url, it download the file.
	2.The downloaded file is a zipped file, so there is a function called unZip.
	3.This also return the file name of the downloaded files, because we want to scan images based on these files.
	Conclusion: This can work, but not good code.
b.Identifier:
	1.Rely on Parser, and CveModel.

c.Parser:
	1.Parse the CVE file, include CveId, application, score, summary.
	2.Using DOM to parse.
	3.Conclusin: This ignore the "AND", "OR", "negate". ParsedInTree is the correct one.
d.NewIdentifier
	1.Rely on ParsedInTree, and NewXmlModel.
	2.This is using the correct logic.
e.ParsedInTree
	1.Using the correct logic to parse CVE file.
	2.Using DOM to parse the cpe-lang:logical-test into tree, then use postorder to make it becoma a list.
	3.Conclusion: This work good.

B. Model package
a.ApplicationModel
	1.The format of application, including name and version
	2.Using this for application in CVE and image.
b.CveModel
	1.The old version of storing data from CVE file.
	2.INCORRECT logic to judge if the application is vulnerable, because ignoring the "AND", "OR".
c.ImageModel
	1.The format of image.
	2.NOTE: Might not need OS in this model, because I see OS as an application in NewXmlModel.
d.LayerModel
	1.The format of layer.
e.LogicTestTreeNode
	1.The tree structure of cpe-lang:logical-test in CVE file.
f.NewXmlModel
	1. Convert the application into true, false boolean to represent if it exist.
	2. Then get the result of this boolean list.
	3. CORRECT logic


C. Scanner package
a.FrontendServiceWrapper
	1. Using the input data talk to FronendService to get the layer of the image manifest.
	2. Parse the manifest to get the all the layer.
b.LayerRepository
	1. This is the testdata representing what applications are on each layer.
	2. This data should be generated from a dynamic scanner or application identifier in the future.
c.Scanner
	1. The entry point of this project.
	2. Mainly talk to Identifer and FrontendServiceWrapper.
	3. This is still using the incorrect version of identifer.

D. TestDta package
a.LayerAppData
	1. List the layer and application relation.

II. Test Case
--------------------
A. Cve Package
a. CveIdentifierTest
	1. Test when given vulnerable app and safe app.
b. CveParserTest
	1. Test by given a cve file.
c. NewIdentifierTest
	1. Test by given a cve file

B. Scanner Package
a. FrontendServiceWrapperTest

b. LayerRepositoryTest
c. ScannerTest

III. Design
--------------------
The wiki page below explained how does the scanner work.
https://w.amazon.com/index.php/Starport/Design/Features/ImageVunerabilityScanning/VulnerabilityScanner

IIII. Hard problem
--------------------
A. Get what applications are on each layer
B. Get the base image of the image on registry

Note.
The CVE part is done(I think so)
