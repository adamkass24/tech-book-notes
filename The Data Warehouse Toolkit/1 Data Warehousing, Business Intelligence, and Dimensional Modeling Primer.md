Always consider the needs of the business and work our way backwards through logical and physical designs. Then come decisions about tech and tools.

**Different Worlds of Data Capture and Data Analysis**
	- Operational data stores optimally should address one transaction at a time - Analytics rarely does. We are often given aggregates or summaries. Transactions are gold.

**Goals of Data Warehousing and Business Intelligence**
- Goals must be **business driven***
- Information must be easily accessable, intuitive, and obvious to the business user. Data Structures and labels should mimic business users' thought process and vocabulary.
- Information must be consistent and credible. Released only when ready for consumption. Labels are common across sources.
- Adaptable - existing process and tools should not break with new requests
- The data warehouse must lead to more improved decision making than other available solutions
- The business must **accept and engage with*** the data warehouse for it to be successful
- Understand the business users
	- What are their job responsibilities, goals, and objectives? Sometimes they don't know.
	- What decisions do they make? Any common threads?
	- Identify the 'best' users who make effective high impact decisions
	- find NEW users
- Deliver High Quality Relevant Information and Analytics
	- Prioritize actionable data
- Sustain the DW/BI Environment
	- Take a portion of the credit for business decisions made using the data warehouse
		- use these to justify staffing and ongoing expenditures
	- Invest in updating and maturing the DW system
	- Maintain users' trust
	- Keep business users, executive sponsors, and IT management happy - be a good partner
- ANTI GOALS
	- We do NOT when humanly possible break users things
	- publish false, incomplete, or not ready data

**Dimensional Modeling Introduction**

Normalizing **too deeply*** makes models hard to understand, and perform poorly in BI applications.
A dimensional model contains the same information, but packages the data in a format that delivers user understandability, query performance, and resilience to change.

- Fact Tables for Measurements
	- Colloquially, a fact is the interaction between at least two dimensions. Facts are the measurements that result.
	- Each row in a fact corresponds to a measurement event, and is at a specific level of detail, the grain. All measurements in a fact table must be at the same grain.
	- #wisdom BEDROCK PRINCIPLE - EVERY MEASUREMENT EVENT should have a 1-1 relationship to a single row in the corresponding fact table.
	- All fact tables fall into one of three categories:
		- transaction grain - most common
		- periodic snapshot
		- accumulating snapshot
	- All fact tbales have two or more foreign keys that connect to the dimension table's primary keys
		- when all keys in the fact correctly match to the pk in corresponding dim tables, they satisfy **referential integrity***
	- #wisdom ***Fact tables express many to many relationships. All others are dimension tables***
- Dimension Tables for Descriptive Context
	- Context - describe who/what/where/when/how/why of a business event / measurement
	- Continuous numeric values are almost always facts
- Facts and Dimensions Joined in a Star Schema

Kimball's DW/BI Architecture - by components
	Operational Source Systems
	Extract, Transformation, and Load System
	- Surrogate Key Assignment
	- Code Lookups for descriptions and friendly names
	- flatten denormalized dimensions
	Presentation Area to Support Business Intelligence
	- Looker/PowerBI/Etc
	- Atomic data is required to withstand assaults from unpredictable ad hoc user queries
	Business Intelligence Applications
	Restaurant Metaphor for the Kimball Architecture

Alternative DW/BI Architectures
- Independent Data Mart Architecture
	- typically domain or department driven, with **their*** requirements
	- most data not synthesized across depts. This is usually step 1.
- Hub-and-Spoke Corporate Information Factory Inmon Architecture
	- Data is normalized, but not necessarily integrated
	- integrated means resolving gaps between source systems and operational inconsistencies
- Hybrid Hub-and-Spoke and Kimball Architecture

Dimensional Modeling Myths
- Myth 1: Dimensional Models are Only for Summary Data
	- Absolutely not. You should ALWAYS give the most detailed/atomic data possible.
- Myth 2: Dimensional Models are Departmental, Not Enterprise
	- Wrong - departments don't matter. Business processes do. We operate without borders.
	- This is why our data domains do not have reporting structures associated. They are the best fit for processes, not department names.
- Myth 3: Dimensional Models are Not Scalable
- Myth 4: Dimensional Models are Only for Predictable Usage
- Myth 5: Dimensional Models Can't be Integrated

More Reasons to Think Dimensionally
- Focusing on the business process removes conversation from repots and focuses on business value
- Hurry to Chapter 4 to see how to get an Enterprise DATA WAREHOUSE BUS MATRIX
- First focus on major dimensions
	- Date
	- Customer
	- Product
	- Employee
	- Facility
	- Provider
	- Studen/Faculty
	- Account
	- and more!

Agile Considerations
- focus on driving business value
- value collaboration between dev team and the business stakeholders
- prefer face-to-face communications where possible
- adapt quickly to evolving requirements
- tackle dev in an incremental manner

Summary
