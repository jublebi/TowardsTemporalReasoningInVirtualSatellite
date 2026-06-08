# Towards Temporal Reasoning in Virtual Satellite
This repository contains data and instructions to try the temporal extension of virtual satellite. We provide two docker images and a docker-compose file to start the tool.

First, clone the repository and navigate to the it with the following commands:
```shell
git clone git@github.com:jublebi/TowardsTemporalReasoningInVirtualSatellite.git temporal_virsat
cd temporal_virsat
```

Next, the provided docker images can be loaded
```shell
docker load < virsat_temporal.tar.gz 
```

To start the tool, execute the following command
```shell
docker compose up -d
```

The tool is now available at http://localhost:8000 

The following two users are added by default to experiment collaboration between users. Both have the same access to the Demo repository
| Username | Password |
| :--- | ---: |
| admin | admin |
| user | user |

A video demonstrating our temporal extension of Virtual Satellite is available at: https://youtu.be/FOI76H2N828


## Tool description - A short tutorial
In this section, we briefly introduce Virtual Satellite and its functionalities to assist in testing our extension.

### Login Page
The first page visible is the login page. For testing our extension, it should be sufficient to use the users mentioned above. However, Virtual Satellite already offers a lot of functionality for creating and managing users and refining access rights for user roles.
![alt text](/images/Login_Page.png)

### Project Overview
Once logged in, one can see the project overview page. Virtual Satellite allows you to create multiple projects within the same instance, each with its own set of data types. For each project, different users can be added. However, we created a Demo Project that already contains all of the data types needed.
![alt text](/images/Project_Overview.png)

### Demo Project
In the Demo Project, one can see how Virtual Satellite is structured. The screen is divided into a navbar on the left and a component view on the right. The navbar contains information about the project's entities. To interact with entities, one can choose between the edit and analyze mode. By selecting an entity in the navbar the component view on the right side will update accordingly. In this demo project, the entities belong to a Demo satellite and its subsystems. Just explore the project a bit, selecting different components and switching between the edit and analyze modes.
![alt text](/images/Demo_Project_Overview.png)

For our temporal extension, the relevant entities are “Development Process” and “Temporal Scenario,” both of type Temporal Graph, that contain temporal information about the system in the form of nodes representing time points and edges representing inequalities between them.
![alt text](/images/Development_Process_Edit.png)

In edit mode, one can view the different nodes and edges using the generic editors. However, this is not a convenient way, which is why we created a visualization for the graph-based structure. To display the temporal graph, select the root entity (Development Process or Temporal Scenario) and go to the analyze mode. By default, no start node is selected, which is why the graph is not displayed nicely:
![alt text](/images/Development_Process_Analyze.png)
To fix this, just select the “Start” node in the start node dropdown menu. The graph will then be rendered like this:
![alt text](/images/Development_Process_Analyze_Start_Selected.png)

### About the Development Process

The temporal graph displayed contains temporal information about the system's development process, with a “Start” time point, a “Launch” time point, and several time points in between representing different milestones. We use Simple Temporal Networks with Uncertainty (STNU) as a temporal model, which is why we distinguish between contingent (light grey) and non-contingent (black) time points as well as between non-contingent links (solid) and contingent ones (dotted). For non-contingent nodes, we can control the exact execution time, while we have no control over the exact execution time of contingent time points. However, we know that the contingent time points have lower and upper bounds defined by the two contingent links. For instance, the time point “Camera Built” occurs between 4 and 7 time units after the “Start” time point; however, we have no control over exactly when.

### About the Temporal Scenario

Besides the Development Process, we provide another example, “Temporal Scenario,” which contains information about the future system's temporal behavior. Starting with the “Receive Request” time point and ending with the “Send Response” time point, this time interval contains information about how data is processed and the temporal relations among the individual steps.

### Interact with the temporal graph

To improve the user experience, we added the functionality to directly interact with the rendered graph. One can select nodes or edges (left-click), move them (drag), edit them (right-click + edit), add new nodes and edges (right-click on the background scene), or remove them (right-click + remove).
![alt text](/images/Edit_Node_Popup.png)

### Check Dynamic Controllability

To check the temporal correctness of the temporal graph, simply press the “Check DC” button. In case of a temporal conflict, the conflicting edges will be highlighted in red. To resolve a conflict, right-click a conflicting edge and select “resolve conflict”. For instance, when selecting the edge between “Start” and “Launch” to resolve the conflict, the weight will be changed from 11 to 12. When checking again whether the temporal graph is dc (by pressing the “Check DC” button), the temporal conflict is resolved.
![alt text](/images/Temporal_Conflict.png)
![alt text](/images/Temporal_Conflict_Resolved.png)

### Add another temporal graph

#### Modeling assumptions

We assume that the user is familiar with the notion of Simple Temporal Networks with Uncertainty (STNU) and how they represent temporal information. If not, we refer to the literature and point out some specifics for our implementation:
- Nodes are set correctly to contingent or non-contingent. A node is contingent if it is the target of a contingent edge. Otherwise, it is non-contingent
- For contingent edges, always two exist. The lower and upper values form the contingent interval, within which the timepoint can occur. You can take a look at the exemplary graphs and see that there are always two dotted edges from the same source to the same target. Use positive weights.
- Non-contingent edges represent inequalities between timepoints. If  A - v -> B is present, it indicates that B < A + v . v can be positive or negative. For non-contingent edges, it is also okay that only one edge exists between two nodes. For instance, between start and launch only one edge exists, representing the overall deadline. 

To add another temporal graph, press the “Add root element” button within the edit mode. Within the pop-up, specify the name and select “Temporal Graph” as the type.

![alt text](/images/Add_Root_Element.png)

Next, create the sub-entities “Nodes” and “Edges” and select the corresponding types:
![alt text](/images/Add_Nodes.png)
![alt text](/images/Add_edges.png)

For each node, add an entity as a sub-entity of the “Nodes” entity and add a NodeCategory to it.
![alt text](/images/Add_start_Node.png)
![alt text](/images/Add_NodeCategory.png)

For edges, no separate entities are needed, which is why we can directly add the EdgeCategories to the “Edges” entity and add the data.
![alt text](/images/Add_EdgeCategory.png)
![alt text](/images/Add_EdgeCategory2.png)

When going to the analyze mode, selecting the root entity of the temporal graph, the new graph is visualized:
![alt text](/images/New_Temporal_Graph_Visualized.png)

## Notes on maturity

While we have already implemented the necessary functionality and some convenience features, it is still a prototype, so further testing is needed. Also, the usability will be further improved based on feedback from Virtual Satellite users.
