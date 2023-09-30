# TEST LATENCY IN URL SHORTENER IN TEST ENVIRONMENT

September 29, 2023

By:  Annie V Lam - Kura Labs

# Background

Nike emailed us about the URL Shortener.  In Nike's email, Nike mentioned customers are complaining about the amount of time it takes to load thier webpages.  Nike will like us to reduce the amount of latency customers are experincing.  

## Troubleshooting

We have a third party application, Codon, that allows us to test for average latency.  In order not to crash the production web server, we created another web server in a test envirnoment that is in the same region, same VPC, but different availability zones (AZ).  As the test server is in the same region as the production server, just in different buildings, the difference in latency if there is any, should be immaterial.

A new hire was tasked with updating the URL shortener. The new hire committed version 2 of the application to the main branch. Which automatically triggered a build, test, and deploy to the production server, replacing version 1 of the application running on the server.  We used Codon to send a thousand GET requests within 2 seconds to see what the latency is.

## Result

The result is that our URL Shortener webser

## Post-Incident Report
The URL Shortener launched, but when testing the function, the URL Shortener was giving Internal Server Error with a 500 code:

![URL ERROR](Images/URL_Shortener_Error500.png)

Since the page was able to launch it is not an Web Server issue.  The issue was with web shortener logic, therefore the issue should be within the application.py file.
We pulled the log file for the Elastic Beansalk production environment and did an error search and the error was pertaining to an error code realated to the wrong json medthod was used:

![JSON METHOD ERROR](Images/EBS_Error_Log.png)

We looked at the logs to see when was Version 1 and Version 2 commited:

![Commmit Versions](Images/git_commit_v2.png)

We also compared the difference between Version 1 and Version 2 of the application:

![Commmit Diff](Images/git_diff.png)

As we the new hire's work was not reviewed before it was put into production, we decided that we will not keep the history of the new hire's works and did a git reset --hard:

![Commmit Diff](Images/git_reset_hard.png)

It worked:

![Commmit Diff](Images/URL_Created2.png)

## Conclusion

We reverted back to a previous known working commit and that resolved the issue.  There are multiple ways to resove this, but fastest way is to rollback to a known working histroy.  The other option is to just troubleshoot the application.py file itself.

Moving forward, we will have a more centralized controll on what gets pushed out to productions and have managers review the work of their staff.  

This incident was reported to Nike.

