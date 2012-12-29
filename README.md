
This algorithm is simulated by Disalgo which is based on python, and through defining a series of functions 
for sending and receiving message from one site to another.It takes N processes and R requests for each 
processes, and mutual exclusion is guaranteed while accessing the CS.

To implement this algorithm, first define two queues. One is waiting queue which collects the requests from other site. 
The other is voting queue which collects the votes send from other sites.
Assume the site i request to enter CS, and it sends out requests to all the other sites. Defined the following functions
to handle the whole process:

OnRequest:
Check if the site which received the request from site i had voted to other site before. If it did not, send Vote(self) 
message to site i. Otherwise, check if the request from site i comes earlier than the request from the voted site. If it
does, send Rescind(self) message to the voted site. Otherwise, put the request from site i onto the waiting queue.

OnVote:
When one site received vote from site j. First check if it has already received more than N/2 votes from other sites. If
it has, it send RemoveVote(self) message to site j. Otherwise, it would put the vote from site j onto its voting queue.

OnRemoveVote:
When one site received RemoveVote(self) message, it would pop out the earlier request from its waiting queue, and vote 
to the site which sent that request.

OnRescind:
When one site received Rescind message from site j. If it's not in CS, then first send RemoveVote(self) message to site
j, and remove the vote from site j from its own voting queue.

When the number of votes in the voting queue of site i is more than N/2, it would enter the CS.
When site i wants to exit the CS, it would send RemoveVote message to all the sites has voted to it. Then upon
receiving this message, those sites would release their vote to site i and send vote to other site.
