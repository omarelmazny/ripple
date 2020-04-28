# How my script works.
My script is based on bash, to connect to ripple server issue server_info command and saves the sequence number of the latest validated ledger along with the current time into a file. Then shows up into a graph.

I assume JQ is available on the sever. JQ is an open source tool that is like sed for JSON data - you can use it to slice and filter and map and transform structured data with the same ease.

<b>variables</b>
graph: records the path of the file where to save the information gathered from the server_info output. 
server_info: records the output from server info command.
time: records the time stamp.
seq: records the sequence number of the validated ledger.

<b>explanation</b>

My script runs in a for loop of 75 times collects the information from server_info then sleeps for 3 seconds. This way it will gather the information every 3 seconds in a span around 5 minutes then it will show the graph. 

The script connects to the ripple server using curl and and issues the server_info command and saves in server_info for later use.
Then using jq collects the seq number and the timestamp from the json output and saves under seq and time variables respectively.
Then it saves the time and ledger sequence in the file.
Later when it’s done, it visualize the collected data into a graph using gnuplot. Gnuplot runs with the below settings
-	Using comma as separator.
-	The x axis as time.
-	Formats the time on the graph and the xdata the same way it’s formatted from the server_info output. 
-	Format the y axis as numbers

I decided the polling interval to be around 4 seconds, since ledgers gets validates between 3 to 6 seconds to validate the transactions, so I thought 4 is a good middle ground to track the progress and increments. The script takes 2 seconds to record the entries and sleeps another 2 seconds.

Result show continues linear increase in the validated ledger in such time. Which means ledger gets validated quickly and regularly to ensure authenticity of the XRP.

Ledger validation duration may vary to handle and validate double spend problems.

