Social Network Graph link prediction

Dataset Link - https://drive.google.com/drive/folders/12KiSqmDsvVh5kN3Li5R7qRK2xLydrn3w?usp=sharing

1. Problem Statement: 
	- Given a directed social graph, we have to predict missing links to recommend friends/connections/followers (Link prediction in graph)

2. Data Overview: 
	- Two columns- source_node and destination_node
	- Each row represents an directed edge from source_node to destination_node
	- Number of vertices: 18,62,220
	- Number of edges/datapoints: 9437520

3. Mapping to ML supervised Learning problem:
	- An important task is to create new features form the two columns from given pair of vertices(u_i, u_j):
		1. Is there an edge from u_j to u_i
		2. How many common vertices are being followed by u_i and u_j

4. Business objective and constraints:
	- No low-latency requirement
	- Predicting the probability of a link is useful because we want to recommend the links with highest probability to the user
	- We want to suggest the most correct links and we don't want to miss out any useful links -- high precision and recall

5. Performance metric and supervised learning:
	- precision, recall, f1-score, confusion matrix

6. libraries:
	- pandas, numpy, matplotlib, networkx

7. Exploratory Data Analysis:
	- Number of unique persons- 1862220
	- number of followers (indegree)
		-  90 percent of people have less than 12 followers
		-  99 percent of people have less than 40 followers
		-  99.9 percent of people have less than 112 followers	
		-  100 percent of people have less than 552 followers
	- number of followings (outdegree)
		-  90 percent of people have less than 12 followings
		-  99 percent of people have less than 40 followers
		-  99.9 percent of people have less than 123 followers	
		-  100 percent of people have less than 1560 followers
	- most people have very few number of followers and followings

8. Posing the problem as classification task
	- Generating some edges which are not present in graph for supervised learning problem
		- We are generating bad links (links which are actually not present and have shortest path length > 2) and marking them as class 0. We are creating 9437520 such links. 
		- So, now we have total 9437520 (class 1) + 9437520 (class 0) = 18875040 links 

9. Train Test Splitting:
	- As the data we have is temporal and it is changing over time but it is the snapshot of a data at a particular time. 
	- We do not have the time column in our data and hence we cannot do the time based splitting
	- So, we have to do random splitting and we cannot use the benefits of time based splitting
	- Some stats:
		- Number of nodes in train data: 1780722
		- Number of nodes in test data: 1144623
		- Number of people common to train and test: 1063125
		- Number of people present in train data but not present in test data: 717595 
		- Number of people present in test data but not present in train data: 82498
		- Percentage of people not there in train but exist in Test data: 7.12%
		- So, for these 7.12% we do not have any information in the train data. This is the cold start problem.

10. Feature engineering on graphs:
	- similarity measures:
		- jaccard distance of followers
		- jaccard distance of followees
		- cosine similarity of followers
		- cosine similarity of followees
	- page rank
	- shortest path
	- weakly connected components
	- Adamic/Adar index- usually used for undirected graphs but we are using for directed graphs (followers and followees)
	- Does the person follows back? -- if we want to predict link(a,b), we want to know if link(b, a) already exists or not
	- Kartz centralilty: The Kartz centrality for a node depends upon the influence of neighbouring nodes
	- HITS Score for each node -- hit score and authority score
	- Using SVD
	- Weight feature:
		- weight of incoming edges
		- weight of outgoing edges
		- weight of incoming edges + weight of outgoing edges
		- weight of incoming edges * weight of outgoing edges
		- 2 * weight of incoming edges + weight of outgoing edges
		- weight of incoming edges + 2 * weight of outgoing edges



