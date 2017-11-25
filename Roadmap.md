### Roadmap

The source code (as well as the binaries) of the Dodoni.net project are separated into several assemblies, i.e. fundamental unit of code, thus dll's. Each assembly is related to a specific purpose. This modular architecture reduces dependencies within the framework. Moreover, it is easily extendable. 

_**This project does not (yet) have the goal to provide high sophisticated models. In the first step we focus on an easy to use, flexible, easily extendable infrastructure.**_  It is planned to establish the Dodoni.net project in several phases, starting from a generic Excel Add-In to mathematical functions, financial mathematical functions, supply finance market instruments etc.:

#### Planned for upcomming Releases
* bugfixing, in particular in {{XLBasicComponents}}: 
	* Excel 2007, Excel 2010 etc. applies a new check to Excelsheets which cause some problems with the Excel data validation that is used in the Dodoni.net library
	* the ExcelPoolInspector is slow and crashes often
* update document for Excel-AddIn
**In a later step:**
* infrastructure for market data, for example yield curve bootstrapping (~~single~~ and multi-curve approach) etc.
* pricing and valuation engine
* Heston model (semi-analytical and Monte-Carlo)
* simple Inflation/yield curve model (risk-neutral and real-world), perhaps a market model