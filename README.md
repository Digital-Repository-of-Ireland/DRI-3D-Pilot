## DRI 3D Pilot

The DRI 3D Pilot is a project headed by [Trinity College Dublin](https://www.tcd.ie/) to investigate ways to support Cultural Heritage Institutions in creating, publishing and aggregating 3D content to Europeana Collections in collaboration with the [Digital Repository of Ireland](https://www.dri.ie/) (DRI), the Europeana National Aggregator for Ireland. The project is part-funded by the European Union within the Europeana Common Culture project.

### Introduction

The purpose of this project is to research and develop workflows and tools for the preparation, ingest and aggregation of 3D Content.

### Review of 3D Content in Europeana

The 3D pilot set out to investigate methods of preparing and aggregating 3D content to Europeana. This work began with a [Review of 3D material in Europeana](https://github.com/Digital-Repository-of-Ireland/DRI-3D-Pilot/blob/master/Report%20on%203D%20Content%20in%20Europeana.pdf).

At the time of conducting this review, there were 28,573 records in Europeana Collections tagged as 3D. A large portion of these, however, had broken links and have subsequently been removed from Europeana Collections search results, or were simply mistagged as 3D content. Of the remainder, the most popular format was 3DPDF. This format, although formerly popular, is now poorly supported on most platforms.

The next most common content consisted of a variety of embedded 3D viewers. Of these, the majority used the 3Dsom viewer, but all of these used the older flash version of this viewer. More recently aggregated content tended to use the Sketchfab viewer, with a smaller number using various proprietary viewers, or offering only panoramic photographs rather than true 3D models.

Only a small proportion of these provided the option to download the model for reuse, even where the license allowed reuse.

### 3D Content in Europeana Task Force

The project collaborated closely with the [3D Content in Europeana Task Force](https://pro.europeana.eu/project/3d-content-in-europeana). The initial review formed an input to the Task force's [Final Report](https://pro.europeana.eu/files/Europeana_Professional/Europeana_Network/Europeana_Network_Task_Forces/Final_reports/3D-TF-final%20report.pdf). Other input was provided to the Task Force activities, and the project undertook to implement the recommendations of the Task Force.

### Preprocessing workflows for 3D content

The project investigated the types of 3D content held by Irish Cultural Heritage Institutions. It was found that these were a mixture of large point clouds, meshes and models in a variety of file formats. To allow the pilot application to have the most impact we first explored workflows and tools that could be used to process these various file types into formats suitable for aggregation to Europeana.

Larger point cloud files cannot be displayed as they are in Europeana or other web-based platforms, but must first have a lightweight mesh generated from the point cloud. We set out to compare a variety of tools to achieve this. The tools included open source tools which could semi-automate the mesh reconstruction process such as CloudCompare and Meshlab, proprietary software such as RealityCapture, as well as open source libraries such as open3D which could be used to develop custom mesh reconstruction code.

<p align="center">
<img src="https://raw.githubusercontent.com/Digital-Repository-of-Ireland/DRI-3D-Pilot/master/model-2.png" width="300" height="300" /> <img  src="https://raw.githubusercontent.com/Digital-Repository-of-Ireland/DRI-3D-Pilot/master/my-mesh.png" width="300" height="300" />
</p>
  <em>An example of Surface reconstruction of Beheenagh Bridge Kerry with (a)CloudCompare and (b)Open3D (origal pointcloud source Transport Infrastructure Ireland) 
</em>


Code snippet for surface reconstruction with Open3D using Poisson surface reconstruction algorithm is as follows:

```python
 pcd = o3d.io.read_point_cloud("path/to/pointcloud") 
 downpcd = pcd.voxel_down_sample(voxel_size=0.03) 
 downpcd.estimate_normals(search_param=o3d.geometry.KDTreeSearchParamHybrid(radius=0.271250,max_nn=30))
 print('run Poisson surface reconstruction')
 mesh, densities = o3d.geometry.TriangleMesh.create_from_point_cloud_poisson(downpcd, depth=10, width=0, scale=1.1, linear_fit=True) 
 o3d.visualization.draw_geometries([mesh]) 
 o3d.io.write_triangle_mesh("yourmesh.ply",mesh)

```


Developing custom code gave the benefit of allowing integration with the existing platform and a simpler more automated workflow for aggregation to Europeana.  The quality of the resultant meshes was lower than that obtained by the semi-automated or manual process using other tools, however, and did not produce models that were of sufficient quality for aggregation. We are in the process of writing up these results as recommendations to content providers.

Where the content came in the format of meshes or models this pre-processing step was generally not necessary, although the quality varied by content provider. More detail on the workflows involved were published in [Europeana Tech Insight issue 14](https://pro.europeana.eu/page/issue-14-3d).

### Viewers for 3D Content

Based on the recommendations of the 3D Task Force we looked into various options for how to aggregate these models to Europeana as EDM. Many of the Europeana content providers currently aggregating 3D content choose to host their models on third-party platforms. While this is suitable for smaller organisations without the capacity to host large volumes of data, it builds in reliance on a service which is out of control of the Content Providers and Aggregators, and there can be issues with the cost of such services as well as the licencing and export or access methods.

As Sketchfab is a very popular option within the Europeana community, we investigated this, but the fees involved make this a less attractive option. For this pilot we eventually opted for a self-hosted 3D viewer as part of the National Aggregator platform. We looked into three possible solutions

- 3DHop
- Smithsonian Voyager
- Three Javascript Library

Of these 3DHop and Smithsonian Voyager only had limited support for input file formats. This was a problem for the pilot as the content was in such a wide variety of formats. It was decided to use the Three JavaScript Library to display these models online. This is now being integrated with the DRI Repository platform and the Europeana aggregation feed.

Code snippet for visualising , loading the 3D model and setting the controls on screen (zoom in zoom out etc) with the 3DHop is as follows:

```javascript
function setup3dhop() { 
	presenter = new Presenter("draw-canvas"); 
	presenter.setScene({
		 meshes: { "Cage" : { url: "path/to/model" } },
		 modelInstances : { "Model" : { mesh : "Cage" } } 
	   }); 
   }

   function actionsToolbar(action) { 
	if(action=='home') presenter.resetTrackball(); 
	else if(action=='zoomin') presenter.zoomIn(); 
	else if(action=='zoomout') presenter.zoomOut();
	else if(action=='light' || action=='light_on') { presenter.enableLightTrackball(!presenter.isLightTrackballEnabled()); lightSwitch();  
	else if(action=='full' || action=='full_on') fullscreenSwitch(); 
	}

```

### Building Self hosted 3D Viewer

After analysing and comparing the pros and cons of using a commercial hosting platform and self hosted viewer we opted to implement our own viewer , because self hosted viewer comes with advantages like less cost, no limit on size of models, number of models and formats etc. We implemented 3D viewer using Three, a cross browser Javascript library and Application Programming Interface (API) that comes with features of not only creating and designing but also displaying animation, 3D graphics in web browser using WebGL. The code for the 3D viewer is open source and is available [here](https://github.com/Digital-Repository-of-Ireland/DRI-3DViewer).

<p align="center">
<img src="https://raw.githubusercontent.com/Digital-Repository-of-Ireland/DRI-3D-Pilot/master/face_shieldmask2.jpeg" width="600" height="300" /> 
</p>
  <em>View of the Face Shield 3D Model as it appears in the DRI Repository 3D viewer (CC-BY 4.0)  
</em>


<!--
In order to ensure that the 3D object can be rendered and interacted with in a browser environment, an embedded version would be more appropriate.

The [Europeana Publishing Guide](https://pro.europeana.eu/post/publication-policy) indicates that any oEmbed compliant player is supported by Europeana Collections. We are thus investigating an oEmbed option which would allow us to send an embeddable 3D model in the edm:isShownBy field of EDM. This may be accompanied by an additional edm:hasView element which gives the url to download the source file, or a link to the source file may be provided in the embeddable viewer. This will be the next phase of development of this pilot application. --->

### Embedding and Sharing with Oembed :

In order to aggregate the 3D models, The [Europeana Publishing Guide](https://pro.europeana.eu/post/publication-policy) indicates that any oEmbed compliant player is supported by Europeana Collections. We implemented the [oEmbed API](https://oembed.com/) that allows the embedded representation of contents (like photos , videos, rich) when a user  posts a link to that resource, without having to parse the resource directly. In order to comply with security considerations, we as an oEmbed provider shared the embedded snippet in iframes so that consumers can display embeds without any fear of XSS attacks. We configured the API setting as oembed provider and embeddable content link is shared in edm:isShownBy element of EDM feed.


Consumer will send an HTTP request like this:

<li>
https://repository.dri.ie/api/oembed?url=https%3A%2F%2Frepository.dri.ie%2Fcatalog%2Fg732sz11t
</li>



DRI as oEmbed provider response is as:


```json
  {
	"type": "rich",
	"version": "1.0",
	"title": "Model beenah",
	"provider_name": "DRI: Digital Repository of Ireland",
	"provider_url": "https://repository.dri.ie/",
	"html": " <iframe src = ""https://repository.dri.ie/embed3d/sn009x76k/files/hd76s004z"" width="1024px" height="1024px"> </iframe> "
   }
```


### 3D Content Providers

DRI is currently working with [Transport Infrastructure Ireland](https://www.tii.ie/) who are providing content for the 3D Pilot. We are interested in working with other Irish institutes who hold 3D content. More information on joining DRI can be found at our [membership pages](https://www.dri.ie/membership).


