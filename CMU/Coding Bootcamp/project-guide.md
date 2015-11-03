# Project Guide

python crawler，可以用库，但是需要更多自己实现的 feature

## Teammate:

hongyis,zhizhouy,dawang,yangqiup,yitingh


 A vision document with a list of planned features
 Functioning web crawler written entirely in Python. All code must be checked into the Gitlab repository on Argonne. 
		+ Iteration plan must include:
			+ The list of features each member of the group will attempt to implement
			+ Estimation of time to complete each feature
		+ Iteration review must include:
			+ 1-for-1 review of the iteration plan, enumerating what features each individual finished and what features were not finished. Include a one-sentence explanation of why unfinished features could not be completed.
			+ Burndown chart comparing actual time to complete each feature versus estimated time

 Takes as input keywords that must be present on a web page
+ Recursively crawls links on a web page
	+ User may specify breadth-first or depth-first crawling
+ Rests at least two seconds between each recursive step for pages within the same domain (no rest required for pages on other domains)
+ Can be terminated at any time by the user
+ Has no preset limit to recursion depth
+ Stores data in a database
+ Provides real-time status to the user about how many pages has been crawled, what is currently being crawled, what is planned, etc. (use some creativity here)
+ May be either command line or web-based application.
+ Define at least four additional features not listed above

 Follow the Agile development pattern
+ Work at least 60-90 minutes on the project each and every day to stay ahead

 [Python web crawling1](http://null-byte.wonderhowto.com/inspiration/basic-website-crawler-python-12-lines-code-0132785/)
 [Python web crawling2](http://www.netinstructions.com/how-to-make-a-web-crawler-in-under-50-lines-of-python-code/)
 [Agile](http://www.agilemanifesto.org)
 [This link has more process than necessary – take a lightweight version of this](http://www.allaboutagile.com/category/how-to-implement-scrum-in-10-easy-steps/)
